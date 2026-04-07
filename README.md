# FileReader - File Reader Plugin

> **Note**: Currently only supports WeCom (Enterprise WeChat) Intelligent Bot.

FileReader is a LangBot plugin that reads files sent by users and extracts structured text for AI processing. It works with the [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) plugin to support parsing of various file formats.

## Features

- **Multi-format Support**: Supports PDF, Word (DOCX), Markdown, HTML, TXT, and image files
- **Smart Processing**: Automatically detects user intent and selects appropriate processing method
- **Custom Prompts**: Supports custom prompt templates for summarization, translation, extraction, Q&A, etc.
- **Auto Processing**: Automatically parses files when received, no manual commands needed
- **Flexible Configuration**: Supports configuring max content length, auto-processing toggle, etc.
- **File Download**: Download files from URLs to local storage
- **Custom Storage Path**: Customize file storage location
- **File Cleanup**: Manual cleanup of all files and automatic cleanup of expired files
- **Storage Management**: Set maximum storage space limit

## Installation

1. Make sure [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) plugin is installed
2. Search for "FileReader" in the LangBot plugin market and install
3. Or manually download and place in LangBot's data/plugins directory

## Usage

### Basic Usage

1. Send a file (PDF, Word, image, etc.) directly to the bot
2. The plugin will automatically parse the file content and show a preview
3. Then you can ask AI questions about the file content

### Example Conversation

```
User: [sends a PDF file]
Bot: Successfully read file: document.pdf

     Content preview:
     This is a research report on artificial intelligence...

     Please tell me how you'd like to process this file (e.g., summarize, translate, extract key information, answer questions, etc.)

User: Please summarize the main content of this file
Bot: This file mainly discusses the following aspects:
     1. The history of AI development...
     2. Current technology trends...
     3. Future application prospects...

User: Translate this file to Chinese
Bot: Here is the translation:
     This is a research report on artificial intelligence...
```

## Configuration Options

The following options can be configured in the LangBot management interface:

| Option | Description | Default |
|--------|-------------|---------|
| `max_content_length` | Maximum content length | `50000` |
| `auto_process` | Auto-processing toggle | `true` |
| `download_path` | Custom file download path | Empty (uses default data directory) |
| `auto_clean_days` | Auto-delete files older than specified days | `7` |
| `max_storage_mb` | Maximum storage space (MB) | `500` |

## File Management Features

### Download Files

The plugin provides a `download_file(url, filename)` method to download files from URLs to local storage:

```python
# Download file from URL
file_path = await plugin.download_file("https://example.com/file.pdf")

# Download with custom filename
file_path = await plugin.download_file("https://example.com/file.pdf", "my_document.pdf")
```

### Clear All Files

Use the `clear_all_files()` method to delete all downloaded files:

```python
result = plugin.clear_all_files()
# Returns: {"success": True, "deleted_count": 10, "freed_space_mb": 25.5}
```

### Get Storage Info

Use the `get_storage_info()` method to get current storage usage:

```python
info = plugin.get_storage_info()
# Returns: {"storage_path": "/path/to/data", "file_count": 10, "total_size_mb": 25.5, ...}
```

### List All Files

Use the `list_files()` method to list all downloaded files:

```python
files = plugin.list_files()
# Returns: [{"name": "file.pdf", "path": "/path/to/file.pdf", "size_mb": 2.5, ...}, ...]
```

## Supported File Formats

| Format | MIME Type | Description |
|--------|-----------|-------------|
| PDF | `application/pdf` | Supports text extraction and table recognition |
| DOCX | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | Word documents |
| TXT | `text/plain` | Plain text files |
| MD | `text/markdown` | Markdown files |
| HTML | `text/html` | HTML files |
| PNG/JPG/JPEG/WEBP/GIF/BMP/TIFF | `image/*` | Image files (requires vision model support) |

## Dependencies

- [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) - File parser plugin (required)

## How It Works

1. User sends a file to the bot
2. FileReader plugin detects the file message
3. Calls GeneralParsers plugin's Parser component to parse the file
4. Stores the parsed text content in the session
5. When user sends a follow-up message, selects appropriate Prompt template based on user intent
6. Sends file content and user request together to AI for processing

## License

MIT License
