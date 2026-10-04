<div align="center">

# 简记 JianJi

**局域网自托管的 AI 原生 Markdown 笔记 —— 全家共用一个知识库**

[⬇️ 下载最新版](https://github.com/kynnzhou/jianji-release/releases/latest)

飞牛 fnOS · Windows · 安卓 · 浏览器 / iOS 加屏（PWA）

`自托管` `NAS` `Markdown` `AI` `多用户` `MCP` `PWA` `SQLite`

</div>

---

## 这是什么

简记跑在你自己的电脑或 NAS 上：**数据存在你家里，AI 也只在你的局域网里干活**。
一家人各自有账号，共享同一个笔记库；浏览器、安卓 App、加屏的 iPhone 都能写。
AI 能帮你写周报、给图片转文字、给录音转文字、答你「我之前记过什么」——用的是你自己的
API Key（DeepSeek / 硅基流动 / 本地 Ollama 都行），笔记内容不会经过任何第三方笔记服务。

## 为什么是简记

| | |
|---|---|
| 🤖 **AI 原生** | 流式问答、保存自动摘要+标签建议、选中改写、语音速记转文字、图片 OCR、自动周报——自托管笔记里少有的全栈 AI，全部自带 Key、免费使用 |
| 👨‍👩‍👧 **多用户家庭共享** | 邀请制注册、管理员/成员角色、数据按账号隔离、审计日志；简记的定位就是「全家的知识库」，不是单人工具 |
| 📱 **三端覆盖** | 安卓原生 App（离线可用、生物锁、桌面速记小组件）；iOS 用 Safari 加屏即装（PWA）；电脑浏览器直接用 |
| 🧬 **数据随时带走** | SQLite 单文件 + 每日自动备份 + 一键导出 zip（纯 Markdown + 附件）。你的笔记永远不锁在任何软件里——包括简记 |
| 🔌 **MCP 开放** | Claude Desktop、Cursor 等 AI 助手通过 MCP 直接搜索/读写你的笔记库，笔记成为 AI 的记忆 |
| 🖥️ **NAS 友好** | 飞牛 fnOS 一键安装（支持自定义端口、升级自动保留）；Windows 安装器开机自启+每日备份；单容器轻量运行 |

## 功能全景

**写作与组织**
- Markdown 编辑/分屏实时预览（表格、代码高亮、任务清单、数学公式）
- 笔记本、嵌套标签（正文打 `#标签` 自动归档）、置顶、模板、每日笔记
- 回收站（30 天可恢复）、版本历史（每篇 20 版）、自动保存

**检索与连接**
- 全文搜索（中文优化）+ 语义搜索 + 命中高亮
- `[[双链]]`、反向链接、关系图谱、相关笔记推荐
- 命令面板（Ctrl+P）、保存的筛选、标签自动补全

**AI（自带 Key，服务端统一管理）**
- AI 问答：基于你的笔记回答，附引用来源，SSE 流式
- 保存笔记自动生成摘要与标签建议
- 语音速记（OpenAI Whisper 兼容转录）、图片文字识别（多模态模型）
- 每周自动聚合上周笔记生成一篇周报
- 行内改写：选中文字让 AI 润色/扩写

**开放与自动化**
- 开放 API（个人令牌）+ Webhook（HMAC 签名、失败重试）
- MCP Server（8 个工具，供 AI 客户端接入）
- 浏览器剪藏扩展（Chrome MV3）、公开分享链接（可设有效期与密码）
- 导入迁移中心（常见笔记格式一键导入）

**多用户与安全**
- 邀请制注册、角色权限、会话管理、登录防爆破、argon2id 口令哈希
- 安卓端生物识别锁

## 下载与安装

到 [**Releases 最新版**](https://github.com/kynnzhou/jianji-release/releases/latest) 下载：

| 平台 | 文件 | 安装方式 |
|---|---|---|
| 飞牛 fnOS | `JianJi-fnOS-<版本>.fpk` | 应用中心 → 手动安装 → 上传 FPK；安装向导可设端口，升级自动保留 |
| Windows | `JianJi-Setup-<版本>.exe` | 双击安装（默认装 `C:\Jianji`，开机自启，每日 03:17 自动备份） |
| 安卓 | `Jianji-Android-<版本>.apk` | 允许安装未知来源后直接装；更新可在应用内一键完成 |
| iOS / 任意浏览器 | 无需安装 | 浏览器打开服务地址；iPhone 在 Safari「添加到主屏幕」获得类 App 体验 |

装好后：浏览器访问 `http://<主机IP>:8008`（飞牛按你向导里设的端口）。**第一个注册的账号自动是管理员。**

## 30 秒开启 AI

1. 管理员登录 → 设置中心 → **AI 与智能服务**
2. 填入任意 OpenAI 兼容服务：Base URL + API Key + 模型名（DeepSeek、硅基流动、Kimi、本地 Ollama 均可）
3. 保存即生效——全家立刻可用问答/摘要/语音/图片识别；未配置时 AI 入口自动隐藏，不碍眼

## 让 AI 助手直接操作你的笔记（MCP）

简记内置 MCP Server（stdio）。给 Claude Desktop / Cursor 配上之后，可以直接用对话
「帮我找一下记过的服务器配置」「把这段整理成新笔记」：

```json
{
  "mcpServers": {
    "jianji": {
      "command": "<python.exe>",
      "args": ["<简记 backend 目录>/mcp_server.py"],
      "env": {
        "JIANJI_API": "http://<主机IP>:8008",
        "JIANJI_TOKEN": "<在 设置中心 → 开放 API 令牌 创建>"
      }
    }
  }
}
```

可用工具：`search_notes` / `list_notes` / `list_notebooks` / `list_tags` / `get_note` / `create_note` / `update_note` / `delete_note`。
令牌即身份：AI 只能读写创建令牌那个账号自己的笔记。完整说明见应用内「设置中心 → MCP 工具」。

## 数据与安全

- **存储**：单文件 SQLite（WAL 模式），每日凌晨自动备份并轮转保留 14 份
- **导出**：设置中心一键导出 zip——按笔记本分目录的 `.md`（含 frontmatter）+ 附件原件
- **口令**：argon2id 哈希、登录失败锁定、会话 30 天过期、登录会话远程吊销
- **事件**：笔记增删改可推 Webhook（HMAC-SHA256 签名，自动重试）
- 局域网 HTTP 明文是自托管常态；若暴露公网，请务必套 HTTPS 反向代理

## 版本更新

三端都支持**应用内检查更新**：下载包经 ed25519 签名清单 + 逐源 SHA-256 校验
（多下载源自动轮换），飞牛升级自动保留你设置的自定义端口。也可以随时来本仓
[Releases](https://github.com/kynnzhou/jianji-release/releases) 手动下载。

---

*简记 JianJi · 为家庭自托管而生*
