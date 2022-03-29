---
title: "hugoで$\\LaTeX$表示できるようにした備忘録"
date: 2022-03-29T00:47:11+09:00
draft: false
categories: "hugo"
tags: ["KaTeX", "hugo setting", "memo"]
ShowToc: true
---

hugoで$\LaTeX$での数式が表示されるように設定した時のメモ

テーマはPaperModでの話です。KaTeXを使用。

## 手順
### 1. /layouts/partials/math.html の作成
/layouts/partials/math.html を作る。math.htmlの中には[Auto-render Extension](https://katex.org/docs/autorender.html)のページにある以下のコードを貼り付ける。

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.15.3/dist/katex.min.css" integrity="sha384-KiWOvVjnN8qwAZbuQyWDIbfCLFhLXNETzBQjA/92pIowpC0d2O3nppDGQVgwd2nB" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.15.3/dist/katex.min.js" integrity="sha384-0fdwu/T/EQMsQlrHCCHoH10pkPLlKA1jL5dFyUOvB3lfeT2540/2g6YgSi2BL14p" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.15.3/dist/contrib/auto-render.min.js" integrity="sha384-+XBljXPPiv+OzfbB3cVmLHf4hdUFHlWNZN5spNQ7rmHTXpd7WvJum6fIACpNNfIR" crossorigin="anonymous"></script>
<script>
    document.addEventListener("DOMContentLoaded", function() {
        renderMathInElement(document.body, {
          // customised options
          // • auto-render specific keys, e.g.:
          delimiters: [
              {left: '$$', right: '$$', display: true},
              {left: '$', right: '$', display: false},
              {left: '\\(', right: '\\)', display: false},
              {left: '\\[', right: '\\]', display: true}
          ],
          // • rendering keys, e.g.:
          throwOnError : false
        });
    });
</script>
```

### 2. /layouts/partials/extend_head.htmlに追記
/layouts/partials/extend_head.htmlに以下を追記（extend_head.htmlファイルがない場合は作成）

```bash
{{ if or .Params.math .Site.Params.math }}
{{ partial "math.html" . }}
{{ end }}
```

### 3. math: trueにする
config.yml内のparams部分に`math: true`を追加。

個別の記事ごとに設定したい場合は各mdファイル一番上の`title: `などがある部分に`math: true`を追加。

## 例
```latex
Block: $$e^x=\sum_{k = 0}^{\infty}  \frac{x^k}{k!}$$

Inline: $e^x=\sum_{k = 0}^{\infty}  \frac{x^k}{k!}$
```
Block: $$e^x=\sum_{k = 0}^{\infty}  \frac{x^k}{k!}$$

Inline: $e^x=\sum_{k = 0}^{\infty}  \frac{x^k}{k!}$

## 参考ページ
[Math Typesetting](https://adityatelange.github.io/hugo-PaperMod/posts/math-typesetting/)
