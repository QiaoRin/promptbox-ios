# PromptBox · iOS 套壳工程

把本地单文件 `PromptBox` 提示词盒子离线内嵌进一个 WKWebView 壳，编译出可自签安装的无签名 IPA。
本工程不含预编译可执行文件，**不能在本地直接压成 IPA**，需经 GitHub Actions 的 macOS runner 云端编译（无需 Mac、无需 $99 开发者账号）。

## 出包步骤

1. 把本目录所有内容（保持目录结构）推送到你自己的 **public** GitHub 仓库 `main` 分支。
2. 仓库的 **Actions** 标签页 → 选择 `Build IPA` → `Run workflow`（或 push 到 main 自动触发）。
3. 构建完成后，在本次运行的 **Artifacts** 里下载 `PromptBox-unsigned-ipa`，解压得到 `PromptBox-unsigned.ipa`。
4. 用自签工具（Sideloadly / AltStore / 全能签 / TrollStore 等）对该 IPA 签名后安装到手机。

> 打 tag（如 `v1.0`）push 后，工作流会自动把 IPA 发到 Release，直链可直接喂给签名工具。

## 常用修改位置

| 想改什么 | 文件 |
| --- | --- |
| App 显示名 / bundle id / 版本 | `project.yml`（`PRODUCT_NAME`、`PRODUCT_BUNDLE_IDENTIFIER`、版本号）、`Resources/Info.plist`（`CFBundleDisplayName`） |
| App 图标 | `Resources/Assets.xcassets/AppIcon.appiconset/AppIcon.png`（1024×1024，**不带 alpha 通道**） |
| 前端页面 | `Resources/index.html`（替换后重新跑 Actions 即可） |

## 说明

- 页面为离线内嵌：壳启动后加载 bundle 内 `index.html`，无任何公网外链，字体回退系统中文字体。
- 提示词文本存在 WKWebView 的 `localStorage`，配图存在 `IndexedDB`；卸载 App 会一并清除。
- 导出 Word / 下载模板时，壳内通过 JS 桥接调起系统分享面板保存文件；导入支持选择 `.docx` 与本地图片。
- 部署目标 iOS 16.0+，仅竖屏。
