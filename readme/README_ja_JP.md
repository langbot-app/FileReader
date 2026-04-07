# FileReader - ファイルリーダープラグイン

> **注意**: 現在、企業微信（WeCom）スマートボットのみ対応しています。

FileReader は LangBot プラグインで、ユーザーが送信したファイルを読み取り、AI 処理用の構造化テキストを抽出します。[GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) プラグインと連携して、様々なファイル形式の解析をサポートします。

## 機能

- **マルチフォーマット対応**: PDF、Word (DOCX)、Markdown、HTML、TXT、画像ファイルに対応
- **スマート処理**: ユーザーの意図を自動検出し、適切な処理方法を選択
- **カスタムプロンプト**: 要約、翻訳、抽出、Q&A などのカスタムプロンプトテンプレートをサポート
- **自動処理**: ファイル受信時に自動解析、手動コマンド不要
- **柔軟な設定**: 最大コンテンツ長、自動処理トグルなどの設定をサポート
- **ファイルダウンロード**: URL からローカルストレージにファイルをダウンロード
- **カスタムストレージパス**: ファイル保存場所をカスタマイズ可能
- **ファイルクリーンアップ**: 全ファイルの手動削除と期限切れファイルの自動削除
- **ストレージ管理**: 最大ストレージ容量の設定

## インストール

1. [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) プラグインがインストールされていることを確認
2. LangBot プラグインマーケットで「FileReader」を検索してインストール
3. または手動でダウンロードして LangBot の data/plugins ディレクトリに配置

## 使用方法

### 基本的な使い方

1. ボットにファイル（PDF、Word、画像など）を直接送信
2. プラグインが自動的にファイル内容を解析してプレビューを表示
3. その後、ファイル内容について AI に質問できます

### 会話例

```
ユーザー: [PDFファイルを送信]
ボット: ファイルの読み取りに成功しました: document.pdf

       コンテンツプレビュー:
       これは人工知能に関する研究レポートです...

       このファイルをどのように処理しますか？（例：要約、翻訳、重要情報の抽出、質問への回答など）

ユーザー: このファイルの主な内容を要約してください
ボット: このファイルは主に以下の点について論じています：
       1. AI 発展の歴史...
       2. 現在の技術トレンド...
       3. 将来の応用展望...

ユーザー: このファイルを中国語に翻訳してください
ボット: 翻訳結果：
       これは人工知能に関する研究レポートです...
```

## 設定オプション

LangBot 管理画面で以下のオプションを設定できます：

| オプション | 説明 | デフォルト |
|-----------|------|-----------|
| `max_content_length` | 最大コンテンツ長 | `50000` |
| `auto_process` | 自動処理トグル | `true` |
| `download_path` | カスタムファイルダウンロードパス | 空（デフォルトの data ディレクトリを使用） |
| `auto_clean_days` | 指定日数を超えたファイルを自動削除 | `7` |
| `max_storage_mb` | 最大ストレージ容量（MB） | `500` |

## ファイル管理機能

### ファイルダウンロード

プラグインは `download_file(url, filename)` メソッドを提供し、URL からローカルストレージにファイルをダウンロードできます：

```python
# URL からファイルをダウンロード
file_path = await plugin.download_file("https://example.com/file.pdf")

# カスタムファイル名でダウンロード
file_path = await plugin.download_file("https://example.com/file.pdf", "my_document.pdf")
```

### 全ファイル削除

`clear_all_files()` メソッドを使用して、ダウンロードした全ファイルを削除できます：

```python
result = plugin.clear_all_files()
# 戻り値: {"success": True, "deleted_count": 10, "freed_space_mb": 25.5}
```

### ストレージ情報取得

`get_storage_info()` メソッドを使用して、現在のストレージ使用状況を取得できます：

```python
info = plugin.get_storage_info()
# 戻り値: {"storage_path": "/path/to/data", "file_count": 10, "total_size_mb": 25.5, ...}
```

### 全ファイル一覧

`list_files()` メソッドを使用して、ダウンロードした全ファイルを一覧表示できます：

```python
files = plugin.list_files()
# 戻り値: [{"name": "file.pdf", "path": "/path/to/file.pdf", "size_mb": 2.5, ...}, ...]
```

## 対応ファイル形式

| 形式 | MIME タイプ | 説明 |
|------|------------|------|
| PDF | `application/pdf` | テキスト抽出と表認識をサポート |
| DOCX | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | Word ドキュメント |
| TXT | `text/plain` | プレーンテキストファイル |
| MD | `text/markdown` | Markdown ファイル |
| HTML | `text/html` | HTML ファイル |
| PNG/JPG/JPEG/WEBP/GIF/BMP/TIFF | `image/*` | 画像ファイル（ビジョンモデルのサポートが必要） |

## 依存関係

- [GeneralParsers](https://space.langbot.app/market/langbot-team/GeneralParsers) - ファイルパーサープラグイン（必須）

## 動作原理

1. ユーザーがボットにファイルを送信
2. FileReader プラグインがファイルメッセージを検出
3. GeneralParsers プラグインの Parser コンポーネントを呼び出してファイルを解析
4. 解析されたテキスト内容をセッションに保存
5. ユーザーがフォローアップメッセージを送信すると、ユーザーの意図に基づいて適切なプロンプトテンプレートを選択
6. ファイル内容とユーザーリクエストを一緒に AI に送信して処理

## ライセンス

MIT License
