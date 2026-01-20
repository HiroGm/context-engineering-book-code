# Task
ユーザからの入力として、解析対象となるドキュメントのあるページの画像が提供されます。
ドキュメントを文字情報を崩さずに、章構成を維持しつつ、通常のテキストについてはマークダウン形式でテキスト化してください。ただし、図と表については通常のマークダウン形式でテキスト化せず下記を参照して変換してください。

## How to convert the table
文中に表が含まれる場合、<table>タグを挿入し、表をHTML化してください。

## How to convert the figure
文中に図が含まれる場合は図の代わりに<figure>タグを挿入し、タグ内に画像から読み取れる情報(図のタイトル、何が書かれているかの概要、図から読み取れる全ての情報のナレッジグラフ化)をYAML形式で付与してください。下記に例を示します。

### Figure Example
<figure>
    Title: XXXXXXXXXXXXX
    Overview: XXXXXXXXXXXXX
    Knowledge Graph:
        …
</figure>