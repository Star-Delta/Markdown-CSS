# [Markdown-CSS](https://www.jsdelivr.com/package/gh/Star-Delta/Markdown-CSS)
## 目次
1. [目次](#目次)
2. [概要](#概要)
   1. [主な仕様](#主な仕様)
3. [利用方法](#利用方法)
   1. [ブラウザで使用する](#ブラウザで使用する)
   2. [VS Code Markdown Previewで使用する](#vs-code-markdown-previewで使用する)
   3. [印刷用スタイルを適用する](#印刷用スタイルを適用する)
4. [ファイル構成](#ファイル構成)
   1. [コアファイル](#コアファイル)
   2. [VSCode向け](#vscode向け)
   3. [マニュアル風印刷](#マニュアル風印刷)
   4. [MARP](#marp)
5. [その他](#その他)
   1. [表示上の仕様](#表示上の仕様)
   2. [License](#license)

## 概要
CommonMarkおよびGitHub Flavored Markdown（GFM）に沿って生成されたHTMLを読みやすく表示するためのCSSです。主にMarked.jsやMarkdown-itの標準機能で変換したHTMLを対象としています。


### 主な仕様
- ライト・ダークテーマに対応
- Webブラウザ向けのコアCSSと、VSCode・印刷用に調整した拡張CSSを用意

## 利用方法
CDN経由の利用を推奨しています。  
CSSをダウンロードして使用する場合は、[GitHub](https://github.com/Star-Delta/Markdown-CSS/tags)から最新のバージョンをダウンロードいただき、以下に記載している設定方法のCDNのURLをダウンロードしたファイルのファイルパスに変更してください。


### ブラウザで使用する

HTMLの`head`でCSSを読み込み、Markdownから生成したHTMLを`.markdown-body`内へ配置します。

```html
<head>
  <link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0.6/Markdown.css">
</head>
<body>
  <div class="markdown-body">
    <!-- Markdownから生成したHTML -->
  </div>
</body>

```

### VS Code Markdown Previewで使用する

VS Codeの`settings.json`で、`markdown.styles`へ`Markdown.css`、`Markdown_VSCode.css`の順に指定します。`Markdown_VSCode.css`は単体ではなく、`Markdown.css`と一緒に読み込むことを前提としています。

```json
{
  "markdown.styles": [
    "https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0.6/Markdown.css",
    "https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0.6/Markdown_VSCode.css"
  ]
}
```

設定方法の詳細は、VS Code公式ドキュメントの[Using your own CSS](https://code.visualstudio.com/docs/languages/markdown#_using-your-own-css)を参照してください。

### 印刷用スタイルを適用する
画面表示そのままではなく社内マニュアル風に印刷したい場合は、以下のCSSも読み込んでください。
```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0.6/Markdown_Print.css"
  media="print">
```


## ファイル構成
当CSSは、Webブラウザで表示するために必要なスタイルを記載したコアCSSと、Webブラウザ以外で表示するための微調整を記載した拡張CSSで構成されています。
| CSS              | ファイル(CDNへのリンク)                                                                          | 用途                                               | 備考                      |
| ---------------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------- | ------------------------- |
| コアCSS          | [Markdown.css](https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown.css)               | Webブラウザ向けに書かれた基本となるCSS             |
| VSCode向け       | [Markdown_VSCode.css](https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_VSCode.css) | `Markdown.css`をVS Code向けに微調整を行うスタイル  |                           |
| マニュアル風印刷 | [Markdown_Print.css](https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_Print.css)   | `Markdown.css`をマニュアル風にカスタムするスタイル |                           |
| MARP             | `MARP.css`                                                                                       | `Markdown.css`のMarp for VS Code向けスタイル       | 未完成のためCDN配信対象外 |

### コアファイル
当CSSでは、画面表示に必要なスタイルと印刷する際に必要な最低限のスタイルを記述しています。
- ライトテーマ・ダークテーマ・印刷用テーマで色の設定を変更
- h2,3,4に自動採番
  > [!TIP]
  > H1でH2・H3の番号をリセットし、H2でH3の番号をリセットします。  
  > H2を省略してH3から始めた場合は、`0.1`から採番します。
- 横幅を超えるTableとCodeBlockはスクロールバーによる横スクロール(印刷時は自動的に解除)

### VSCode向け
`Markdown_VSCode.css`では、VS Codeが生成する`.vscode-body`を対象に以下を調整します。

- VS Codeのライト・ダークテーマに対応した本文やスクロールバー、フォーム部品の配色
- Mermaidの横幅と初期化用要素
- Preview先頭要素の上余白
- CodeBlockコピーボタンを収める最小高さ

コピーボタンの最小高さは、Windows版VS Codeで確認した下限値を基に`50px`としています。他のOSや将来のVS CodeでButtonの寸法が変わった場合は、調整が必要になる可能性があります。

### マニュアル風印刷
`Markdown_Print.css`は、`Markdown.css`をなんかJTCで使われてそうな社内マニュアルっぽい印刷レイアウトへ調整する拡張CSSです。
- 文書先頭のH1とそれに隣接する段落をページ中央付近に配置
- H2の前で改ページ

当CSSを適用しても通常の画面表示には影響しません。実際の印刷イメージは印刷プレビュー機能などで確認してください。

当CSSはWebブラウザの印刷機能を用いて動作確認しています。
VS CodeにはMarkdown PreviewをCSSの印刷レイアウトとして出力する標準機能がないため、動作確認を行っていません。  
そのため、VSCode拡張機能を用いた印刷については保証しません。

### MARP
`MARP.css`は、`Markdown.css`のスタイルを基になんかJTCで使われてそうなパワポっぽいレイアウトを提供するCSSです。

現在はほとんど使用しておらず、抜本的な見直しも完了していません。基本となるCSS群のメンテナンス完了後に対応する予定のため、現時点では試験的なファイルとして扱い、CDN配信対象には含めません。

VS Code拡張機能のMarp for VS Codeで使用することを想定しています。

## その他
### 表示上の仕様
- CodeBlockは長い行を折り返さず横スクロールし、縦方向は全行を表示します。
- 本文・リンク・見出し・インラインコードは、長い連続文字列でも表示領域内で改行します。
- 印刷時はTableとCodeBlockを用紙幅内で改行します。
- CSS Nesting、`:has()`、`:nth-child(... of ...)`などを使用しているため、対応する比較的新しいブラウザ／Preview環境が必要です。

### License

[Mozilla Public License 2.0](LICENSE)

