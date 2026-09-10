# Jotify Moment MCP Server

[![npm version](https://img.shields.io/npm/v/jotify-moment-mcp.svg)](https://www.npmjs.com/package/jotify-moment-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Model Context Protocol (MCP) server for **[Jotify Moment](https://github.com/daocatt/jotify-moment)**.

让 **Claude Desktop**、**Cursor**、**Antigravity**、**OpenCode** 等任意支持 MCP 协议的 AI 客户端，通过自然语言免交互地在你的 Jotify Moment 平台上：
- 📸 **上传本地图片/音视频或 URL 资源**（支持批量与单张上传）
- ✍️ **发布 Moment 动态**（完整支持 Markdown 与富媒体配图）
- 🔍 **检索与查询** 平台最近发布的动态
- 👤 **校验 Token 鉴权** 并获取当前用户信息

---

## 🚀 快速接入配置

无需手动 clone 或安装代码，借助 `npx` 即可直接免安装运行。

### 🔑 第一步：获取 API 密钥
1. 登录你的 Jotify Moment 网站。
2. 进入 **「个人设置」(`/settings`)** -> **「API 密钥 (PAT)」**。
3. 点击 **「生成新密钥」**，勾选权限（如 `posts:write`、`media:upload`），复制生成的 Token（格式如 `jotify_pat_...`）。

---

### 💻 第二步：配置到各大 AI 工具

#### 1. Claude Desktop
编辑你的 `claude_desktop_config.json`：
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "jotify-moment": {
      "command": "npx",
      "args": [
        "-y",
        "jotify-moment-mcp"
      ],
      "env": {
        "JOTIFY_BASE_URL": "https://your-moment-domain.com",
        "JOTIFY_API_TOKEN": "jotify_pat_xxxxxxxxxxxxxxxx"
      }
    }
  }
}
```

#### 2. Cursor
在 Cursor Settings -> **Features** -> **MCP Servers** -> **Add New MCP Server**：
- **Name**: `jotify-moment`
- **Type**: `command`
- **Command**: `npx -y jotify-moment-mcp`
- **Environment Variables**:
  - `JOTIFY_BASE_URL`: `https://your-moment-domain.com`
  - `JOTIFY_API_TOKEN`: `jotify_pat_xxxxxxxxxxxxxxxx`

#### 3. Antigravity / Gemini / 自定义 MCP Client
在项目的 `mcp_config.json` 或相应配置文件中添加：
```json
{
  "mcpServers": {
    "jotify-moment": {
      "command": "npx",
      "args": ["-y", "jotify-moment-mcp"],
      "env": {
        "JOTIFY_BASE_URL": "https://your-moment-domain.com",
        "JOTIFY_API_TOKEN": "jotify_pat_xxxxxxxxxxxxxxxx"
      }
    }
  }
}
```

---

## 🧰 提供的 MCP Tools

| 工具名称 (Tool Name) | 功能说明 | 核心入参 (Parameters) |
| :--- | :--- | :--- |
| **`jotify_get_profile`** | 验证 API Token 并获取当前登录用户信息（昵称、Slug、角色等） | *无* |
| **`jotify_upload_media`** | 上传单个媒体文件（支持本地路径 `filePath`、远程 `url` 或 `base64Data`） | `filePath` 或 `url` 或 `base64Data` |
| **`jotify_upload_batch`** | 一次性批量上传多张图片/多媒体，减少多轮交互延迟 | `filePaths` 或 `items` 列表 |
| **`jotify_create_post`** | 在 Jotify Moment 发布一条图文动态（正文支持 Markdown） | `content` (必填), `mediaUrls` (可选) |
| **`jotify_list_recent_posts`** | 获取平台最近发布的公开动态列表 | `limit` (默认 10) |

---

## 💬 自然语言交互示例

配置完成后，你可以直接在 AI 对话框中输入：

- 📝 **文字动态**：
  > *"帮我发一条 jotify：今天攻克了一个棘手的 bug，下班喝杯咖啡庆祝一下 ☕"*

- 🖼️ **图文结合**：
  > *"把桌面上的 `./charts/result.png` 上传并发到 jotify，配文写一下今天的实验结论分析"*

- 🎨 **AI 生图并自动发布**：
  > *"帮我画一张赛博朋克风格的雨夜小巷壁纸，然后直接发布到我的 jotify moment 动态"*

- 🔍 **查看与自检**：
  > *"检查一下我的 jotify 登录状态"*
  > *"看看我最近在 jotify 发了什么动态"*

---

## 🛠️ 本地开发与调试

```bash
# 克隆仓库
git clone git@github.com:daocatt/jotify-moment-mcp.git
cd jotify-moment-mcp

# 安装依赖并构建
npm install
npm run build

# 本地调试运行
JOTIFY_BASE_URL="https://your-domain.com" JOTIFY_API_TOKEN="your_token" node dist/index.js
```

---

## 📄 开源协议

[MIT License](LICENSE)
