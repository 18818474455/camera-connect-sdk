# Android 相机直连 SDK 实战：从 USB 识别到照片同步的工程链路

做影像类 App 时，很多人一开始会把“相机连接”理解成一个很简单的功能：插上相机，读取照片，再同步到手机。

真正落地之后会发现，它更像一条完整的工程链路：设备识别、USB 权限、会话保持、相机文件索引、RAW/JPG 筛选、传输队列、断线恢复、进度回调、缓存管理、UI 状态同步，每一层都可能影响最终体验。

最近整理了一个相机连接方向的项目资料：

项目地址：https://github.com/18818474455/camera-connect-sdk

这篇文章不做夸张宣传，主要从 Android 工程接入角度，聊聊一个“相机直连 SDK”应该怎么拆，哪些地方最容易踩坑，以及为什么这类能力对婚礼跟拍、棚拍、赛事、发布会、图片直播、教学演示等场景很有价值。

## 1. 相机连接不是“读取文件”这么简单

如果只是读取手机本地相册，逻辑相对直接：拿系统媒体库权限，查询图片，展示列表即可。

但专业相机接入移动端时，链路会复杂很多：

```text
相机接入
  -> 设备发现
  -> 权限申请
  -> 建立会话
  -> 扫描相机存储
  -> 生成文件索引
  -> 判断 JPG / RAW / 视频等类型
  -> 加入传输队列
  -> 同步到本地缓存
  -> 通知业务层刷新 UI
  -> 失败重试 / 断开恢复
```

这也是为什么相机连接更适合封装成 SDK，而不是让每个业务 App 都从底层通信开始写。

`camera-connect-sdk` 这个项目展示的方向，核心可以概括为几件事：

- USB 直连相机，减少传统导卡和人工传图流程
- 支持 JPG、RAW 等不同类型素材的同步策略
- 支持图片直播和标准同步两种使用方式
- 支持多选、条件筛选和批量传输
- 在 UI 上展示全部、同步中、已完成、失败等进度状态
- 面向 Canon、Nikon、Sony、Panasonic 等主流相机品牌做兼容适配

对于开发者来说，重点不是界面有多炫，而是底层状态是否稳定，业务回调是否清晰，异常场景能不能兜住。

## 2. 为什么很多团队需要相机直连能力

相机直连移动端的价值，主要体现在“现场效率”。

比如婚礼摄影场景，摄影师拍完一组照片后，助理可以在移动端快速挑图、预览或同步给修图流程。传统做法往往需要拔卡、读卡、导入电脑，再分发给后续人员，中间步骤多，现场不可控。

棚拍和商业拍摄也类似。模特、摄影师、客户、修图师都在现场，大家希望照片在拍摄过程中就能被看到，而不是等一轮拍摄结束之后再集中处理。

赛事、发布会和媒体采编对时效要求更高。照片慢几分钟，就可能错过第一波传播窗口。相机直连能力可以把“拍摄”和“分发”之间的距离压缩到更短。

所以这类 SDK 解决的不是单纯技术问题，而是影像工作流问题：

- 摄影师不想频繁换卡和导文件
- 现场客户希望实时看到样片
- 运营团队希望更快拿到可用素材
- App 开发者希望把专业相机能力集成到自己的产品里

## 3. SDK 可以怎么分层

一个相机连接 SDK，我会建议至少拆成下面几层。

```text
业务 App
  -> UI Adapter
  -> Camera Connect SDK
       -> DeviceDiscovery
       -> PermissionSession
       -> CameraFileIndex
       -> TransferQueue
       -> LocalCache
       -> SyncReporter
  -> Android USB / Storage / Network
```

### DeviceDiscovery：发现设备

设备发现层负责监听相机接入和拔出。

Android 侧通常需要处理 USB Host、设备过滤、广播监听、前台页面生命周期等问题。这里最容易出错的是：相机拔掉后 UI 还显示连接中，或者用户重新插入后 SDK 没有恢复到可用状态。

建议设备状态至少抽象成：

```kotlin
sealed class CameraDeviceState {
    data object Detached : CameraDeviceState()
    data class Attached(val deviceName: String) : CameraDeviceState()
    data class PermissionRequired(val deviceName: String) : CameraDeviceState()
    data class Ready(val deviceName: String, val model: String?) : CameraDeviceState()
    data class Error(val message: String) : CameraDeviceState()
}
```

业务层只订阅状态，不直接碰底层 USB 细节。

### PermissionSession：管理权限和连接会话

相机连接不是一次性动作，而是一个会话。

建立会话后，SDK 需要知道：

- 当前相机是否仍然在线
- 权限是否还有效
- 文件列表是否已经扫描
- 是否有传输任务正在执行
- 页面销毁时是否需要释放资源

这层如果没有封装好，业务代码会到处散落 `requestPermission`、`openDevice`、`closeSession`，后期维护会很痛苦。

更理想的方式是让业务侧只感知类似这样的接口：

```kotlin
interface CameraConnectSession {
    suspend fun scanFiles(filter: CameraFileFilter): List<CameraAsset>
    fun observeTransfer(): Flow<TransferEvent>
    suspend fun enqueue(files: List<CameraAsset>, mode: TransferMode)
    suspend fun close()
}
```

这里的代码只是说明设计思路，具体 API 以实际 SDK 交付为准。

### CameraFileIndex：给相机文件建立索引

很多相机里的文件并不是简单的“按时间排序读取列表”。

真实场景里会遇到：

- 同一张照片同时存在 JPG 和 RAW
- 文件数量很多，首次扫描时间较长
- 用户拍摄过程中仍在产生新文件
- 相机目录结构因品牌或设置不同而变化
- 文件名不能完全代表拍摄顺序

所以 SDK 需要一层文件索引，而不是每次都全量扫描。

一个常见做法是把相机侧文件抽象成统一模型：

```kotlin
data class CameraAsset(
    val id: String,
    val name: String,
    val path: String,
    val type: AssetType,
    val size: Long,
    val capturedAt: Long?,
    val groupId: String?
)

enum class AssetType {
    JPG,
    RAW,
    VIDEO,
    OTHER
}
```

`groupId` 可以用来关联同一次快门产生的 JPG 和 RAW。这样业务层就可以实现“只看 JPG”“只同步 RAW”“JPG 预览、RAW 后续处理”等策略。

### TransferQueue：传输必须队列化

相机文件传输最怕两件事：阻塞 UI 和状态混乱。

如果用户一次选择几十张甚至几百张照片，SDK 不能把所有任务都同时扔出去。更合理的方式是做一个可观察的传输队列：

```kotlin
enum class TransferState {
    Pending,
    Running,
    Success,
    Failed,
    Canceled
}

data class TransferTask(
    val asset: CameraAsset,
    val state: TransferState,
    val progress: Int,
    val errorMessage: String? = null
)
```

UI 展示“全部 / 同步中 / 已完成 / 失败”时，本质上就是对这批任务状态做过滤。

这也是 `camera-connect-sdk` 项目资料里比较值得关注的一点：它不是只强调“能连上”，而是把传输过程本身当成用户体验的一部分。

## 4. 两种典型模式：图片直播与标准同步

相机连接类产品常见两种模式。

第一种是图片直播模式。

它更关注“新照片尽快出现”。适合活动现场、发布会、婚礼快修、赛事采编等场景。此时 SDK 要尽量缩短从相机产生新文件到 App 看到文件的时间，并且在 UI 上持续刷新最新内容。

第二种是标准同步模式。

它更关注“选定文件稳定完成”。适合棚拍、教学、归档、批量整理等场景。此时 SDK 要更重视完整队列、失败重试、缓存校验和任务结果统计。

两种模式对底层能力要求不同：

| 能力 | 图片直播 | 标准同步 |
| --- | --- | --- |
| 目标 | 更快看到新图 | 更稳完成任务 |
| 文件选择 | 自动监听新增 | 手动多选或条件筛选 |
| UI 重点 | 最新照片、实时状态 | 队列进度、失败原因 |
| 异常处理 | 断开后继续监听 | 断点续传、批量重试 |
| 适合场景 | 婚礼、发布会、赛事 | 棚拍、教学、素材整理 |

如果要做成 SDK，最好不要把这两种模式写死在 UI 层，而是抽象成 `TransferMode`：

```kotlin
enum class TransferMode {
    Live,
    Batch
}
```

业务 App 根据自己的场景选择即可。

## 5. 接入时最容易踩的 10 个坑

### 1）只处理插入，不处理拔出

很多 Demo 能跑，是因为只覆盖了“插入相机 -> 读取照片”的 happy path。

正式场景里，用户可能随时拔线、换机、重插。如果 SDK 没有稳定的状态机，很容易出现页面假连接、队列卡死、资源未释放等问题。

### 2）把权限申请写在 Activity 里

如果权限逻辑和页面强绑定，横竖屏切换、页面返回、重新进入都可能打断连接流程。更好的方式是 SDK 内部维护会话状态，UI 只负责触发和展示。

### 3）没有区分 JPG 和 RAW

RAW 文件体积更大，传输耗时更长。图片直播场景可以先拿 JPG 预览，后续再处理 RAW。若不区分类型，用户会觉得“怎么一直在转”。

### 4）每次都全量扫描

相机里文件多的时候，全量扫描体验会很差。可以用最近扫描位置、文件时间、对象 ID 或本地索引做增量策略。

### 5）进度状态太粗

只给一个 loading 很难定位问题。建议至少区分：等待中、同步中、成功、失败、已取消。失败原因也要能透出，方便用户决定重试还是跳过。

### 6）没有失败重试

相机连接场景不可能永远稳定。线材、接口、电量、设备休眠都会影响传输。失败重试、任务恢复和错误提示是必须项。

### 7）主线程做重活

扫描文件、解析元数据、写缓存、传输大文件都不应该阻塞主线程。SDK 对外可以给 Flow、Callback 或事件总线，内部用协程/线程池处理耗时任务。

### 8）缓存没有边界

图片和 RAW 文件很容易占满手机存储。SDK 应该提供缓存目录、最大占用、清理策略，至少让业务方能接管。

### 9）忽略品牌和机型差异

不同相机品牌、不同固件版本的行为可能不同。不要把某一台测试机的结果当成所有机型都成立。SDK 层要留出能力检测和兼容分支。

### 10）UI 没有表达“当前能做什么”

相机连接是强状态功能。用户需要明确知道：还没插相机、等待授权、已连接、正在扫描、正在同步、同步失败、相机已断开。状态表达越清楚，用户越少困惑。

## 6. 一个比较舒服的业务接入方式

如果让我设计业务侧接入，我希望代码大概长这样：

```kotlin
class CameraViewModel(
    private val cameraSdk: CameraConnectSdk
) : ViewModel() {

    val deviceState = cameraSdk.observeDeviceState()
        .stateIn(viewModelScope, SharingStarted.WhileSubscribed(), CameraDeviceState.Detached)

    val transferEvents = cameraSdk.observeTransferEvents()

    fun connect() {
        viewModelScope.launch {
            cameraSdk.openSession()
        }
    }

    fun syncSelected(files: List<CameraAsset>) {
        viewModelScope.launch {
            cameraSdk.currentSession()
                ?.enqueue(files, TransferMode.Batch)
        }
    }
}
```

页面层只做三件事：

- 订阅设备状态
- 展示文件列表和传输进度
- 触发连接、筛选、同步、重试等用户操作

底层连接、权限、扫描和队列细节交给 SDK 处理。这样的边界比较清楚，也更适合后续扩展到不同业务形态。

## 7. 适合哪些业务集成

从场景上看，相机直连 SDK 适合下面几类产品：

- 影楼和摄影工作室 App
- 婚礼、会议、活动现场交付系统
- 媒体采编和赛事摄影工具
- 电商直播和商品拍摄工作流
- 摄影教学、拍摄演示、作品评审工具
- 企业内部素材采集和归档系统

这些场景有一个共同点：照片不是“拍完以后再慢慢处理”，而是现场流程的一部分。

## 8. 评估一个相机连接 SDK，看这些点就够了

如果你正在选型或准备自己封装，可以按下面的清单评估：

- 是否能稳定发现相机接入和断开
- 是否把 USB 权限和连接会话封装清楚
- 是否支持主流相机品牌和常见机型
- 是否能区分 JPG、RAW、视频等文件类型
- 是否支持多选、条件筛选和批量任务
- 是否有传输进度、失败原因和重试机制
- 是否支持图片直播和标准同步两类模式
- 是否有缓存控制和存储清理策略
- 是否能在页面生命周期变化时保持状态一致
- 是否有清晰的业务层 API，而不是把底层细节暴露给 App

## 总结

相机连接 SDK 的价值，不是简单把相机照片“搬到手机里”，而是把专业拍摄现场的工作流接进移动端。

对 Android 开发者来说，最关键的是把设备状态、权限会话、文件索引、传输队列和 UI 展示拆清楚。只要这几层边界稳定，后面无论是做婚礼快修、棚拍预览、赛事传图，还是影像类 App 的专业能力扩展，都会更顺。

项目资料可以参考：

https://github.com/18818474455/camera-connect-sdk

维护者标识：cylbaw
