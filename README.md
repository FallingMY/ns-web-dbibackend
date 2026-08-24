# ns-web-dbibackend：Switch DBI Backend WebUSB 传输工具 / ns-web-dbibackend: Switch DBI Backend WebUSB Transfer Tool

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://makeapullrequest.com)

> 一个基于 WebUSB API 的 Switch DBI 后端传输工具，无需安装任何软件，打开浏览器即可使用。  
> A browser-based Switch DBI Backend transfer tool using WebUSB API. No installation required.

---

## 🌐 在线使用 / Online

无需本地运行，直接访问：https://fallingmy.github.io/ns-web-dbibackend/  
No local hosting needed, just visit: https://fallingmy.github.io/ns-web-dbibackend/

---

## ⚠️ 项目状态 / Project Status

**本代码由 DeepSeek 编写。** 目前功能大体可用，但存在许多小 bug，欢迎提交 Issue 和 Pull Request。  
**This code was written by DeepSeek.** The core functionality is mostly working, but there are many small bugs. Issues and Pull Requests are welcome.

当前为 **稳定版 v9**：修复文件计量、速率改用滑动窗口 + EMA 平滑、消除分片完成误报、USB 传输超时自动断开并提示重新连接。  
Current version is **v9**: fixed file progress tracking, speed now uses a sliding window + EMA smoothing, removed false "chunk complete" reports, and USB timeouts now auto-disconnect with a reconnection prompt.

| 项目 / Item                                  | 状态 / Status            |
| -------------------------------------------- | ------------------------ |
| 基本连接与传输 / Basic connection & transfer | ✅ 可用 / Working         |
| 文件计量 / File progress tracking            | ✅ 已修复 / Fixed (v9)    |
| 速率计算 / Speed calculation                 | ✅ 基本可用 / Usable      |
| 错误处理 / Error handling                    | ⚠️ 改进中 / Improving    |
| 大文件稳定性 / Large file stability          | ⚠️ 需测试 / Needs testing |

---

## 📖 项目简介 / Introduction

这是一个纯前端的 HTML 工具，利用 Chromium 内核浏览器（Chrome / Edge）的 WebUSB API 直接与 Nintendo Switch 上的 DBI 插件通信，实现 NSP / XCI / NSZ 游戏文件的传输安装。  
This is a pure frontend HTML tool that uses the WebUSB API in Chromium-based browsers (Chrome/Edge) to communicate directly with the DBI plugin on a Nintendo Switch for transferring and installing NSP/XCI/NSZ game files.

这个工具主要是为了方便 **macOS 和 Linux** 用户使用而制作的——这两个系统无法（或不方便）通过 MTP 挂载 Switch，浏览器 + WebUSB 是最省事的传输方式。Windows 用户建议优先使用 DBI 的 MTP 连接模式（资源管理器直接拖放文件，无需安装驱动）。  
This tool is primarily built for **macOS and Linux** users — these systems cannot (or cannot easily) mount the Switch via MTP, so a browser + WebUSB is the most convenient way to transfer. Windows users are recommended to use DBI's MTP mode instead (drag & drop in Explorer, no driver needed).

### 特性 / Features

- 🚀 **纯浏览器运行**：无需安装 Python、Node.js 或其他依赖  
  **Pure browser-based**：No Python, Node.js or other dependencies needed
- 🔌 **绕过 MTP**：直接通过 USB Bulk 端点通信，不受系统 MTP 协议干扰  
  **Bypasses MTP**：Communicates directly via USB Bulk endpoints, unaffected by system MTP protocol
- 📦 **支持多格式**：NSP、XCI、NSZ  
  **Multi-format support**：NSP, XCI, NSZ
- 📊 **传输进度**：显示文件整体覆盖进度（去重后）  
  **Transfer progress**：Shows overall file coverage (deduplicated)
- ⚡ **速率显示**：实时估算传输速率  
  **Speed display**：Real-time transfer speed estimation
- 🛡️ **超时保护**：内置心跳检测与 USB 读写超时  
  **Timeout protection**：Built-in heartbeat detection and USB read/write timeouts
- 🌐 **中英双语**：右上角一键切换全部界面语言（含排查指南）  
  **Bilingual UI**：One-click language switch (ZH/EN) in the top-right corner, including the troubleshooting guide

---

## 🔧 使用方法 / Usage

### 前提条件 / Prerequisites

1. **Switch 端**：已安装 [DBI](https://github.com/rashevskyv/dbi) 插件  
   **On Switch**：DBI plugin installed
2. **电脑端**：Chrome 或 Edge 浏览器（版本 ≥ 89，支持 WebUSB）  
   **On PC**：Chrome or Edge browser (version ≥ 89, WebUSB supported)
3. **USB 数据线**：支持数据传输的 Type-C 线（非仅充电线）  
   **USB cable**：Data-capable Type-C cable (not charge-only)

### 操作步骤 / Steps

1. **启动 DBI**：在 Switch 相册中打开 DBI，选择「后端」（Backend）模式  
   **Launch DBI**：Open DBI from the Switch album, select "Backend" mode
2. **连接 USB**：用数据线连接 Switch 与电脑  
   **Connect USB**：Connect Switch to PC with a data cable
3. **打开工具页面**：在浏览器中打开本工具的 HTML 文件（需 HTTPS 或 localhost 环境）  
   **Open tool page**：Open the HTML file in your browser (HTTPS or localhost required)
4. **连接设备**：点击「连接 Switch」按钮，在弹窗中选择 DBI 设备  
   **Connect device**：Click "Connect Switch" and select the DBI device from the dialog
5. **添加文件**：拖放或点击添加 NSP/XCI/NSZ 文件到列表  
   **Add files**：Drag & drop or click to add NSP/XCI/NSZ files
6. **启动服务**：点击「启动服务」按钮，开始监听 Switch 的传输请求  
   **Start server**：Click "Start Server" to begin listening for Switch requests
7. **在 Switch 上操作**：在 DBI 中选择要安装的文件（文件名会出现在列表中）  
   **Operate on Switch**：Select the file to install in DBI (filenames will appear in the list)

---

## 🖥️ 本地运行 / Local Hosting

由于 WebUSB 要求安全上下文，你需要通过以下任一方式运行：

Since WebUSB requires a secure context, you need to run it via one of these methods:

```bash
# 方式 1：使用 Python 内置 HTTP 服务器
# Method 1: Python built-in HTTP server
python3 -m http.server 8080
# 然后访问 http://localhost:8080

# 方式 2：使用 Node.js http-server
# Method 2: Node.js http-server
npx http-server -p 8080
# 然后访问 http://localhost:8080
```

---

## 🔍 常见问题 / Troubleshooting

**连接不到设备 / Cannot find the device**

- 确认 Switch 上 DBI 已启动并处于「后端」(Backend) 模式
- 确认使用支持数据传输的 USB 线（非仅充电线），尝试换一个 USB 口
- 关闭占用 USB 的软件（如系统文件管理器对 Switch 的 MTP 弹窗）
- 页面必须通过 HTTPS 或 localhost 访问，`file://` 协议下 WebUSB 不可用

**传输中断后无法重新连接 / Cannot reconnect after a transfer failure**

USB 传输超时后设备连接无法恢复，工具会自动断开并提示重新连接；如仍失败请刷新页面后重试。

**Windows 下无法识别设备 / Device not detected on Windows**

WebUSB 在 Windows 上要求设备接口绑定 **WinUSB** 驱动，而 DBI 官方教程为 Windows 推荐安装的是 **libusbK** 驱动（供 dbibackend.exe 使用）。libusbK 驱动下浏览器无法访问设备，选择弹窗中不会出现 Switch。macOS / Linux 无此驱动要求，可直接使用。

修复：用 [Zadig](https://zadig.akeo.ie) 将 Switch 设备的驱动替换为 **WinUSB**（Options → List All Devices → 选择 Switch / DBI 设备（VID 057E / PID 3000）→ 目标驱动选 WinUSB → Install），然后重新插拔 USB。替换后 dbibackend.exe 依然可用（libusb 支持 WinUSB）。

**Windows 下 WebUSB 无响应 / WebUSB not responding on Windows**

检查设备管理器中 Switch 的驱动，若被识别为 MTP 设备，需先在系统设置中禁用 MTP 自动挂载（或在 DBI 端重新插拔）。

---

## 📜 许可 / License

[MIT](LICENSE)
