<!-- ==================================================================================================== -->
<!-- ==================================================================================================== -->
<!-- ==================================================================================================== -->
<!-- ==================================================================================================== -->

# 「LLMの原理、RAG、エージェント開発から読み解く　コンテキストエンジニアリング」サンプルコード
このリポジトリは、書籍「LLMの原理、RAG、エージェント開発から読み解く　コンテキストエンジニアリング」で扱うサンプルコードを、お手元で試せる形にまとめたものです。

本文は、コードを実行しなくても読み進められるように書かれておりますが、紙面の都合で出力結果を省略している箇所もございます。実際の挙動を確かめたいときに、どうぞご活用ください。

![表紙](image/image.png)

書籍ページ  
https://gihyo.jp/book/2026/978-4-297-15419-6

## はじめに
- 本リポジトリの中心は、各章のノートブックです（`chapter2.ipynb` など）。
- ノートブック内では、ルート直下の `config.yaml` を読み込み、OpenAI API を呼び出します。
- 入力サンプルは `reference` に、変換結果などの出力は `result` に保存されます。

## 注意点
本書は、実装方法そのものを体系的に学ぶことを主目的としておりません。API や SDK は更新されることがございますので、実装を進める際は公式ドキュメントもあわせてご確認ください。

OpenAI 公式ドキュメント（Responses API）  
https://platform.openai.com/docs/api-reference/responses

また、API 呼び出しには利用料金が発生する場合がございます。実行前に、ご自身の契約内容や上限設定をご確認いただけますと安心です。

## クイックスタート
### 1. リポジトリを取得します
```bash
git clone https://github.com/HiroGm/context-engineering-book-code
cd context-engineering-book-code
```

### 2. Python 環境を用意します
推奨は Python 3.11 系です。

#### Windows（PowerShell の例）
```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r library\requirements.txt
```

#### macOS、Linux の例
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r library/requirements.txt
```

### 3. config.yaml を用意します
ルート直下の `config.yaml` に、API キーを設定してください。ファイルが見当たらない場合は、新規に作成していただいて大丈夫です。

`config.yaml` は `.gitignore` に含まれておりますので、公開リポジトリへ誤って含めないようにする意図があります。キーは外部へ共有せず、大切に取り扱ってくださいませ。

```yaml
oai:
  key: YOUR_OPENAI_API_KEY
```

もしキーを外部へ共有してしまった可能性がある場合は、念のため無効化や再発行をご検討ください。

### 4. ノートブックを開いて実行します
Visual Studio Code をお使いの場合は、拡張機能の Python と Jupyter を入れて、ノートブックを開いてください。

カーネルは、仮想環境の Python を選択するとスムーズです。必要に応じて、次のコマンドでカーネル登録も行えます。

```bash
python -m ipykernel install --user --name context_book --display-name context_book
```

## ノートブック一覧
- 第2章 `chapter2.ipynb`
- 第3章 `chapter3.ipynb`
- 第4章 `chapter4.ipynb`
- 第5章 `chapter5.ipynb`

いずれのノートブックも、リポジトリのルートをカレントディレクトリとして実行する前提で、相対パスを記述しております。フォルダを開く場所が異なると、`reference` や `result` が見つからない場合がございますので、その点だけご注意ください。

## フォルダ構成
- `library` 依存ライブラリ定義（`requirements.txt` など）
- `reference` 入力サンプル（PDF、Word、テキスト、画像など）
- `result` 変換結果や中間生成物の出力先
- `image` README 用の画像

## よくあるつまずき
### モジュールが見つからないと言われます
仮想環境が有効になっているか、また `library/requirements.txt` のインストールが完了しているかをご確認ください。

### 第4章で変換が失敗します
第4章では、PDF や Word を扱う処理が含まれます。環境によっては追加の外部ツールが必要になることがございますので、ノートブック内のエラーメッセージに従って導入してください。

## 書籍の誤りや、発生したエラーについて
恐れ入りますが、GitHub の Issue にご連絡いただけますと助かります。  
https://github.com/HiroGm/context-engineering-book-code/issues

## ライセンス
MIT License です。詳細は `LICENSE` をご覧ください。
