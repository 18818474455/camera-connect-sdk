# Contributing to Camera Connect SDK

> **First time contributor?** Welcome! This guide will get you set up in 5 minutes.

We love every kind of contribution — code, docs, design, bug reports, feature ideas, even just a kind word in [Discussions](https://github.com/18818474455/camera-connect-sdk/discussions).

---

## 🐞 Found a bug?

Please open a [Bug Report](https://github.com/18818474455/camera-connect-sdk/issues/new?template=bug_report.yml) — the template walks you through everything we need (SDK version, camera model, repro steps, logs).

> **Tip:** include `adb logcat` filtered to your app's package — this saves us hours.

## ✨ Have an idea?

For small, well-scoped ideas → [Feature Request](https://github.com/18818474455/camera-connect-sdk/issues/new?template=feature_request.yml)

For "what if...?" or open-ended thinking → [Discussions › Ideas](https://github.com/18818474455/camera-connect-sdk/discussions/categories/ideas)

## 🆕 Want a new camera supported?

Open a [Feature Request](https://github.com/18818474455/camera-connect-sdk/issues/new?template=feature_request.yml) with:
- Brand + exact model + firmware version
- A photo of the USB port (helps us verify the protocol variant)
- Whether you can lend us a body or remote-test

We aim to add new bodies **within 24 hours** of a public release.

## 📚 Docs / README / design tweaks

These are first-class contributions. Just open a PR with the change — no issue needed.

- Markdown / asset changes only? → no test plan required
- Adds a new section? → please show a before/after screenshot in the PR

## 💻 Code contributions

1. **Discuss first** — for non-trivial changes, please open an issue or Discussion before writing code, so we can align on direction.
2. **Branch** — create your feature branch from `main`:
   ```bash
   git checkout -b feature/short-name
   ```
3. **Conventional commits** — please use the [Conventional Commits](https://www.conventionalcommits.org/) format:
   ```
   feat(android): add Sony A1 detection
   fix(ios): handle USB hotplug race condition
   docs(readme): clarify pairing flow
   ```
4. **Open a PR** — fill out the PR template, link any related issues.
5. **CI must pass** — CodeQL + link check + (later) tests.

## 🧪 Testing your change

For the demo app:
```bash
flutter pub get
flutter run -d <your-device>
```

For SDK changes, please test on **at least one Canon, one Sony, and one Nikon body** if available — the protocol behavior diverges enough between brands that single-vendor testing often misses bugs.

## 🎨 Showcase your work

Built something with Camera Connect SDK? We'd love to see it!

→ Open a [Showcase Discussion](https://github.com/18818474455/camera-connect-sdk/discussions/new?category=showcase) with the template.

We feature great showcases in:
- The README's "Wall of Love" section
- Our quarterly newsletter
- Twitter / LinkedIn (with your permission)

## 📜 Code of Conduct

Be kind. Be specific. Assume good intent. We follow the [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/) — TL;DR: don't be a jerk, especially to first-time contributors.

## 📄 License

By contributing, you agree that your contributions will be licensed under the same license as the project.

---

**Questions?** Open a [Discussion](https://github.com/18818474455/camera-connect-sdk/discussions) — we're here to help.
