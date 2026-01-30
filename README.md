<!-- ==================================================================================================== -->
<!-- ==================================================================================================== -->
<!-- ==================================================================================================== -->
<!-- ==================================================================================================== -->

# 「LLMの原理、RAG、エージェント開発から読み解く　コンテキストエンジニアリング」サンプルコード
このリポジトリは、書籍「LLMの原理、RAG、エージェント開発から読み解く　コンテキストエンジニアリング」で扱うサンプルコードを、お手元で試せる形にまとめたものです。

本文は、コードを実行しなくても読み進められるように書かれておりますが、紙面の都合で出力結果を省略している箇所もございます。実際の挙動を確かめたいときに、どうぞご活用ください。

<img src="image/image.png" alt="表紙" width="200"/>


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

## 前提環境の準備

サンプルコードを動かすには、以下のツールが必要です。初めての方は、このセクションを参考にセットアップしてください。

### Git のインストール

Git は、ソースコードのバージョン管理ツールです。このリポジトリをダウンロード（クローン）するために使用します。

#### インストール方法
- **Windows**: [Git for Windows](https://gitforwindows.org/) からインストーラーをダウンロードして実行します。インストール時の設定は、基本的にデフォルトのままで問題ありません。
- **macOS**: ターミナルで `git --version` を実行すると、未インストールの場合はインストールを促されます。または [Homebrew](https://brew.sh/ja/) で `brew install git` を実行します。
- **Linux**: お使いのディストリビューションのパッケージマネージャーで導入できます（例: `sudo apt install git`）。

#### インストール確認
ターミナル（Windows の場合は PowerShell）で以下を実行し、バージョンが表示されれば成功です。
```bash
git --version
```

> 📚 参考: [Git 公式ドキュメント](https://git-scm.com/book/ja/v2)

---

### Python のインストール

本書では **Python 3.11 系**を推奨しています。

#### インストール方法
- **Windows**: [Python 公式サイト](https://www.python.org/downloads/) からインストーラーをダウンロードします。  
  ⚠️ インストール時に **「Add Python to PATH」にチェック**を入れてください。これを忘れると、ターミナルから `python` コマンドが使えません。
- **macOS**: [Python 公式サイト](https://www.python.org/downloads/) からダウンロードするか、Homebrew で `brew install python@3.11` を実行します。
- **Linux**: 多くのディストリビューションにはプリインストールされていますが、バージョンが古い場合は `pyenv` などで管理するのがおすすめです。

#### インストール確認
```bash
python --version
```
`Python 3.11.x` のように表示されれば成功です（macOS/Linux では `python3 --version` の場合もあります）。

> 📚 参考: [Python 公式チュートリアル](https://docs.python.org/ja/3/tutorial/)

---

### OpenAI API キーの取得

本書のサンプルコードでは、OpenAI の API を使用します。API を利用するには、API キーが必要です。

#### 取得手順
1. [OpenAI Platform](https://platform.openai.com/) にアクセスし、アカウントを作成またはログインします。
2. 右上のアイコンから **「API keys」** を選択します（または直接 https://platform.openai.com/api-keys にアクセス）。
3. **「Create new secret key」** をクリックして、新しいキーを作成します。
4. 表示されたキー（`sk-...` で始まる文字列）を**安全な場所にコピー**して保存します。  
   ⚠️ このキーは一度しか表示されません。閉じてしまうと再表示できないため、必ずコピーしてください。

#### 料金について
- OpenAI API は従量課金制です。無料枠もありますが、使用量に応じて料金が発生します。
- 事前に [Usage ページ](https://platform.openai.com/usage) で上限設定（Spending Limits）を確認しておくと安心です。

> 📚 参考: [OpenAI API ドキュメント](https://platform.openai.com/docs/overview)

---

## クイックスタート

前提環境が整ったら、以下の手順でサンプルコードを動かしてみましょう。

### 1. リポジトリを取得します

ターミナル（Windows の場合は PowerShell）を開き、作業したいフォルダに移動してから以下を実行します。

```bash
git clone https://github.com/HiroGm/context-engineering-book-code
cd context-engineering-book-code
```

**コマンドの解説:**
- `git clone <URL>`: 指定した URL のリポジトリを、現在のフォルダにダウンロード（複製）します。
- `cd <フォルダ名>`: 指定したフォルダに移動します（cd = change directory）。

---

### 2. Python 仮想環境を作成します

Python では「仮想環境」を使って、プロジェクトごとに独立したライブラリ環境を作るのが一般的です。これにより、他のプロジェクトとライブラリのバージョンが衝突するのを防げます。

#### Windows（PowerShell の例）
```bash
python -m venv .venv
```

**コマンドの解説:**
- `python -m venv`: Python の標準機能である `venv` モジュールを使って仮想環境を作成します。
- `.venv`: 仮想環境を格納するフォルダ名です。慣例的に `.venv` や `venv` がよく使われます。

次に、作成した仮想環境を「有効化」します。

```bash
.\.venv\Scripts\Activate.ps1
```

**コマンドの解説:**
- このスクリプトを実行すると、ターミナルのプロンプトの先頭に `(.venv)` と表示され、仮想環境が有効になったことを示します。
- 以降、`pip install` などは仮想環境内にインストールされます。

> ⚠️ PowerShell でスクリプト実行がブロックされる場合は、以下を先に実行してください:
> ```bash
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```

最後に、必要なライブラリをインストールします。

```bash
cd library
pip install -r requirements.txt
```

**コマンドの解説:**
- `pip install -r <ファイル>`: 指定したファイルに書かれたライブラリを一括インストールします。
- `requirements.txt` には、本書のサンプルコードに必要なライブラリがリストアップされています。

#### macOS / Linux の例
```bash
python3 -m venv .venv
source .venv/bin/activate
cd library
pip install -r requirements.txt
```

Windows との違い:
- `python3` コマンドを使用します（環境によっては `python` でも可）。
- 仮想環境の有効化は `source` コマンドを使います。

---

### 3. config.yaml を作成します

ルート直下に `config.yaml` ファイルを作成し、先ほど取得した OpenAI API キーを設定します。

```yaml
oai:
  key: YOUR_OPENAI_API_KEY
```

`YOUR_OPENAI_API_KEY` の部分を、実際のキー（`sk-...` で始まる文字列）に置き換えてください。

**例:**
```yaml
oai:
  key: sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

> ⚠️ **セキュリティに関する重要な注意**
> - `config.yaml` は `.gitignore` に含まれており、Git にコミットされません。
> - API キーは**絶対に他人と共有しないでください**。SNS やブログに投稿するのも厳禁です。
> - 万が一キーが漏洩した可能性がある場合は、[API keys ページ](https://platform.openai.com/api-keys) から即座に無効化し、新しいキーを発行してください。

---

### 4. ノートブックを開いて実行します
Visual Studio Code をお使いの場合は、拡張機能の Python と Jupyter をインストールし、ノートブックを開いてください。

実行の前に右上のアイコンから実行カーネルを選択できますので、作成した仮想環境の Python を選択するとスムーズです。必要に応じて、仮想環境内で次のコマンドでカーネル登録も可能です。

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
