# FileReader - 文件阅读器插件

[English](#english) | [中文](#中文)

---

## 中文

### 简介

FileReader 是一个 LangBot 插件，用于读取用户发送的文件并提取结构化文本供 AI 处理。它与 [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) 插件配合使用，支持多种文件格式的解析。

### 功能特点

- 📄 **多格式支持**: 支持 PDF、Word (DOCX)、Markdown、HTML、TXT 和图片文件
- 🤖 **智能处理**: 自动检测用户意图，选择合适的处理方式
- 📝 **自定义 Prompt**: 支持自定义总结、翻译、提取、问答等多种 Prompt 模板
- ⚡ **自动处理**: 收到文件后自动解析，无需手动命令
- 🔧 **灵活配置**: 支持配置最大内容长度、自动处理开关等

### 安装

1. 确保已安装 [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) 插件
2. 在 LangBot 插件市场搜索 "FileReader" 并安装
3. 或者手动下载并放置到 LangBot 的 data/plugins 目录

### 使用方法

#### 基本使用

1. 直接向机器人发送文件（PDF、Word、图片等）
2. 插件会自动解析文件内容并显示预览
3. 然后你可以向 AI 提问关于文件内容的问题

#### 示例对话

```
用户: [发送一个PDF文件]
机器人: 📄 已成功读取文件: document.pdf

        📝 内容预览:
        这是一份关于人工智能的研究报告...

        💡 请告诉我您想如何处理这个文件（例如：总结、翻译、提取关键信息、回答问题等）

用户: 请总结这个文件的主要内容
机器人: 这个文件主要讲述了以下几个方面：
        1. 人工智能的发展历史...
        2. 当前的技术趋势...
        3. 未来的应用前景...

用户: 把这个文件翻译成英文
机器人: Here is the translation:
        This is a research report on artificial intelligence...
```

### 配置选项

在 LangBot 管理界面中，可以配置以下选项：

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `default_prompt` | 默认处理提示词 | `请阅读以下文件内容，并根据用户的要求进行处理：\n\n{content}` |
| `summary_prompt` | 总结提示词 | `请总结以下文件的主要内容：\n\n{content}` |
| `translate_prompt` | 翻译提示词 | `请将以下文件内容翻译成{language}：\n\n{content}` |
| `extract_prompt` | 提取提示词 | `请从以下文件内容中提取关键信息：\n\n{content}` |
| `qa_prompt` | 问答提示词 | `基于以下文件内容回答问题：\n\n文件内容：\n{content}\n\n问题：{question}` |
| `max_content_length` | 最大内容长度 | `50000` |
| `auto_process` | 自动处理开关 | `true` |

### Prompt 模板占位符

- `{content}` - 文件内容
- `{question}` - 用户问题
- `{language}` - 目标语言

### 支持的文件格式

| 格式 | MIME 类型 | 说明 |
|------|-----------|------|
| PDF | `application/pdf` | 支持文本提取和表格识别 |
| DOCX | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | Word 文档 |
| TXT | `text/plain` | 纯文本文件 |
| MD | `text/markdown` | Markdown 文件 |
| HTML | `text/html` | HTML 文件 |
| PNG/JPG/JPEG/WEBP/GIF/BMP/TIFF | `image/*` | 图片文件（需要视觉模型支持） |

### 依赖

- [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) - 文件解析器插件（必需）

### 工作原理

1. 用户发送文件给机器人
2. FileReader 插件检测到文件消息
3. 调用 GeneralParsers 插件的 Parser 组件解析文件
4. 将解析后的文本内容存储在会话中
5. 当用户发送后续消息时，根据用户意图选择合适的 Prompt 模板
6. 将文件内容和用户请求一起发送给 AI 处理

---

## English

### Introduction

FileReader is a LangBot plugin that reads files sent by users and extracts structured text for AI processing. It works with the [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) plugin to support parsing of various file formats.

### Features

- 📄 **Multi-format Support**: Supports PDF, Word (DOCX), Markdown, HTML, TXT, and image files
- 🤖 **Smart Processing**: Automatically detects user intent and selects appropriate processing method
- 📝 **Custom Prompts**: Supports custom prompt templates for summarization, translation, extraction, Q&A, etc.
- ⚡ **Auto Processing**: Automatically parses files when received, no manual commands needed
- 🔧 **Flexible Configuration**: Supports configuring max content length, auto-processing toggle, etc.

### Installation

1. Make sure [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) plugin is installed
2. Search for "FileReader" in the LangBot plugin market and install
3. Or manually download and place in LangBot's data/plugins directory

### Usage

#### Basic Usage

1. Send a file (PDF, Word, image, etc.) directly to the bot
2. The plugin will automatically parse the file content and show a preview
3. Then you can ask AI questions about the file content

#### Example Conversation

```
User: [sends a PDF file]
Bot: 📄 Successfully read file: document.pdf

     📝 Content preview:
     This is a research report on artificial intelligence...

     💡 Please tell me how you'd like to process this file (e.g., summarize, translate, extract key information, answer questions, etc.)

User: Please summarize the main content of this file
Bot: This file mainly discusses the following aspects:
     1. The history of AI development...
     2. Current technology trends...
     3. Future application prospects...

User: Translate this file to Chinese
Bot: 以下是翻译内容：
     这是一份关于人工智能的研究报告...
```

### Configuration Options

The following options can be configured in the LangBot management interface:

| Option | Description | Default |
|--------|-------------|---------|
| `default_prompt` | Default processing prompt | `Please read the following file content and process according to user's request:\n\n{content}` |
| `summary_prompt` | Summary prompt | `Please summarize the main content of the following file:\n\n{content}` |
| `translate_prompt` | Translation prompt | `Please translate the following file content to {language}:\n\n{content}` |
| `extract_prompt` | Extraction prompt | `Please extract key information from the following file content:\n\n{content}` |
| `qa_prompt` | Q&A prompt | `Answer questions based on the following file content:\n\nFile content:\n{content}\n\nQuestion:{question}` |
| `max_content_length` | Maximum content length | `50000` |
| `auto_process` | Auto-processing toggle | `true` |

### Prompt Template Placeholders

- `{content}` - File content
- `{question}` - User question
- `{language}` - Target language

### Supported File Formats

| Format | MIME Type | Description |
|--------|-----------|-------------|
| PDF | `application/pdf` | Supports text extraction and table recognition |
| DOCX | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | Word documents |
| TXT | `text/plain` | Plain text files |
| MD | `text/markdown` | Markdown files |
| HTML | `text/html` | HTML files |
| PNG/JPG/JPEG/WEBP/GIF/BMP/TIFF | `image/*` | Image files (requires vision model support) |

### Dependencies

- [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) - File parser plugin (required)

### How It Works

1. User sends a file to the bot
2. FileReader plugin detects the file message
3. Calls GeneralParsers plugin's Parser component to parse the file
4. Stores the parsed text content in the session
5. When user sends a follow-up message, selects appropriate Prompt template based on user intent
6. Sends file content and user request together to AI for processing

---

## License

MIT License
