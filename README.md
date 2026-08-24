# Switch DBI Backend WebUSB 传输工具 / Switch DBI Backend WebUSB Transfer Tool

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://makeapullrequest.com)

> 一个基于 WebUSB API 的 Switch DBI 后端传输工具，无需安装任何软件，打开浏览器即可使用。  
> A browser-based Switch DBI Backend transfer tool using WebUSB API. No installation required.

---

## ⚠️ 项目状态 / Project Status

**本代码由 DeepSeek 编写。** 目前功能大体可用，但存在许多小 bug，欢迎提交 Issue 和 Pull Request。  
**This code was written by DeepSeek.** The core functionality is mostly working, but there are many small bugs. Issues and Pull Requests are welcome.

| 项目 / Item                                  | 状态 / Status            |
| -------------------------------------------- | ------------------------ |
| 基本连接与传输 / Basic connection & transfer | ✅ 可用 / Working         |
| 文件计量 / File progress tracking            | ⚠️ 有 bug / Buggy         |
| 速率计算 / Speed calculation                 | ⚠️ 不精确 / Inaccurate    |
| 错误处理 / Error handling                    | ⚠️ 不完善 / Incomplete    |
| 大文件稳定性 / Large file stability          | ⚠️ 需测试 / Needs testing |

---

## 📖 项目简介 / Introduction

这是一个纯前端的 HTML 工具，利用 Chromium 内核浏览器（Chrome / Edge）的 WebUSB API 直接与 Nintendo Switch 上的 DBI 插件通信，实现 NSP / XCI / NSZ 游戏文件的传输安装。  
This is a pure frontend HTML tool that uses the WebUSB API in Chromium-based browsers (Chrome/Edge) to communicate directly with the DBI plugin on a Nintendo Switch for transferring and installing NSP/XCI/NSZ game files.

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