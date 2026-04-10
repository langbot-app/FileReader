# FileReader - 文件阅读器插件

> **注意**: 目前仅支持企业微信智能机器人,钉钉机器人和飞书机器人。

FileReader 是一个 LangBot 插件，用于读取用户发送的文件并提取结构化文本供 AI 处理。它与 [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) 插件配合使用，支持多种文件格式的解析。

## 功能特点

- **多格式支持**: 支持 PDF、Word (DOCX)、Markdown、HTML、TXT 和图片文件
- **智能处理**: 自动检测用户意图，选择合适的处理方式
- **自定义 Prompt**: 支持自定义总结、翻译、提取、问答等多种 Prompt 模板
- **自动处理**: 收到文件后自动解析，无需手动命令
- **灵活配置**: 支持配置最大内容长度、自动处理开关等
- **文件下载**: 支持从URL下载文件到本地存储
- **自定义存储路径**: 支持自定义文件存储位置
- **文件清理**: 支持手动清除所有文件，以及自动清理过期文件
- **存储空间管理**: 支持设置最大存储空间限制

## 安装

1. 确保已安装 [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) 插件
2. 在 LangBot 插件市场搜索 "FileReader" 并安装
3. 或者手动下载并放置到 LangBot 的 data/plugins 目录

## 使用方法

### 基本使用

1. 私聊中直接向机器人发送文件（PDF、Word、图片等），群聊中先发送文件，然后@机器人时引用文件。
2. 插件会自动解析文件内容并显示预览
3. 然后你可以向 AI 提问关于文件内容的问题

### 示例对话

```
用户: [发送一个PDF文件]
机器人: 已成功读取文件: document.pdf

        内容预览:
        这是一份关于人工智能的研究报告...

        请告诉我您想如何处理这个文件（例如：总结、翻译、提取关键信息、回答问题等）

用户: 请总结这个文件的主要内容
机器人: 这个文件主要讲述了以下几个方面：
        1. 人工智能的发展历史...
        2. 当前的技术趋势...
        3. 未来的应用前景...

用户: 把这个文件翻译成英文
机器人: Here is the translation:
        This is a research report on artificial intelligence...
```

## 配置选项

在 LangBot 管理界面中，可以配置以下选项：

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `max_content_length` | 最大内容长度 | `50000` |
| `auto_process` | 自动处理开关 | `true` |
| `download_path` | 自定义文件下载保存路径 | 空（使用默认data目录） |
| `auto_clean_days` | 自动清理超过指定天数的文件 | `7` |
| `max_storage_mb` | 最大存储空间（MB） | `500` |

## 文件管理功能

### 下载文件

插件提供了 `download_file(url, filename)` 方法，可以从URL下载文件到本地存储：

```python
# 从URL下载文件
file_path = await plugin.download_file("https://example.com/file.pdf")

# 指定文件名下载
file_path = await plugin.download_file("https://example.com/file.pdf", "my_document.pdf")
```

### 清除所有文件

使用 `clear_all_files()` 方法可以清除所有已下载的文件：

```python
result = plugin.clear_all_files()
# 返回: {"success": True, "deleted_count": 10, "freed_space_mb": 25.5}
```

### 获取存储信息

使用 `get_storage_info()` 方法可以获取当前存储空间使用情况：

```python
info = plugin.get_storage_info()
# 返回: {"storage_path": "/path/to/data", "file_count": 10, "total_size_mb": 25.5, ...}
```

### 列出所有文件

使用 `list_files()` 方法可以列出所有已下载的文件：

```python
files = plugin.list_files()
# 返回: [{"name": "file.pdf", "path": "/path/to/file.pdf", "size_mb": 2.5, ...}, ...]
```

## 支持的文件格式

| 格式 | MIME 类型 | 说明 |
|------|-----------|------|
| PDF | `application/pdf` | 支持文本提取和表格识别 |
| DOCX | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | Word 文档 |
| TXT | `text/plain` | 纯文本文件 |
| MD | `text/markdown` | Markdown 文件 |
| HTML | `text/html` | HTML 文件 |
| PNG/JPG/JPEG/WEBP/GIF/BMP/TIFF | `image/*` | 图片文件（需要视觉模型支持） |

## 依赖

- [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) - 文件解析器插件（必需）

## 工作原理

1. 用户发送文件给机器人
2. FileReader 插件检测到文件消息
3. 调用 GeneralParsers 插件的 Parser 组件解析文件
4. 将解析后的文本内容存储在会话中
5. 当用户发送后续消息时，根据用户意图选择合适的 Prompt 模板
6. 将文件内容和用户请求一起发送给 AI 处理

## 许可证

MIT License
