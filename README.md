# [Markdown-CSS](https://www.jsdelivr.com/package/gh/Star-Delta/Markdown-CSS)
## 目次
1. [目次](#目次)
2. [概要](#概要)
   1. [主な仕様](#主な仕様)
3. [利用方法](#利用方法)
   1. [ブラウザで使用する](#ブラウザで使用する)
   2. [VS Code Markdown Previewで使用する](#vs-code-markdown-previewで使用する)
   3. [CSSをダウンロードする](#cssをダウンロードする)
4. [ファイル構成](#ファイル構成)
   1. [コアファイル](#コアファイル)
   2. [最適化CSS](#最適化css)
   3. [拡張CSS](#拡張css)
5. [その他](#その他)
   1. [カスタマイズ](#カスタマイズ)
   2. [表示上の仕様](#表示上の仕様)
   3. [License](#license)

## 概要
CommonMarkおよびGitHub Flavored Markdown（GFM）に沿って生成されたHTMLを読みやすく表示するためのCSSです。主にMarked.jsやMarkdown-itの標準機能で変換したHTMLを対象としています。

共通スタイルに加え、VS CodeのMarkdown Previewと印刷向けの追加CSSを収録しています。

### 主な仕様
- OS／ブラウザの設定に応じたライト・ダークテーマ
- H2・H3の自動採番
- H6を注釈として表示するスタイル

#### H6を注釈として使用する

`Markdown.css`では、H6を通常の見出しではなく注釈として表示します。H6に注釈の識別子を記述し、その直後のParagraphに注釈本文を記述します。

```markdown
###### 1

これは注釈の本文です。
```

H6の前後には`[*`と`]`が追加され、次のような注釈として表示されます。

```text
[*1]
これは注釈の本文です。
```

注釈の識別子は自動採番されないため、H6へ明示的に記述してください。小さい文字と詰めた上余白が適用される注釈本文は、H6の直後にある1つのParagraphだけです。

## 利用方法
特に制限がない限りCDN経由の利用を推奨しています。

### ブラウザで使用する

HTMLの`head`でCSSを読み込み、Markdownから生成したHTMLを`.markdown-body`内へ配置します。

```html
<head>
  <link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown.css">
</head>
<body>
  <div class="markdown-body">
    <!-- Markdownから生成したHTML -->
  </div>
</body>

```

画面表示そのままではなく社内マニュアル風に印刷したい場合は、以下のCSSも読み込んでください。
```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_Print.css"
  media="print">
```

ローカルのCSSを使用する場合は、`href`をそれぞれのファイルパスへ変更してください。

### VS Code Markdown Previewで使用する

VS Codeの`settings.json`で、`markdown.styles`へ`Markdown.css`、`Markdown_VSCode.css`の順に指定します。`Markdown_VSCode.css`は単体ではなく、`Markdown.css`と一緒に読み込むことを前提としています。

```json
{
  "markdown.styles": [
    "https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown.css",
    "https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_VSCode.css"
  ]
}
```

VS Codeは`https` URLのほか、現在のWorkspaceを基準とした相対パスも読み込めます。ローカルで編集結果を確認する場合は、例えば次のように指定します。

```json
{
  "markdown.styles": [
    "./CSS/Markdown.css",
    "./CSS/Markdown_VSCode.css"
  ]
}
```

設定方法の詳細は、VS Code公式ドキュメントの[Using your own CSS](https://code.visualstudio.com/docs/languages/markdown#_using-your-own-css)を参照してください。

### CSSをダウンロードする
[GitHub](https://github.com/Star-Delta/Markdown-CSS/tags)から最新のバージョンをダウンロードしてください。

## ファイル構成
当CSSは[JSDELIVR](https://www.jsdelivr.com/package/gh/Star-Delta/Markdown-CSS)にて配信しています。
| コアファイル | ファイル(CDNへのリンク)                                                            | 備考 |
| ------------ | ---------------------------------------------------------------------------------- | ---- |
| CSS          | [Markdown.css](https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown.css) |      |

| 最適化CSS  | ファイル(CDNへのリンク)                                                                          | 用途                                              | 備考 |
| ---------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------- | ---- |
| VSCode向け | [Markdown_VSCode.css](https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_VSCode.css) | `Markdown.css`をVS Code向けに微調整を行うスタイル |      |

| 拡張CSS          | ファイル(CDNへのリンク)                                                                        | 用途                                               | 備考                      |
| ---------------- | ---------------------------------------------------------------------------------------------- | -------------------------------------------------- | ------------------------- |
| マニュアル風印刷 | [Markdown_Print.css](https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_Print.css) | `Markdown.css`をマニュアル風にカスタムするスタイル |                           |
| MARP             | `MARP.css`                                                                                     | `Markdown.css`のMarp for VS Code向けスタイル       | 未完成のためCDN配信対象外 |

### コアファイル
#### CSS
当CSSでは、画面表示に必要なスタイルと印刷する際に必要な最低限のスタイルを記述しています。
- 表示用と印刷用で色の設定を変更
- 横幅を超えるTableとCodeBlockはスクロールバーによる横スクロール(印刷時は自動的に解除)
- 下記以外の要素対して左右インデントを付与
  - H2
  - H3
  - H6(およびそれに隣接する段落)
  - hr

### 最適化CSS
#### VSCode向け
`Markdown_VSCode.css`では、VS Codeが生成する`.vscode-body`を対象に以下を調整します。

- VS Codeのライト・ダークテーマに対応した本文やスクロールバー、フォーム部品の配色
- Mermaidの横幅と初期化用要素
- Preview先頭要素の上余白
- CodeBlockコピーボタンを収める最小高さ

コピーボタンの最小高さは、Windows版VS Codeで確認した下限値を基に`50px`としています。他のOSや将来のVS CodeでButtonの寸法が変わった場合は、調整が必要になる可能性があります。

### 拡張CSS
#### マニュアル風印刷
`Markdown_Print.css`は、`Markdown.css`をなんかJTCで使われてそうな社内マニュアルっぽい印刷レイアウトへ調整する拡張CSSです。
- 文書先頭のH1とそれに隣接する段落をページ中央付近に配置
- H2の前で改ページ

当CSSを適用しても通常の画面表示には影響しません。実際の印刷イメージは印刷プレビュー機能などで確認してください。

当CSSはWebブラウザの印刷機能を用いて動作確認しています。
VS CodeにはMarkdown PreviewをCSSの印刷レイアウトとして出力する標準機能がないため、動作確認を行っていません。  
そのため、VSCode拡張機能を用いた印刷については保証しません。

#### MARP
`MARP.css`は、`Markdown.css`のスタイルを基になんかJTCで使われてそうなパワポっぽいレイアウトを提供するCSSです。

現在はほとんど使用しておらず、抜本的な見直しも完了していません。基本となるCSS群のメンテナンス完了後に対応する予定のため、現時点では試験的なファイルとして扱い、CDN配信対象には含めません。

VS Code拡張機能のMarp for VS Codeで使用することを想定しています。

## その他
### カスタマイズ

`Markdown.css`の`.markdown-body`で、基本寸法をCSSカスタムプロパティとして定義しています。

| プロパティ         | 初期値             | 用途                      |
| ------------------ | ------------------ | ------------------------- |
| `--font-size`      | `1rem`             | 本文と各要素の寸法基準    |
| `--line-height`    | `1.5`              | 本文の行高                |
| `--margin-bottom`  | `var(--font-size)` | Block要素下部の基本余白   |
| `--content-indent` | `var(--font-size)` | 左右インデント            |
| `--border`         | `none`             | `.markdown-body`のborder  |
| `--padding`        | `0`                | `.markdown-body`のpadding |

色は画面用ライト・画面用ダーク・印刷用ごとに、以下の系列で定義しています。

- `--color-font`
- `--color-background`、`--color-background-emphasis`、`--color-background-muted`
- `--color-border`、`--color-border-subtle`
- `--color-heading-default`、`--color-heading-muted`、`--color-heading-subtle`
- `--color-syntax`：インラインコードの文字色

利用側のCSSで同じカスタムプロパティを上書きすることで配色や寸法を変更できます。

### 表示上の仕様

- H2とH3の番号はCSS Counterで生成されるため、Markdown本文そのものには追加されません。
- H1でH2・H3の番号をリセットし、H2でH3の番号をリセットします。H2を省略してH3から始めた場合は、`0.1`から採番します。
- H1からH5までの行高は文字サイズの1.2倍です。
- H6は通常の小見出しではなく、注釈としての利用を想定しています。
- 強調（`strong`）は周囲の文字色を引き継ぎ、太字で表示します。
- 画面表示のTable Cellは自動改行せず、必要な場合にTable内を横スクロールします。
- CodeBlockは長い行を折り返さず横スクロールし、縦方向は全行を表示します。
- 本文・リンク・見出し・インラインコードは、長い連続文字列でも表示領域内で改行します。
- 印刷時はTableとCodeBlockを用紙幅内で改行します。
- CSS Nesting、`:has()`、`:nth-child(... of ...)`などを使用しているため、対応する比較的新しいブラウザ／Preview環境が必要です。

### License

[Mozilla Public License 2.0](LICENSE)
