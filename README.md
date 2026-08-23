# [Markdown-CSS](https://www.jsdelivr.com/package/gh/Star-Delta/Markdown-CSS)

CommonMarkおよびGitHub Flavored Markdown（GFM）に沿って生成されたHTMLを読みやすく表示するためのCSSです。主にMarked.jsやMarkdown-itの標準機能で変換したHTMLを対象としています。

共通スタイルに加え、VS CodeのMarkdown Previewと印刷向けの追加CSSを収録しています。

## 目次
1. [目次](#目次)
2. [機能と構成](#機能と構成)
   1. [ファイル構成](#ファイル構成)
   2. [H6を注釈として使用する](#h6を注釈として使用する)
3. [利用方法](#利用方法)
   1. [ブラウザで使用する](#ブラウザで使用する)
   2. [VS Code Markdown Previewで使用する](#vs-code-markdown-previewで使用する)
   3. [印刷](#印刷)
   4. [Marp](#marp)
4. [その他](#その他)
   1. [カスタマイズ](#カスタマイズ)
   2. [表示上の仕様](#表示上の仕様)
   3. [License](#license)

## 機能と構成
- OS／ブラウザの設定に応じたライト・ダークテーマ
- H1からH5までの見出し装飾と、H2・H3の自動採番
- H6を注釈として表示するスタイル
- 本文のインデントと、見出し・本文間の余白調整
- 横幅を超えるTableとCodeBlockの横スクロール
- Tableの外周、列間、HeaderとBodyの境界、Body内の行間を区別した罫線
- Table Bodyの縞模様
- 長いインラインコードの自動改行
- 印刷時のTableとCodeBlockのスクロール解除および自動改行
- VS Code Markdown Preview固有のMermaidとCodeBlockコピーボタンの調整

### ファイル構成

| ファイル              | 用途                                                         | URL                                                                         |
| --------------------- | ------------------------------------------------------------ | --------------------------------------------------------------------------- |
| `Markdown.css`        | Markdown用スタイル                                           | `https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown.css`        |
| `Markdown_VSCode.css` | `Markdown.css`をVS Codeで使用する際に必要なスタイル          | `https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_VSCode.css` |
| `Markdown_Print.css`  | `Markdown.css`を印刷・PDF出力する際に必要なスタイル          | `https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_Print.css`  |
| `MARP.css`            | `Markdown.css`をMarp for VS Codeで使用する際に必要なスタイル | 未完成のためCDN配信対象外                                                   |

### H6を注釈として使用する

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
### ブラウザで使用する

HTMLの`head`でCSSを読み込み、Markdownから生成したHTMLを`.markdown-body`内へ配置します。

```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown.css">
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/Star-Delta/Markdown-CSS@0/Markdown_Print.css"
  media="print">

<div class="markdown-body">
  <!-- Markdownから生成したHTML -->
</div>
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

`Markdown_VSCode.css`では、VS Codeが生成する`.vscode-body`を対象に以下を調整します。

- VS Codeのライト・ダークテーマに対応した色
- Mermaidの横幅と初期化用要素
- Preview先頭要素の上余白
- CodeBlockコピーボタンを収める最小高さ

コピーボタンの最小高さは、Windows版VS Codeで確認した下限値を基に`50px`としています。他のOSや将来のVS CodeでButtonの寸法が変わった場合は、調整が必要になる可能性があります。

### 印刷

`Markdown_Print.css`は、`Markdown.css`を社内マニュアルのような少し堅めの印刷レイアウトへ調整する追加CSSです。Webブラウザの印刷機能による印刷またはPDF化を想定しています。

想定するMarkdownは、H1をタイトル、その直後の1つのParagraphを説明文、最初のH2以降を本文とする構成です。

- 1ページ目のH1をページ中央付近から開始
- H1直後のParagraphを中央揃え
- H2の前で改ページ
- Tableの横スクロールを解除し、セル内を自動改行
- CodeBlockのスクロールを解除し、長い行を要素内で自動改行

画面表示ではTableとCodeBlockの横スクロールが維持されます。

VS CodeにはMarkdown PreviewをCSSの印刷レイアウトとして出力する標準機能がないため、`Markdown_Print.css`はVS Codeでの利用を想定していません。

### Marp

`MARP.css`は、VS Code拡張機能のMarp for VS Codeで使用することを想定したCSSです。将来的には`Markdown.css`を基にMarp向けのスタイルへ調整する予定です。

現在はほとんど使用しておらず、抜本的な見直しも完了していません。基本となるCSS群のメンテナンス完了後に対応する予定のため、現時点では試験的なファイルとして扱い、CDN配信対象には含めません。

## その他
### カスタマイズ

`Markdown.css`の`.markdown-body`で、基本寸法をCSSカスタムプロパティとして定義しています。

| プロパティ         | 初期値             | 用途                                         |
| ------------------ | ------------------ | -------------------------------------------- |
| `--font-size`      | `16px`             | 本文と各要素の寸法基準                       |
| `--line-height`    | `1.5`              | 本文の行高                                   |
| `--margin-bottom`  | `var(--font-size)` | Block要素下部の基本余白                      |
| `--content-indent` | `1rem`             | 見出し以外の直下要素に使用する左右インデント |
| `--border`         | `none`             | `.markdown-body`のborder                     |
| `--padding`        | `none`             | `.markdown-body`のpadding                    |

色はライト・ダークテーマごとに、以下の系列で定義しています。

- `--color-font`
- `--color-canvas-*`
- `--color-border-*`
- `--color-headline-*`
- `--color-danger`
- `--color-syntax`

利用側のCSSで同じカスタムプロパティを上書きすることで配色や寸法を変更できます。

### 表示上の仕様

- H2とH3の番号はCSS Counterで生成されるため、Markdown本文そのものには追加されません。
- H6は通常の小見出しではなく、注釈としての利用を想定しています。
- 画面表示のTable Cellは自動改行せず、必要な場合にTable内を横スクロールします。
- CodeBlockは長い行を折り返さず横スクロールし、縦方向は全行を表示します。
- インラインコードは、長い連続文字列でも表示領域内で改行します。
- 印刷時はTableとCodeBlockを用紙幅内で改行します。
- CSS Nesting、`:has()`、`:nth-child(... of ...)`などを使用しているため、対応する比較的新しいブラウザ／Preview環境が必要です。

### License

[Mozilla Public License 2.0](LICENSE)
