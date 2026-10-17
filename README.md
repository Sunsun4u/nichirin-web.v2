# 日轮 · 世界计划角色配色版

> 在 [Nichirin](https://github.com/ryujou/Nichirin_APP)（安卓蓝牙灯控 App）基础上扩展的版本：
> **86 个角色**，点一下就能把灯变成对应应援色，另附一个 **iPhone 也能用的网页版**。

<p>
<img alt="Android" src="https://img.shields.io/badge/Android-12%2B-3DDC84?logo=android&logoColor=white">
<img alt="Kotlin" src="https://img.shields.io/badge/Kotlin-2.0-7F52FF?logo=kotlin&logoColor=white">
<img alt="Jetpack Compose" src="https://img.shields.io/badge/Jetpack%20Compose-Material3-4285F4?logo=jetpackcompose&logoColor=white">
<img alt="BLE" src="https://img.shields.io/badge/BLE-FFE0%2FFFE1%2FFFE2-0082FC">
</p>

原项目是一个围绕 `FFE0/FFE1/FFE2` 服务、Modbus RTU 协议的实验性灯控工具，能以 400 Hz 向设备写入 12 带能量谱数据。
这个版本**保留全部原有功能**，并把「角色配色」做成了主角。

---

## 目录

- [功能一览](#功能一览)
- [界面说明](#界面说明)
- [安装](#安装)
- [使用方法](#使用方法)
- [网页版（iPhone）](#网页版iphone)
- [自己加角色](#自己加角色)
- [常见问题](#常见问题)
- [构建](#构建)
- [项目结构](#项目结构)
- [更新日志](#更新日志)
- [致谢与版权](#致谢与版权)

---

## 功能一览

### 🎨 角色配色（本版新增，主打）

- **86 个角色**：BanG Dream! 60 位 + 世界计划 26 位
- 世界计划覆盖 6 个组合：Leo/need、MORE MORE JUMP!、Vivid BAD SQUAD、
  Wonderlands×Showtime、25点，Nightcord见。、VIRTUAL SINGER
- 全部使用**官方应援色**，点一下 = 蓝牙下发一条颜色帧
- 支持**搜索角色**、**按组合筛选**
- 头像：邦邦 180×180、世界计划 240×240，图源[萌娘百科](https://zh.moegirl.org.cn)

![世界计划角色](docs/images/世界计划角色.png)

### ⚙️ 灯光模式

| 模式 | 说明 |
|---|---|
| 模式 1 | 流水 |
| 模式 2 | 爆闪 |
| 模式 3 | **常亮**（想看角色颜色选这个） |
| 模式 4 | 呼吸 |
| 模式 5 | 频谱（音频驱动，颜色区被隐藏） |

### 📊 原有功能（保留）

- 扫描 BLE 设备（可只显示广播 FFE0 服务的设备）、连接 / 断开、RSSI 调试信息
- 三种 400 Hz 数据源：**测试锯齿 / 实时麦克风 / 本地音频文件**
- 12 带能量谱，可调采样率、块大小、地板与动态范围
- 前台服务保活，避免息屏后台被杀导致断连
- 桌面动态快捷方式 `watching_1/2/3`

---

## 界面说明

进入设备详情页后有两个标签：

**① 控制台**
- 模式切换、色相 / 饱和度 / 亮度、参数
- 启动 / 停止频谱发送，选择数据源（锯齿 / 麦克风 / 文件）
- 从设备读回当前配置

**② 角色配色**
- 顶部搜索框 + 组合下拉
- 点任意角色 → 立即把该角色的应援色下发到灯
- 显示 `共 86 名角色`

---

## 安装

### 安卓（推荐）

1. 下载 [`apk/日轮-世界计划版.apk`](apk/日轮-世界计划版.apk)（约 32 MB）
2. 传到手机 → 点击安装 → 允许「安装未知来源应用」
3. 首次启动授予**蓝牙 / 附近设备**权限

**要求**：Android 12（API 31）及以上。

> ⚠️ 如果装过官方版 Nichirin，**请先卸载**再装这个 —— 签名不同，无法覆盖安装。

### iPhone

Safari 不支持 Web Bluetooth，需要按 [网页版](#网页版iphone) 一节部署后用 **Bluefy** 打开。

---

## 使用方法

1. 打开灯的电源，确保**没有其他设备连着它**（BLE 同一时间只能连一个主设备）
2. App 里点「开始扫描」→ 选中你的设备 → 进入详情页
3. 切到「**角色配色**」标签页
4. 搜索角色，或用「乐队」下拉筛选组合
5. 点任意角色 → 灯立刻变成对应应援色

---

## 网页版（iPhone）

网页版是**单文件**（`web/index.html`，约 420 KB），86 个角色和头像全部内嵌，不需要服务器后端。

**部署**：把 `web/index.html` 传到 GitHub Pages 等任意 **https** 静态托管，
详细图文步骤见 [`web/部署到iPhone.md`](web/部署到iPhone.md)。

**使用**：iPhone 安装 [Bluefy](https://apps.apple.com/app/id1492822055) → 打开网址 → 点「连接」→ 选灯 → 点角色。

> ✅ **算法一致性已验证**：网页版的取色与组帧逻辑是从安卓源码逐字移植的，
> 并用真正的 Kotlin 编译器跑原版算法做对拍 —— 94 个颜色 × 2 种模式 = **188 帧逐字节一致**
> （包括 Kotlin 32 位浮点误差，用 `Math.fround` 精确模拟）。

**网页版限制**：没有频谱功能（麦克风拾音、本地音频播放），那是安卓原生能力。

---

## 自己加角色

角色完全由数据驱动，**加新角色不用改逻辑代码**：

1. 头像 PNG 放进 `app/src/main/assets/` 下（新作品建议建独立文件夹，如 `pjsk_avatars/`）
2. 在 `app/src/main/assets/bangdream_avatars.json` 的 `items` 数组里追加一条：

```json
{
  "name": "角色名",
  "color": "#33AAEE",
  "band": "组合名",
  "image": "assets/pjsk_avatars/01_ichika.png"
}
```

3. （可选）在 `MainActivity.kt` 的 `bandPalettes` 里加一条组合色卡
4. 重新编译

> `color` 必须是 **6 位十六进制**（`#33AAEE` 或 `33AAEE`）。写错了不会崩溃，会静默变成粉红 `#FF66AA`。
> 图片路径**大小写敏感**，找不到文件不会报错，只会显示空白。

完整步骤和踩坑清单见 [`docs/世界计划角色加入指南.md`](docs/世界计划角色加入指南.md)。

---

## 常见问题

<details>
<summary><b>连上了，但点角色灯不变色</b></summary>

多半是当前处于**模式 5（频谱）**。频谱模式下颜色被音频数据盖住，切到「模式 3 常亮」再试。
</details>

<details>
<summary><b>扫描不到设备</b></summary>

- 灯被别的设备连着（比如安卓 App 还连着，或另一台手机）→ 先断开
- 灯没进入广播状态 → 重新上电
- 手机没给「附近设备」权限
- 网页版的话，勾上「显示全部蓝牙设备」再试
</details>

<details>
<summary><b>安装时报「应用未安装」/「解析包错误」</b></summary>

- 手机上已有官方版，签名冲突 → 先卸载旧版
- 安卓版本低于 12 → 这个包装不了
</details>

<details>
<summary><b>角色头像显示为空白</b></summary>

`image` 路径与实际文件对不上（大小写、目录层级）。程序不会报错，只会空着。
</details>

<details>
<summary><b>网页版提示"浏览器不支持 Web Bluetooth"</b></summary>

你用的是 Safari。iOS 必须用 **Bluefy**；安卓用 **Chrome**。
</details>

<details>
<summary><b>网页版提示"当前不是 https 地址"</b></summary>

用 `file://` 或 `http://` 打开了。Web Bluetooth 强制要求 **https**（localhost 除外）。
</details>

---

## 构建

```bash
./gradlew assembleDebug
# 产物：app/build/outputs/apk/debug/app-debug.apk
```

| 项 | 版本 |
|---|---|
| minSdk | 31（Android 12） |
| compileSdk / targetSdk | 36 |
| AGP | 9.0.0 |
| Gradle | 9.1.0 |
| Kotlin | 2.0.21 |
| JDK | 17+（实测 21 可用） |

---

## 项目结构

```
app/src/main/
├── AndroidManifest.xml
├── assets/
│   ├── bangdream_avatars.json     # 86 个角色：名字 / 应援色 / 组合 / 头像路径
│   ├── bangdream_avatars/         # BanG Dream! 60 张头像
│   ├── pjsk_avatars/              # 世界计划 26 张头像
│   ├── band_icon/                 # 组合图标
│   └── card.json                  # 背景卡面（在线加载）
├── java/com/example/nichirin/
│   ├── MainActivity.kt            # UI + 全部交互逻辑
│   ├── CharacterRepository.kt     # 读取角色 JSON
│   ├── CardRepository.kt          # 读取卡面 URL
│   ├── BleProtocol.kt             # Modbus RTU 组帧 + CRC16
│   ├── BleConstants.kt            # UUID / 寄存器地址
│   ├── BleUtils.kt / BleModels.kt
│   ├── ForegroundKeepAliveService.kt
│   ├── dsp/                       # FFT / 频谱引擎
│   └── ui/theme/                  # Material3 主题
└── res/                           # 图标、背景、字符串

web/
├── index.html                     # 单文件网页版（86 角色全内嵌）
└── 部署到iPhone.md

docs/
├── 世界计划角色加入指南.md           # 手把手教程
└── images/                        # README 用图
```

---

## 更新日志

### v3.0-world（本版）

- ✨ 新增《世界计划》26 位角色及官方应援色
- ✨ 新增 6 个组合色卡（Leo/need / MMJ / VBS / WxS / 25ji / VS）
- 🖼 邦邦 60 张头像从 50px 缩略图升级为 180px 原图
- 🖼 世界计划 26 张头像使用官方 Q 版角色图标（240px）
- 📱 新增单文件网页版，支持 iPhone（Bluefy / Web Bluetooth）
- 📝 补全 README、角色添加教程、网页版部署文档

### v3.0（原项目）
- 400 Hz 12 带频谱发送、三种数据源、前台保活服务

---

## 致谢与版权

- 原项目：[ryujou/Nichirin_APP](https://github.com/ryujou/Nichirin_APP)
- 角色头像来自[萌娘百科](https://zh.moegirl.org.cn)
- 角色与素材版权归 **SEGA / Colorful Palette / CFM**（世界计划）
  与 **Bushiroad**（BanG Dream!）所有
- 本项目仅供**个人 DIY 与学习**使用，请勿商用或公开分发角色素材
- 部分代码与文档在 AI 工具辅助下完成
