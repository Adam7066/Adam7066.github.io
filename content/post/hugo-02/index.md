---
slug: "hugo-02"
title: "Hugo-02：優化 Blog"
date: 2021-02-07T04:06:17+08:00
tags:
    - Hugo
---
# 簡介
- 這部份是紀錄我修改的主題內容，並將 codeblocks 區塊美化，以及支援 [KaTex](https://katex.org/) 和 Google Analytics

# 教學開始
- 我使用的主題為 [Stack](https://themes.gohugo.io/hugo-theme-stack/)

## 主題內容修改
- 找到 `assets/scss/variables.scss`，並修改
    ```scss
    --link-background-color: 90, 240, 250; //189, 195, 199;
    --code-text-color: #ef3982; //rgba(255, 255, 255, 0.9);
    ```

## Codeblocks 區塊美化
1. 到 `static/css/` 下建立 `copy-to-clipboard.css`，內容如下
    ```css
    .highlight {
        position: relative;
    }
    .highlight .ln {
        -moz-user-select: none;
        -webkit-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }
    .highlight-copy-btn {
        position: absolute;
        top: 7px;
        right: 7px;
        border: 0;
        border-radius: 4px;
        padding: 1px;
        font-size: 0.8em;
        line-height: 1.8;
        color: #fff;
        background-color: #777;
        min-width: 55px;
        text-align: center;
    }
    .highlight-copy-btn:hover {
        background-color: #666;
    }
    ```
2. 到 `static/js/` 下建立 `copy-to-clipboard.js`，內容如下
    ```javascript
    (function () {
        'use strict';
        if (!document.queryCommandSupported('copy')) {
            return;
        }
        function flashCopyMessage(el, msg) {
            el.textContent = msg;
            setTimeout(function () {
                el.textContent = "Copy";
            }, 1000);
        }
        function selectText(node) {
            var selection = window.getSelection();
            var range = document.createRange();
            range.selectNodeContents(node);
            selection.removeAllRanges();
            selection.addRange(range);
            return selection;
        }
        function addCopyButton(containerEl) {
            var copyBtn = document.createElement("button");
            copyBtn.className = "highlight-copy-btn";
            copyBtn.textContent = "Copy";

            var codeEl = containerEl.firstElementChild;
            copyBtn.addEventListener('click', function () {
                try {
                    var selection = selectText(codeEl);
                    document.execCommand('copy');
                    selection.removeAllRanges();

                    flashCopyMessage(copyBtn, 'Copied!')
                } catch (e) {
                    console && console.log(e);
                    flashCopyMessage(copyBtn, 'Failed :\'(')
                }
            });
            containerEl.appendChild(copyBtn);
        }
        // Add copy button to code blocks
        var highlightBlocks = document.getElementsByClassName('highlight');
        Array.prototype.forEach.call(highlightBlocks, addCopyButton);
    })();
    ```
3. 修改 `config.yaml`
    ```yaml
    custom_css: ["css/copy-to-clipboard.css"]

    pygmentsUseClasses: true
    markup:
        highlight:
            lineNos: true
            lineNumbersInTable: false
    ```
4. 修改 `layouts/partials/head/custom.html`
    ```html
    {{ range .Site.Params.custom_css -}}
        <link rel="stylesheet" href="{{ . | absURL }}">
    {{- end }}
    {{ range .Site.Params.custom_js -}}
        <script type="text/javascript" src="{{ . | absURL }}"></script>
    {{- end }}
    ```
5. 修改 `layouts/partials/footer/custom.html`
    ```html
    <script src="/js/copy-to-clipboard.js"></script>
    ```
6. 這樣程式碼區塊就有 Copy Button，並且直接複製時不會選取到行號了

## KaTex
1. 到 `layouts/partials/` 下新增 `math.html`，內容如下
    ```html
    {{ if or .Params.math .Site.Params.math }}
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.12.0/dist/katex.min.css" integrity="sha384-AfEj0r4/OFrOo5t7NnNe46zW/tFgW6x/bCJG8FqQCEo3+Aro6EYUG4+cU+KJWu/X" crossorigin="anonymous">
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.12.0/dist/katex.min.js" integrity="sha384-g7c+Jr9ZivxKLnZTDUhnkOnsh30B4H0rpLUpJ4jAIKs4fnJI+sEnkvrMWph2EDg4" crossorigin="anonymous"></script>
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.12.0/dist/contrib/auto-render.min.js" integrity="sha384-mll67QQFJfxn0IYznZYonOWZ644AWYC+Pt2cHqMaRhXVrursRwvLnLaebdGIlYNa" crossorigin="anonymous" onload="renderMathInElement(document.body);"></script>
    <script>
        document.addEventListener("DOMContentLoaded", function() {
            renderMathInElement(document.body, {
                delimiters: [
                    {left: "$$", right: "$$", display: true},
                    {left: "\\[", right: "\\]", display: true},
                    {left: "$", right: "$", display: false},
                    {left: "\\(", right: "\\)", display: false}
                ]
            });
        });
    </script>
    {{ end }}
    ```
2. 修改 `layouts/partials/head/custom.html`
    ```html
    {{ partial "math.html" . }}
    ```

## Google Analytics
1. 到 Google Analytics 創建一個資料串流
2. 由於 Hugo 內建的 GA 模板，似乎還不支援 GA4，因此我們使用 `gtag.js`
3. 複製剛剛建立的資料串流中的 `gtag.js` 內容到 `layouts/partials/google_analytics.html`中
4. 修改 `layouts/partials/head/custom.html`
    ```html
    {{ if not .Site.IsServer }}{{ partial "google_analytics.html" . }}{{ end }}
    ```
    - 這裡的 `if` 是避免在 local 測試時的數據也被紀錄下來
> Hugo 於 0.82.0 版本已更新了[對 GA4 的支援](https://gohugo.io/templates/internal/#google-analytics)，使用方法如下
1. 同上
2. 在 `config.yaml` 中設定 `googleAnalytics: G-`
3. 修改 `layouts/partials/head/custom.html`
    ```html
    {{ template "_internal/google_analytics.html" . }}
    ```