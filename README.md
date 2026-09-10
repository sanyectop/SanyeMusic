<div align="center">

# SanyeMusic

**叁葉音乐** —— 基于 Tauri 2 + React 19 的 Windows 桌面音乐播放器

内置 QQ音乐 / 网易云 / 酷狗多源搜索 · lx-music 音源脚本兼容 · QQ 音乐雷达 · 逐字歌词 · 桌面歌词 · AI 歌词翻译

`PrivateBeta4`

![platform](https://img.shields.io/badge/platform-Windows-0078d4?logo=windows&logoColor=white)
![tauri](https://img.shields.io/badge/Tauri-2.x-24C8D8?logo=tauri&logoColor=white)
![react](https://img.shields.io/badge/React-19-61dafb?logo=react&logoColor=black)
![ts](https://img.shields.io/badge/TypeScript-5.8-3178c6?logo=typescript&logoColor=white)
![license](https://img.shields.io/badge/license-MIT-green)

> ⚠️ 本项目为非官方个人学习项目，与腾讯 QQ音乐、网易云音乐及各音源/主题权利方无关。

</div>

---

## ✨ 特性一览

### 🔍 多平台搜索与播放
- 内置 **QQ音乐 / 网易云 / 酷狗** 三平台在线源（搜索、歌单、歌词、播放链接解析均由本地完成）
- **兼容 lx-music 音源脚本**：导入 `.js` / `.csd` 插件即可扩展自定义源（支持拖拽导入），由 Rust 侧 boa_engine JS 沙箱隔离执行，兼容 lx-music preload API
- 音质可选 **Hi-Res（flac24bit）/ 无损（flac）/ 极准（320k）/ 标准（128k）**，缺音质自动就近降级，播放中可即时切换
- 搜索历史、播放进度记忆（可按音质分别记忆）、右键菜单（下一首播放 / 移动 / 重排序等）

### 📡 QQ 音乐雷达
在主窗口内嵌真实 `y.qq.com` 电台页，自动抓取私人雷达推荐曲目并连续播放，队列将尽时自动预载续播；支持 QQ 扫码登录（头像授权一键完成）。

### 🎤 歌词
- 应用内歌词面板：LRC 与**逐字歌词**（已适配 QQ `qrc` / 酷狗 `krc` / 酷我 `lrcx` / 网易 `yrc` 格式解密归一化）
- **桌面歌词**独立进程（SanyeLyric）：透明置顶、横/竖排、逐字高亮、活动行放大、字重/字号/行距/颜色/透明度全量可调、锁定后鼠标穿透、禁止拖出屏幕
- **AI 歌词翻译**：流式翻译 + 磁盘缓存，支持 中/英/日/韩/俄/法/西/葡/德 等目标语言，提供商可配 Grok / OpenAI / DeepSeek / Moonshot / 自定义（API Key 本地 AES-256-GCM 加密存储）

### 🎮 Phigros 主题「我的列表」（致敬向）
音乐游戏风格的歌单浏览体验：Touch to Start 起始界面、章节式歌单选择、横向封面转盘（惯性 + 吸附）、点击试听片段、随音质变化的游戏音效反馈；支持通过 `.smt` 声明式主题文件自定义。

### 💾 数据与缓存
- 音频**本地磁盘缓存**（LRU 容量上限、预载下一首），播过的歌可离线重播
- **WebDAV 同步**（内置中科院 data.cstcloud.cn 与 Koofr 预设）与**局域网设备互传**
- 播放列表（试听 / 收藏 / 雷达 / 自建）管理，支持粘贴分享链接 / 歌单 ID 直接导入在线歌单

### 🖥️ 系统集成
- Windows SMTC 系统媒体控制（锁屏/通知中心卡片、媒体键）与曲目元数据/封面同步
- 无边框自绘标题栏，**触屏/平板模式下亦支持触摸拖窗**
- 单实例启动、可折叠侧边导航、系统字体 + 在线字体自定义界面字体

---

## 🧪 测试歌单

导入以下歌单可快速验证多源搜索、音质切换、逐字歌词与歌单收藏等功能：

**[QQ音乐测试歌单（点击打开）](https://y.qq.com/n/ryqq_v2/playlist/5148432890)**

> 在「我的列表」页使用「通过链接打开歌单」，粘贴上面的链接（或歌单 ID `5148432890`）即可导入。

---

## 🚀 快速开始

### 环境要求
- Windows 10/11
- [Rust](https://www.rust-lang.org/tools/install)（stable）
- [Node.js](https://nodejs.org/) ≥ 18
- Visual Studio C++ Build Tools（Tauri 前置依赖）

### 开发调试

```bash
# 主程序
cd SanyeMusic
npm install
npm run tauri dev

# 桌面歌词（独立进程，开发时单独起）
cd ../SanyeLyric
npm install
npm run tauri dev
```

> 打包主程序时，SanyeLyric 的 release 产物会作为 `binaries/sanyelyric.exe` 随主程序一同分发、由主程序自动拉起。

### 生产构建

```bash
cd SanyeMusic && npm run tauri build
cd SanyeLyric && npm run tauri build
```

---

## 🏗️ 项目结构

```
.
├── SanyeMusic/          # 主程序（Tauri 2 + React 19 + MUI 9）
│   ├── src/             #   前端：页面 / 组件 / 播放与音源服务
│   ├── src-tauri/       #   Rust 后端：多平台源解析、qrc/krc/lrcx/yrc 歌词解密、
│   │                    #   boa_engine 音源沙箱、音频代理与缓存、雷达 WebView、
│   │                    #   歌词 HTTP/SSE API、WebDAV/LAN 同步、SMTC
│   └── tests/           #   47 项行为测试（node --test）
├── SanyeLyric/          # 桌面歌词窗口（独立 Tauri 进程，经 HTTP+SSE 与主程序同步）
├── doc/                 # 使用文档站（Vite + React）
└── UI参考/ 参考代码/    # 视觉参考与 lx-music 兼容性参考
```

### 技术栈

| 层 | 选型 |
|---|---|
| 桌面框架 | Tauri 2.11（Rust 后端 + WebView2） |
| 前端 | React 19 · TypeScript 5.8 · Vite 7 · MUI 9（Material 3 设计） |
| 音源沙箱 | boa_engine（lx-music 脚本兼容） |
| 网络 | reqwest（rustls）+ 本地流媒体音频代理（Range / Referer 伪装） |
| 加密 | AES-256-GCM（AI Key 保管）、AES/MD5/SHA（平台接口签名） |
| 系统集成 | tauri-plugin-media（SMTC）、global-shortcut、dialog、opener |

---

## ⚖️ 免责声明

- 本项目为个人学习与技术研究用途的非官方开源项目，**与腾讯、网易、酷狗、音源脚本作者及 Phigros（Pigeon Games）等权利方均无关联**。
- Phigros 主题中的音效、字体等素材版权归 Pigeon Games 所有，仅作致敬演示，请勿用于任何商业用途。
- 平台接口解析逻辑参考 [lx-music-desktop](https://github.com/lyswhut/lx-music-desktop)（Apache-2.0），谨向原作者致谢。
- 请勿将本项目用于侵犯音乐版权的行为，使用后请在 24 小时内删除相关文件；因使用本项目造成的后果由使用者自行承担。

## 🙏 致谢

- [lx-music-desktop](https://github.com/lyswhut/lx-music-desktop) — 音源脚本生态与解析思路
- [Tauri](https://tauri.app/) · [React](https://react.dev/) · [MUI](https://mui.com/)
- [Phigros](https://www.pigegame.org/)（Pigeon Games）— 主题灵感来源

---

<div align="center">

如果本项目对你有帮助，欢迎点个 **Star** ⭐

</div>
