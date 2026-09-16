# 智能运动（SmartFit）

一个对标 Keep 核心能力的安卓智能运动 App：**GPS 轨迹记录 + 运动传感器 + 离线智能语音教练**，完全本地运行，**无广告、无需联网、数据不离开手机**。

## 功能总览

| 能力 | 说明 |
| --- | --- |
| 运动模式 | 跑步 / 健走 / 爬山 / 骑行，每种模式独立默认配速与热量系数 |
| GPS 轨迹 | 手机 GPS 实时定位，运动中实时绘制轨迹地图（Canvas 绘制，不依赖地图 SDK） |
| 海拔测量 | GPS 海拔 + 气压计（支持时自动校准），统计累计爬升 / 下降 |
| 运动数据 | 时间（总用时 / 运动用时 / 暂停时间）、里程、平均配速、**最近一公里用时**、当前公里进度、步数、步频、速度、最大速度、卡路里 |
| 智能语音教练 | 系统 TTS 离线语音：开始播报、每公里分段播报、阶段总结（可设 1/5/10 分钟）、**跑太快提醒放慢、跑太慢鼓励加速**（可开关） |
| 训练计划 | 5公里入门 / 10公里进阶 / 半马挑战 / 减脂健走 / 爬山耐力，5 套计划自动打卡 |
| **专属定制方案** | 输入身高/体重/年龄/性别，选择目标与偏好，自动生成 BMI、热量预算、蛋白质建议、目标体重与一周 7 天专属安排，可直接开始跑步/课程 |
| **卡路里估算** | 按**实际平均配速动态查 MET**（快跑/慢跑分开计）+ **爬坡补偿**（爬山爬升越多消耗越高）+ **身高体重校准**（BMI 高者单位消耗略高），数值更贴合实际 |
| **运动分享图** | 运动结束一键生成精美竖版成绩卡片（类型、距离、用时、配速、消耗、爬升、步数、步频、最大速度 + 激励语），**底部带 App 下载二维码——扫码直达 APK 安装包（免登录、免进仓库页）**，可分享微信/朋友圈 |
| **训练课程中心** | 10 门语音跟练课（减脂/塑形/拉伸），全屏跟练 + 语音教练逐动作指导 + 结束统计；**29 个动作全部配「真人教练示范图」+「Q 版拟人教练动画」（肤色四肢/运动服/短裤/五官，比火柴人直观得多）**，课程详情页与跟练页双处展示，新手照着做一看就会 |
| **减脂中心** | 每日热量预算（Mifflin-St Jeor）、饮食记录与 26 种食物热量库、体重趋势曲线、BMI 状态 |
| 数据统计 | 近 8 周 / 6 个月里程柱状图、个人最佳（最长距离、最快 1 公里、最大爬升、最佳配速） |
| **成就徽章** | 10 枚徽章（首跑 / 累计里程 / 最快一公里 / 长跑 / 爬升 / 课程打卡 / 连续 7 天）按运动数据自动解锁，统计页展示 |
| **运动音乐** | 18 首在线完整版免费正版音乐（SoundHelix + Incompetech），分类筛选（燃脂跑步 / 舒缓放松 / 轻快活力），运动页 / 音乐库页单独控制播放、暂停、切歌；**可添加自选在线歌曲**（粘贴直链）；语音教练播报时自动压低音乐音量、播报结束恢复 |
| 历史记录 | 记录详情含轨迹回放、海拔剖面、每公里分段配速条、分享、删除 |
| 设置 | 语音开关/频率/语速、快慢提醒、体重、每周目标、公里/英里、数据导出（JSON）、清空 |
| **远程升级** | 检查更新/自动检查开关、可配置更新源，**升级下载实时显示进度条**，下载完成自动调起系统安装器 |
| 隐私 | 运动数据存于 App 私有目录；**联网仅用于可选的"检查更新"**（下载新版 APK），无广告 SDK、无埋点、无账号系统 |

## 使用方法

1. 把 `app-release.apk` 传到安卓手机（Android 8.0 / API 26 及以上）。
2. 点击安装；若提示"未知来源"，在系统设置中允许安装此应用。
3. 首次使用：授予**位置权限**（必选，用于 GPS 轨迹）和**通知权限**（用于前台服务常驻提醒）。
4. 首页选择运动模式 → 开始 → 语音教练开始陪伴；运动中可暂停 / 结束保存。
5. 运动全程可在通知栏查看实时数据；退出 App 也不会中断记录（前台服务）。

## 技术说明

- Kotlin + Jetpack Compose（Material 3），minSdk 26 / targetSdk 34
- 定位：`LocationManager`（GPS + 网络定位择优），无 Google Play 服务依赖
- 传感器：`TYPE_STEP_COUNTER` 计步、`TYPE_PRESSURE` 气压计海拔校准
- 语音：系统 `TextToSpeech`（中文离线语音包），无第三方语音服务
- 数据：JSON 文件存于 `filesDir/workouts`，可导出
- 前台服务：持续定位 + 部分唤醒锁，保证锁屏/后台记录不中断

## 远程升级（OTA）

App 内置「检查更新」：依次轮询更新源列表中的每个 `version.json`（每行一个 URL，多源自动容错），
取版本号最高的结果；若 `versionCode` 大于当前版本，自动下载新版 APK 并调起系统安装器完成升级。
任一源可达即可升级——jsDelivr 与 Gitee 均为国内可直连的源，**不翻墙也能升级**。

### 已部署的更新源（v1.6.3，双源容错）

- GitHub 仓库：`https://github.com/tangjie11685-rgb/smartfit-update`（main 分支）
  - version.json（jsDelivr CDN，国内可直连）：`https://cdn.jsdelivr.net/gh/tangjie11685-rgb/smartfit-update@main/version.json`
  - APK CDN 直链：`https://cdn.jsdelivr.net/gh/tangjie11685-rgb/smartfit-update@main/smartfit-v1.6.3.apk`
  - Release 备份：`https://github.com/tangjie11685-rgb/smartfit-update/releases/tag/v1.6.3`
- Gitee 仓库（国内备份源）：`https://gitee.com/the-plump-buddha/smartfit-update`（master 分支）
  - version.json：`https://gitee.com/the-plump-buddha/smartfit-update/raw/master/version.json`
  - APK 直链（发行版附件，≤100M）：`https://gitee.com/the-plump-buddha/smartfit-update/releases/download/v1.6.3/smartfit-v1.6.3.apk`
  - 下载页：`https://gitee.com/the-plump-buddha/smartfit-update/releases/tag/v1.6.3`
- 分享图二维码指向 jsDelivr APK 直链（扫码即开始下载安装包，新手无需进仓库选文件）
- App 内默认更新源为两行（jsDelivr + Gitee），可在 设置 → 更新源地址 中自行增删（每行一个 URL）
- 本地维护文件：`update/version.json`（GitHub 版）、`update/version-gitee.json`（Gitee 版）、`update/version.json.example`（模板）

### 远程升级的安装权限（重要）

Android 8+ 由 App 调起安装器前，App 必须先获得"安装未知应用"权限：
- 清单已声明 `REQUEST_INSTALL_PACKAGES`（否则系统列表里找不到本 App）。
- 用户未授权时，App 会自动跳转系统授权页；开启一次后永久有效。
- 手机端也可手动开启：设置 → 应用 → 智能运动 → 安装未知应用 → 允许。

### 更新源配置（发新版时）

1. 构建新版 APK（记得把 `app/build.gradle.kts` 的 `versionCode` 递增、`versionName` 更新）。
2. 把新版 APK 上传到仓库 main 分支（jsDelivr 需要文件在仓库里才能加速），再上传到 GitHub Releases（如 `v1.2.0`）作为备份直链。
3. 修改 `update/version.json`：`versionCode`/`versionName` 换成新版，`url` 换成新版 APK 的 CDN 直链。
4. 在仓库网页编辑 `version.json` 提交，或本地 push 到 main 分支。
5. 清除 jsDelivr 缓存：访问 `https://purge.jsdelivr.net/gh/tangjie11685-rgb/smartfit-update@main/version.json`（缓存刷新有延迟；App 端已内置时间戳缓存穿透，可即时拿到最新内容）。
6. 用户端启动时会自动提示更新，也可手动「检查更新」。

注意：
- 升级包必须与当前安装包**使用相同签名**（本工程 release 使用调试签名；
  若换电脑构建请保持一致，或配置正式签名，否则系统会拒绝安装覆盖）。
- `INTERNET` 权限仅用于检查/下载更新，不发送任何其他数据。

## 智能语音教练

- 播报内容会说出**当前配速**（如"当前配速5'20""）；
- 配速偏快：提示放慢，并说出放慢的好处（心率平稳、减少受伤、后半程不掉速等）；
- 配速偏慢：提示加快，并说出加快的好处（燃脂更高、心肺提升、更快达成目标等）；
- 节奏合适：给予鼓励；文案从多个话术池中随机生成，避免重复。

## 在电脑上二次开发 / 重新打包

本工程为完整 Android Studio 工程（含 Gradle Wrapper），可直接用 Android Studio 打开构建：

```bash
# 命令行构建（需 JDK 17，已在 local.properties 配置 SDK 路径）
./gradlew assembleRelease   # 产物: app/build/outputs/apk/release/app-release.apk
```

依赖的构建环境（已安装到 D:\AndroidDev）：
- JDK 17（Temurin）
- Android SDK：platform 34、build-tools 34.0.0（D:\AndroidDev\sdk）

## 注意事项

- 海拔精度取决于手机硬件：有气压计的机型更准；纯 GPS 机型误差较大，爬升数据以 2 米阈值去噪。
- 室内 / 隧道等无 GPS 环境无法产生轨迹，建议在户外开阔处运动。
- 语音教练使用系统 TTS；若手机上未安装中文语音包，可在系统"文字转语音"设置中下载。
- 本 App 为离线本地实现；Keep 的社区、直播课、视频课程等依赖服务器的功能无法离线实现，已用本地训练计划 + 语音指导替代。

