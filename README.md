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
- 📥 **文件下载**: 支持从URL下载文件到本地存储
- 📁 **自定义存储路径**: 支持自定义文件存储位置
- 🗑️ **文件清理**: 支持手动清除所有文件，以及自动清理过期文件
- 💾 **存储空间管理**: 支持设置最大存储空间限制

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
| `download_path` | 自定义文件下载保存路径 | 空（使用默认data目录） |
| `auto_clean_days` | 自动清理超过指定天数的文件 | `7` |
| `max_storage_mb` | 最大存储空间（MB） | `500` |
| `encoding_aes_key` | 企业微信消息加密密钥（EncodingAESKey） | 空 |

### 企业微信文件解密配置

如果您使用企业微信智能机器人，企业微信会对发送的文件进行 AES 加密。要正确读取文件内容，需要配置 `encoding_aes_key`：

1. 登录[企业微信管理后台](https://work.weixin.qq.com/wework_admin/frame)
2. 进入「应用管理」→ 选择您的机器人应用
3. 在「接收消息」设置中找到 **EncodingAESKey**
4. 将这个 43 字符的密钥复制到插件配置的 `encoding_aes_key` 字段

**注意**：
- EncodingAESKey 是一个 43 字符的 Base64 编码字符串
- 如果不配置此项，从企业微信下载的文件将显示为乱码
- 此功能需要安装 `pycryptodome` 库（已包含在 requirements.txt 中）

### 文件管理功能

#### 下载文件

插件提供了 `download_file(url, filename)` 方法，可以从URL下载文件到本地存储：

```python
# 从URL下载文件
file_path = await plugin.download_file("https://example.com/file.pdf")

# 指定文件名下载
file_path = await plugin.download_file("https://example.com/file.pdf", "my_document.pdf")
```

#### 清除所有文件

使用 `clear_all_files()` 方法可以清除所有已下载的文件：

```python
result = plugin.clear_all_files()
# 返回: {"success": True, "deleted_count": 10, "freed_space_mb": 25.5}
```

#### 获取存储信息

使用 `get_storage_info()` 方法可以获取当前存储空间使用情况：

```python
info = plugin.get_storage_info()
# 返回: {"storage_path": "/path/to/data", "file_count": 10, "total_size_mb": 25.5, ...}
```

#### 列出所有文件

使用 `list_files()` 方法可以列出所有已下载的文件：

```python
files = plugin.list_files()
# 返回: [{"name": "file.pdf", "path": "/path/to/file.pdf", "size_mb": 2.5, ...}, ...]
```

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
- 📥 **File Download**: Download files from URLs to local storage
- 📁 **Custom Storage Path**: Customize file storage location
- 🗑️ **File Cleanup**: Manual cleanup of all files and automatic cleanup of expired files
- 💾 **Storage Management**: Set maximum storage space limit

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
| `download_path` | Custom file download path | Empty (uses default data directory) |
| `auto_clean_days` | Auto-delete files older than specified days | `7` |
| `max_storage_mb` | Maximum storage space (MB) | `500` |
| `encoding_aes_key` | WeCom EncodingAESKey for file decryption | Empty |

### WeCom File Decryption Configuration

If you're using WeCom (Enterprise WeChat) smart bot, WeCom encrypts files sent through the platform using AES encryption. To properly read file contents, you need to configure `encoding_aes_key`:

1. Log in to [WeCom Admin Console](https://work.weixin.qq.com/wework_admin/frame)
2. Go to "Application Management" → Select your bot application
3. Find **EncodingAESKey** in the "Receive Messages" settings
4. Copy this 43-character key to the plugin's `encoding_aes_key` configuration field

**Notes**:
- EncodingAESKey is a 43-character Base64 encoded string
- Without this configuration, files downloaded from WeCom will appear as garbled text
- This feature requires the `pycryptodome` library (included in requirements.txt)

### File Management Features

#### Download Files

The plugin provides a `download_file(url, filename)` method to download files from URLs to local storage:

```python
# Download file from URL
file_path = await plugin.download_file("https://example.com/file.pdf")

# Download with custom filename
file_path = await plugin.download_file("https://example.com/file.pdf", "my_document.pdf")
```

#### Clear All Files

Use the `clear_all_files()` method to delete all downloaded files:

```python
result = plugin.clear_all_files()
# Returns: {"success": True, "deleted_count": 10, "freed_space_mb": 25.5}
```

#### Get Storage Info

Use the `get_storage_info()` method to get current storage usage:

```python
info = plugin.get_storage_info()
# Returns: {"storage_path": "/path/to/data", "file_count": 10, "total_size_mb": 25.5, ...}
```

#### List All Files

Use the `list_files()` method to list all downloaded files:

```python
files = plugin.list_files()
# Returns: [{"name": "file.pdf", "path": "/path/to/file.pdf", "size_mb": 2.5, ...}, ...]
```

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
