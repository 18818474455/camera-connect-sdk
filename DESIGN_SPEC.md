<!--
  ═══════════════════════════════════════════════════════════════════════
  CAMERA CONNECT SDK · 设计稿规范 (Design Spec)
  for Figma / Photoshop · README 占位图全套规范
  Version 1.0 · 2026
  ═══════════════════════════════════════════════════════════════════════
-->

# Camera Connect SDK · 视觉设计规范

> 本文档为 README 长滚动落地页中所有占位图的**最终设计输出规范**。  
> 设计师可直接按此文档在 Figma / PS 中产出全套视觉资产。  
> 风格基准：**Linear · Vercel · Apple Newsroom**

---

## 目录

1. [品牌基础规范](#1-品牌基础规范)
2. [Figma 文件结构](#2-figma-文件结构)
3. [组件库 (Components)](#3-组件库-components)
4. [占位图清单 (24 张)](#4-占位图清单-24-张)
5. [导出与命名规范](#5-导出与命名规范)
6. [可复用源文件下载链接占位](#6-可复用源文件下载链接占位)

---

## 1. 品牌基础规范

### 1.1 色彩系统

#### 主色 · Neutral Palette

| Token | HEX | RGB | 用途 |
|---|---|---|---|
| `--bg-base` | `#0A0A0A` | 10, 10, 10 | 全局背景（深空黑） |
| `--bg-surface` | `#111111` | 17, 17, 17 | 卡片底色 |
| `--bg-elevated` | `#1A1A1A` | 26, 26, 26 | 悬浮卡片 / 弹层 |
| `--border-subtle` | `#1F1F1F` | 31, 31, 31 | 极弱分割线 |
| `--border-default` | `#2A2A2A` | 42, 42, 42 | 卡片描边 |
| `--text-primary` | `#FFFFFF` | 255, 255, 255 | 标题、巨号字 |
| `--text-secondary` | `#A1A1AA` | 161, 161, 170 | 正文、副标题 |
| `--text-tertiary` | `#52525B` | 82, 82, 91 | 注释、版权、辅助 |

#### 强调色 · Accent Palette

| Token | HEX | 用途 |
|---|---|---|
| `--accent-primary` | `#FFFFFF` | 主按钮、强调元素（反白即视觉重点） |
| `--accent-cyan` | `#00D4FF` | 渐变线、Hero 高光、连接动效 |
| `--accent-violet` | `#7C3AED` | 渐变收尾、按钮 hover 光晕 |
| `--accent-pulse` | `#22C55E` | 在线/绿色脉冲点（实时状态） |
| `--accent-warning` | `#F59E0B` | 限时/促销点缀（极少使用） |

#### 渐变 · Gradients

```
G1 · 顶部渐变线
  linear-gradient(90deg, #00D4FF 0%, #7C3AED 100%)
  高度: 1px · 透明度: 30%

G2 · Hero 光束散射
  radial-gradient(circle at 70% 50%,
    rgba(0,212,255,0.20) 0%,
    rgba(124,58,237,0.10) 40%,
    transparent 70%)

G3 · 卡片 hover 边框
  linear-gradient(135deg, #00D4FF 0%, transparent 50%, #7C3AED 100%)
```

> **配色铁律**：暗色背景占据 90% 以上画面，强调色仅作 1-3% 的点缀使用。**绝不滥用渐变**。

---

### 1.2 字体系统

| 用途 | 字体 | 字重 | 字号 | 行距 | 字间距 |
|---|---|---|---|---|---|
| **Display (Hero)** | Inter / Geist | 700 Bold | 96 / 120 / 144 px | 100% | -3% |
| **H1 模块标题** | Inter / Geist | 600 SemiBold | 56 / 64 px | 110% | -2% |
| **H2 卡片标题** | Inter / Geist | 600 SemiBold | 32 / 40 px | 120% | -1% |
| **Body Large** | Inter | 400 Regular | 18 / 20 px | 150% | 0% |
| **Body** | Inter | 400 Regular | 16 px | 160% | 0% |
| **Caption** | Inter | 400 Regular | 13 px | 150% | +1% |
| **Mono / Code** | JetBrains Mono / Geist Mono | 500 Medium | 14 px | 140% | 0% |
| **数字 (Big Stat)** | Inter / Geist | 700 Bold | 80 / 96 px | 100% | -4% |

#### 中文字体回退栈

```
font-family:
  "Inter", "Geist",
  -apple-system, "PingFang SC",
  "HarmonyOS Sans SC", "Source Han Sans SC",
  "Noto Sans CJK SC", sans-serif;
```

---

### 1.3 间距与栅格

#### 基础单位

```
Base unit: 4px
Spacing scale: 4 / 8 / 12 / 16 / 24 / 32 / 48 / 64 / 96 / 128 / 192 px
```

#### 响应式断点

| 断点 | 宽度 | 内边距 | 栅格 | 列间距 |
|---|---|---|---|---|
| Mobile | 375 - 767 | 24 px | 4 cols | 16 px |
| Tablet | 768 - 1023 | 48 px | 8 cols | 24 px |
| Desktop | 1024 - 1439 | 96 px | 12 cols | 32 px |
| Large | 1440 - 1919 | 128 px | 12 cols | 32 px |
| **XL (设计基准)** | **1920** | 160 px | 12 cols | 32 px |

#### 模块垂直节奏

```
模块上下边距：128 px (大屏) / 80 px (移动)
模块标题与内容间距：48 px
卡片内边距：32 - 48 px
卡片间距：24 px
```

---

### 1.4 圆角与阴影

#### Radius Scale

| Token | Value | 用途 |
|---|---|---|
| `r-xs` | 4 px | Badge、标签 |
| `r-sm` | 8 px | 输入框、按钮 |
| `r-md` | 12 px | 卡片 |
| `r-lg` | 16 px | 大卡片 / 弹层 |
| `r-xl` | 24 px | Hero 视觉块 |
| `r-full` | 9999 px | 头像 / 圆形按钮 |

#### Shadow（暗色场景慎用）

```
S1 · 卡片悬浮（极淡）
  box-shadow: 0 0 0 1px rgba(255,255,255,0.06),
              0 8px 24px rgba(0,0,0,0.40);

S2 · Hero 光晕
  box-shadow: 0 0 120px rgba(0,212,255,0.15);
```

---

### 1.5 噪点纹理（关键质感）

> Linear / Vercel 风格的灵魂：**所有暗色背景必须叠加一层 0.5-1% 的灰度噪点**，防止纯黑发腻。

```
图层名: NOISE
导出: noise.png (256×256 平铺)
混合模式: Overlay
不透明度: 4-8%
```

PS 制作方式：滤镜 → 杂色 → 添加杂色（数量 30，高斯分布，单色）→ 高反差保留 1px

---

## 2. Figma 文件结构

```
📁 Camera Connect SDK · README Visual Kit
│
├── 🎨 00 · Design Tokens
│   ├── Colors
│   ├── Typography
│   ├── Spacing & Grid
│   └── Effects (Shadow / Blur / Noise)
│
├── 🧩 01 · Components
│   ├── Buttons (Primary / Secondary / Ghost)
│   ├── Badges
│   ├── Cards (Story / Stat / Testimonial / Scenario)
│   ├── Logos & Marks
│   ├── Avatars
│   └── Icons (Stroke 1.5px)
│
├── 🖼  02 · Master Artboards (24)
│   ├── 01 · Hero Banner [1920×680]
│   ├── 02 · Logo Bar [1920×120]
│   ├── 03 · Showcase GIF Frames [1080×720]
│   ├── ...
│   └── 24 · CTA Banner [1920×400]
│
├── 📦 03 · Export Ready
│   ├── @1x · PNG
│   ├── @2x · PNG (Retina)
│   ├── SVG (Logos / Icons)
│   └── GIF / MP4 (动效)
│
└── 📚 04 · Reference Board
    ├── Linear inspiration
    ├── Vercel inspiration
    ├── Apple Newsroom inspiration
    └── Color mood board
```

---

## 3. 组件库 (Components)

### 3.1 Button

| 类型 | 背景 | 文字 | 边框 | 内边距 | 圆角 |
|---|---|---|---|---|---|
| **Primary** | `#FFFFFF` | `#000000` 600 | 无 | 14×24 px | 8 px |
| **Secondary** | 透明 | `#FFFFFF` 500 | `1px #2A2A2A` | 14×24 px | 8 px |
| **Ghost** | 透明 | `#A1A1AA` 500 | 无 | 12×16 px | 6 px |

**hover 状态**：Primary 加 `#F4F4F5` 背景、Secondary 边框变 `#FFFFFF`。

---

### 3.2 Badge

```
高度: 28 px · 内边距: 8×12 px · 圆角: 6 px
背景: rgba(255,255,255,0.05)
边框: 1px rgba(255,255,255,0.10)
文字: 12 px / 500 / #A1A1AA / letter-spacing 1%

变体:
  · Default · Success (绿色边框) · Pulse (绿色脉冲)
```

---

### 3.3 Card · 故事卡片

```
尺寸: 632 × 480 px (2 列布局)
背景: #111111
边框: 1px #1F1F1F
圆角: 16 px
内边距: 48 px

结构:
  ┌────────────────────────────────┐
  │  01.   ←  Mono · 14px · Cyan   │
  │                                │
  │  巨号标题                       │
  │  ←  H2 · 32px · White · 600    │
  │                                │
  │  正文描述...                    │
  │  ←  Body · 16px · #A1A1AA      │
  │                                │
  │  [视觉占位区 280px]             │
  │                                │
  │  ─────────────────────────     │
  │  ▸ 已被 2,000 家使用            │
  └────────────────────────────────┘
```

---

### 3.4 Card · 数字卡片

```
布局: 居中文字 · 无背景
数字: Display · 96px · #FFFFFF
副标题: 16px · 500 · #FFFFFF
说明: 14px · 400 · #A1A1AA · 居中
```

---

### 3.5 Card · 证言卡片

```
尺寸: 408 × 240 px
背景: #111111
边框: 1px #1F1F1F
圆角: 12 px
内边距: 32 px

结构:
  · 引号符号（28px · #2A2A2A）
  · 引文（18px · 500 · #FFFFFF · 行距 150%）
  · 头像 40×40 (radius full) + 姓名 + 身份
```

---

### 3.6 Avatar

```
默认尺寸: 40 / 56 / 80 px
形状: 圆形
描边: 1px rgba(255,255,255,0.10)
fallback: 灰色 #2A2A2A 背景 + 首字母白色
```

---

### 3.7 Icon

```
线条: Stroke 1.5px · 无填充
尺寸: 16 / 20 / 24 px
颜色: 默认 #A1A1AA · hover #FFFFFF
推荐图标库: Lucide / Phosphor (Light)
```

---

## 4. 占位图清单 (24 张)

> 每一张都对应 README_LANDING.md 中的 `[ ... ]` 占位标记。  
> 按 README 顺序排列，可直接对照制作。

---

### 🖼 P01 · HERO BANNER

| 项 | 规格 |
|---|---|
| **尺寸** | 1920 × 680 px @2x |
| **背景** | `#0A0A0A` + Noise Overlay 6% |
| **顶部** | 1px 渐变线 G1（透明度 30%） |
| **左侧 60% 内容** | 巨号字 "Connect Anything." · 144px · White · 700 · 居左 |
| **右侧 40% 视觉** | 抽象 3D 渲染：USB 接口 → 光束 → 散射成相机轮廓 |
| **3D 渲染参数** | Cinema 4D / Blender · Subtle Cyan→Violet glow · 黑色金属反射 |
| **底部** | 副标题 + Badge 横排 + 3 个 CTA |
| **导出** | `01-hero.png` `01-hero@2x.png` |

---

### 🖼 P02 · TRUST BAR (LOGO 滚动)

| 项 | 规格 |
|---|---|
| **尺寸** | 1920 × 120 px |
| **背景** | `#0A0A0A` |
| **Logo 数量** | 16 个 · 单行横向滚动 |
| **Logo 处理** | 全部转 `#52525B` 单色 · 高度统一 32 px · 间距 96 px |
| **Hover 效果** | 鼠标 hover 时单个 logo 恢复彩色 + 微缩放 1.05 |
| **动画** | 30s 循环 · 匀速 · 鼠标 hover 暂停 |
| **导出** | `02-logos.svg`（推荐 SVG 矢量） |

---

### 🖼 P03 · SHOWCASE 01 · 流畅传输 GIF

| 项 | 规格 |
|---|---|
| **尺寸** | 1080 × 720 px @2x |
| **格式** | GIF · 5 秒循环 · ≤ 2MB |
| **场景** | 暗色 UI 中快速滑动浏览高清照片瀑布流 |
| **关键帧** | 0s 静止 → 1s 开始滑动 → 3s 加载新一批 → 5s 回到顶部 |
| **UI 风格** | iOS 暗色 / Material Dark · 圆角 16px 缩略图 |
| **导出** | `03-showcase-fluid.gif` `03-showcase-fluid.mp4` |

---

### 🖼 P04 · SHOWCASE 02 · 拍即所得 GIF

| 项 | 规格 |
|---|---|
| **尺寸** | 1080 × 720 px @2x |
| **格式** | GIF · 4 秒循环 |
| **场景** | 左右双窗：左为单反取景 + 快门动画，右为手机大屏接收 |
| **关键帧** | 0s → 1s 按下快门（红圈闪一下）→ 1.3s 手机大屏出现照片 |
| **导出** | `04-showcase-instant.gif` |

---

### 🖼 P05 · SHOWCASE 03 · 可靠运行（静态）

| 项 | 规格 |
|---|---|
| **尺寸** | 1080 × 720 px @2x |
| **背景** | `#0A0A0A` 渐变到 `#111111` 顶光 |
| **主体** | 居中环形进度指示器（圆环 480px · 描边 8px · 95% 进度填满） |
| **环形颜色** | 描边渐变 G1 · 内部黑色 |
| **环内文字** | "99.97%" · 96px White · 下方 "稳定运行" 16px Gray |
| **背景装饰** | 微弱网格线（透明度 5%） |
| **导出** | `05-reliability.png` |

---

### 🖼 P06 · SHOWCASE 04 · 全品牌矩阵（静态）

| 项 | 规格 |
|---|---|
| **尺寸** | 1080 × 720 px @2x |
| **布局** | 4 个相机品牌 logo 居中等距排列（2×2 网格） |
| **Logo 尺寸** | 单个 240×120 px · 灰度 `#A1A1AA` |
| **品牌** | Canon · Nikon · Sony · Panasonic |
| **每个 logo 下方** | 8px 圆点 · `#22C55E` · 慢呼吸脉冲 2s |
| **背景** | `#0A0A0A` + 极淡网格 |
| **导出** | `06-brands.png` |

---

### 🖼 P07-09 · ROLES · 三种角色场景图

| ID | 主题 | 场景描述 |
|---|---|---|
| **P07** | 摄影师 | 暗色环境，摄影师与客户共看一台 iPad，平板上显示刚拍的照片 |
| **P08** | 直播方 | 直播间多机位画面墙，主机位单反镜头特写，环境光柔和 |
| **P09** | 影像 App | 多个手机 App 截图悬浮拼贴，3D 透视摆放 |

| 项 | 规格 |
|---|---|
| **尺寸** | 632 × 400 px @2x（每张） |
| **风格** | 真实摄影 / Midjourney 暗调写实 |
| **色调** | 整体压暗 · 仅高光区域保留细节 · 微 Cyan 偏色 |
| **后期** | 加 Noise Overlay 5% · 边缘渐隐 |
| **导出** | `07-role-photographer.png` 等 |

---

### 🖼 P10-19 · 十个应用场景大图

> 每张配套**主图 + 一张极小客户 logo 角标**。

| ID | 场景 | 视觉描述 | 尺寸 |
|---|---|---|---|
| **P10** | 婚礼现场 | 暗色调婚礼晚宴 + 一台平板正在实时滚动出片 | 720×540 |
| **P11** | 时装秀场 | 暗色 T 台 + 后台摄影师手机大屏涌入实时高清照 | 720×540 |
| **P12** | 体育赛事 | 暗色球场夜景 + 看台上记者笔记本图片正快速刷新 | 720×540 |
| **P13** | 电商直播 | 暗色直播间 + 多机位画面墙 + 单反镜头特写 | 720×540 |
| **P14** | 新闻发布会 | 暗色发布会舞台 + 媒体席手机海洋 + 发光焦点 | 720×540 |
| **P15** | 影楼写真 | 暗色影楼 + 客户在大屏前选片 + 摄影师同步在拍 | 720×540 |
| **P16** | 演唱会 | 暗色演唱会舞台 + 万人荧光棒 + 大屏显示刚拍照片 | 720×540 |
| **P17** | 地产豪宅 | 暗色豪宅夜景 + iPad 上 360° 看房界面 | 720×540 |
| **P18** | 户外探险 | 暗色雪山日落 + 一只手套握着相机 + 远处帐篷透出灯光 | 720×540 |
| **P19** | 摄影教学 | 暗色教室 + 投影大屏正在实时显示老师相机拍出的样片 | 720×540 |

| 项 | 通用规格 |
|---|---|
| **格式** | PNG @2x（如条件允许，提供 WEBM 微动效版本） |
| **统一处理** | 暗色调 + Cyan 偏色 + 边角渐隐 + Noise 5% |
| **来源建议** | Midjourney v7 + 关键词 `cinematic, dark, moody, dramatic lighting, --ar 4:3` |
| **MJ Prompt 模板** | `[场景描述], dark cinematic, moody atmosphere, subtle cyan rim light, photographic, 4:3 aspect ratio --style raw --v 7` |
| **导出** | `10-scenario-wedding.png` 至 `19-scenario-teaching.png` |

---

### 🖼 P20 · GLOBAL MAP · 全球客户分布

| 项 | 规格 |
|---|---|
| **尺寸** | 1600 × 720 px @2x |
| **背景** | `#0A0A0A` |
| **地图** | 简化暗色世界地图 · 国家边界线 0.5px `#1F1F1F` |
| **节点** | 38 个城市光点 · 半径 4-8px · `#00D4FF` |
| **节点动效** | 慢呼吸脉冲 + 向外扩散光环 3s 循环 |
| **连接线** | 主要城市间细弧线 · 渐变 G1 · 透明度 30% · 流光动画 |
| **底部数据** | 5 个数字横排（已在 README 中） |
| **推荐工具** | D3.js / Mapbox Studio Dark 主题 / Figma Map plugin |
| **导出** | `20-global-map.png` + `20-global-map.lottie` |

---

### 🖼 P21 · WALL OF LOVE · 头像集

| 项 | 规格 |
|---|---|
| **数量** | 6 个头像（对应 6 条证言） |
| **尺寸** | 80 × 80 px @2x · 圆形 |
| **风格** | 真实人像（黑白 + 微 Cyan 高光） |
| **Fallback** | 若无真实照片，使用 [Boring Avatars](https://boringavatars.com/) `marble` 模式 |
| **导出** | `21-avatar-01.png` ~ `21-avatar-06.png` |

---

### 🖼 P22 · MEDIA LOGOS · 媒体引语 logo

| 项 | 规格 |
|---|---|
| **数量** | 4 个媒体 logo |
| **尺寸** | 单个 120 × 32 px |
| **处理** | 灰度 `#52525B` · SVG 矢量 |
| **导出** | `22-media-01.svg` ~ `22-media-04.svg` |

---

### 🖼 P23 · DEMO VIDEO THUMBNAIL

| 项 | 规格 |
|---|---|
| **尺寸** | 1280 × 720 px @2x |
| **背景** | 一帧产品演示画面 + 暗色蒙版 60% |
| **中央** | 96px 圆形播放按钮（白色边框 + 三角形） |
| **左下** | 视频时长徽章 "3:24" |
| **导出** | `23-video-thumbnail.png` |

---

### 🖼 P24 · FOUNDER PORTRAIT

| 项 | 规格 |
|---|---|
| **尺寸** | 600 × 600 px @2x |
| **风格** | 黑白肖像 · 极简背景 · 侧光 |
| **后期** | 高对比 · 颗粒感 · 微 warm tone |
| **导出** | `24-founder.jpg` |

---

### 🖼 P25 · CTA BANNER（最后召唤）

| 项 | 规格 |
|---|---|
| **尺寸** | 1920 × 400 px @2x |
| **背景** | `#0A0A0A` + Noise + 顶部光晕 G2 |
| **主标题** | 巨号字 96px White · 居中两行 |
| **按钮** | 2 个 Primary · 间距 24px |
| **底部** | 三段灰色小字 · 14px · `#52525B` |
| **导出** | `25-cta.png` |

---

## 5. 导出与命名规范

### 5.1 命名规则

```
{编号两位}-{模块英文名}-{变体}.{格式}

✓ 正例:
  01-hero.png
  03-showcase-fluid.gif
  10-scenario-wedding.png
  21-avatar-01.png

✗ 反例:
  HeroBanner_Final_v3.png
  scenario1.jpg
```

### 5.2 输出倍率

| 用途 | 倍率 | 格式 |
|---|---|---|
| README 网页展示 | @2x | PNG / WEBP |
| 矢量元素（logo / icon） | - | SVG |
| 动效 | - | GIF (< 2MB) + MP4 fallback |
| 复杂动画 | - | Lottie JSON |
| 移动端缩略图 | @1x | WEBP |

### 5.3 体积控制

```
单张静态图 ≤  500 KB
单张 GIF    ≤    2 MB
所有资产合计 ≤   30 MB
```

> **优化建议**：使用 [Squoosh](https://squoosh.app/) 压缩 PNG → WEBP，使用 [Ezgif](https://ezgif.com/) 压缩 GIF。

### 5.4 文件夹结构（交付）

```
📦 camera-connect-readme-assets/
├── 📁 01-hero/
│   ├── 01-hero.png
│   ├── 01-hero@2x.png
│   └── 01-hero.webp
├── 📁 02-logos/
│   └── 02-logos.svg
├── 📁 03-09-showcase-roles/
├── 📁 10-19-scenarios/
├── 📁 20-global-map/
├── 📁 21-25-others/
├── 📁 source/
│   └── design-source.fig (Figma 源文件)
└── README-INDEX.md
```

---

## 6. 可复用源文件下载链接占位

> 设计师完成后，请将以下链接补全：

| 资源 | 链接 |
|---|---|
| 🎨 **Figma 源文件** | `[ TBD · figma.com/file/xxxxx ]` |
| 🖼 **Photoshop 源文件 (PSD)** | `[ TBD · 网盘链接 ]` |
| 🎬 **After Effects 动效源** | `[ TBD ]` |
| 🌐 **3D Hero 渲染源 (Blender)** | `[ TBD ]` |
| 📦 **完整资产包 (ZIP)** | `[ TBD · GitHub Release ]` |

---

## 7. 设计验收 Checklist

交付前请逐项打勾：

- [ ] 所有占位图均使用 `#0A0A0A` 主背景，整体调性统一
- [ ] 所有暗色背景叠加 4-8% 噪点纹理
- [ ] Hero 顶部 1px 渐变线 G1 已添加（透明度 30%）
- [ ] 所有数字使用 Inter / Geist 700 + 负字间距
- [ ] 所有客户 logo 已转灰度 `#52525B`
- [ ] 所有按钮符合 3.1 规范（Primary 反白）
- [ ] 所有卡片圆角 12-16 px，描边 1px `#1F1F1F`
- [ ] 所有 GIF ≤ 2MB，提供 MP4 fallback
- [ ] 所有 PNG 已通过 WEBP 压缩
- [ ] 资产命名严格遵循 `{编号}-{模块}.{ext}`
- [ ] Figma 文件结构清晰，组件已发布到团队库
- [ ] README-INDEX.md 已生成，标注每张图对应的位置

---

## 8. AI 生成工具推荐

| 工具 | 用途 | 推荐 Prompt 风格 |
|---|---|---|
| **Midjourney v7** | 10 个场景大图 | `cinematic, dark moody, subtle cyan rim, --ar 4:3 --style raw` |
| **Stable Diffusion XL + Juggernaut** | 头像 / 创始人肖像 | `black and white portrait, dramatic side lighting, film grain` |
| **Runway Gen-3** | 动态背景视频 | `slow zoom, dark abstract, particle flow, 5s loop` |
| **Spline / Blender** | Hero 3D 视觉 | USB 接口 + 光束 + 相机散射 |
| **Lottie Files** | Map 动效 | 城市脉冲点 + 流光连线 |
| **Figma + Anima** | 高保真原型 → 代码 | 一键导出 React/Vue 组件 |

---

## 9. 风格基准参考链接

| 名称 | 链接 | 借鉴点 |
|---|---|---|
| **Linear** | https://linear.app | 暗色基底、字体节奏、3D Hero |
| **Vercel** | https://vercel.com | 全黑布局、噪点质感、CTA 按钮 |
| **Apple Newsroom** | https://www.apple.com/newsroom/ | 巨号标题 + 极致留白 |
| **Stripe Sessions** | https://stripe.com/sessions | 卡片节奏 + 渐变高光 |
| **Framer** | https://framer.com | 动效与微交互 |
| **Geist Design System** | https://vercel.com/geist | 字体 / 组件参考 |

---

<div align="center">

<br/>

**Design Spec · v1.0 · 2026**

<sub>Built with obsession for the next generation of imaging.</sub>

<br/>
<br/>

</div>
