# axion-stack-launcher

将 AxionOS 的「堆叠式最近任务」(AxStack) 移植到 **Lawnchair 16 (Android 17 / SDK 37)**，
并利用 **GitHub Actions 云端构建**，产出可直接安装到 PixelOS / Android 17 的桌面 APK。

## 目标
- 在 Android 17 (SDK 37) 的 PixelOS 上实现堆叠式(卡片堆叠)最近任务。
- 全程 **GitHub Actions 自动构建**，本地无需搭建 Android 编译环境。

## 方案
1. 基于 **Lawnchair `16-dev`** 分支（`targetSdk 37` / Android 17，纯 Gradle 独立构建）。
2. 通过 GitHub Actions 的 `ubuntu-latest` runner（自带 JDK 17）执行 `./gradlew assembleRelease`。
3. 编译产物（桌面 APK）作为 Actions Artifact 上传，可直接下载。
4. 堆叠 recents (AxStack) 逻辑作为后续增量 patch 合入。

## 构建
仓库内 `.github/workflows/build.yml` 触发的 Actions 会自动完成构建：
- 检出源码
- 使用预编译 framework jar（`framework-16.jar` 等，来自 Lawnchair 16 专用 prebuilt）
- `gradlew assembleRelease`
- 上传 APK artifact

## 说明
AxStack 为 AxionOS 私有定制，本仓库的 recents 堆叠逻辑为独立重实现 / 增量移植，
以避免直接使用混淆后的反编译代码。