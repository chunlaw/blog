---
title: 【Canvas 小撇步】從 Issuu 擷取高清圖像
date: 2022-12-06 23:41:47 +0800
categories: [Technology, Canvas]
tags: [html, canvas]     # TAG names should always be lowercase
image: 
  path: /posts/canvas-1/canvas-1.jpg
  width: 600
  alt: 擷取下來的雜誌單元故事《燕處危巢》
---

薄有名氣的 [Issuu](https://issuu.com/) 一直是不同雜誌出版社發佈其中一個第三方平台，此角色亦令其保留了部份再無下文的雜誌社出品。近日朋友就傳來求助希望能為她過往的雜誌出品再作備份，幸而[她的作品](https://issuu.com/arianalife/docs/ariana04/108)剛好在 Issuu 上可以免費取閱，省卻許多麻煩。

## 問題初探

隨便 Google 一下自然有不少簡單直接的網站幫忙下載整份雜誌的 PDF，朋友想必也試過不少，奈何成品質素只是可讀水平，文字部份在轉為圖像時被粗暴壓縮，對比變得不明確，糊作一團。

![pdf放到150%會咁，所以100%或再細時啲字都會好朦同黐底， 似一pat嘢](/posts/canvas-1/canvas-2.jpeg){: width="800"}
_pdf放到150%會咁，所以100%或再細時啲字都會好朦同黐底， 似一pat嘢_

我也試過幾個現成的網站，也找了些 Open Source 的 library，基本上成品都跟網站下載一樣。此路不通，只好另僻蹊徑。

## 仔細分析

回到 Issuu 網站，打開 Inspector 一看，原來是 `<canvas />`，那就可以簡單解決了。

![在 Issuu 上對頁面以右鍵打開 inspector ，確定 canvas 位置](/posts/canvas-1/canvas-3.jpg){: width="600"}
_在 Issuu 上對頁面以右鍵打開 inspector ，確定 canvas 位置_

```js
// 借用 Issuu reader 上的 Fullscreen 按鈕轉到全營幕，以便擷取力所能及的高清擷圖
// 擷取 canvas 的 image data 
var imgData = document.getElementsByTagName('canvas')[0].toDataURL() 

// 創建一個 HTML 中 <image /> 的 object，並把 image data 寫到裡頭
var image = new Image();
image.src = imgData

// 把 <image /> 放到新視窗中
var w = window.open("");
w.document.write(image.outerHTML)
```

以此方法，逐頁擷圖取得一張張 PNG，再用網上 PNG 合併 PDF 工具即可。乍看之下，難度不高，但操作上仍需逐頁處理，若有需要，日後再探討如何備份全本雜誌。

![高清擷圖下，輕鬆解決文字模糊問題](/posts/canvas-1/canvas-4.jpg){: width="800"}
_高清擷圖下，輕鬆解決文字模糊問題_

## 進階技術探討 - 解決透明圖問題

及後仔細研究下，不難發現 Issuu 使用的 `<canvas />` 是用 [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/) 而不是 [2d](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D)。由於 WebGL 在很多情況下會為了載入速度而不會保留 Drawing Buffer，因此上邊提到的 `.toDataURL()` 便會失效，剩下一個透明頁面。

一般解決的方法是在繪畫 `<canvas />` 時使用 `{preserveDrawingBuffer: true}`，然而這次情況我們沒辦法改變 Issuu 的 javascript。只好在翻到那頁面時，趁 Buffer 還在時就在 inspector 上跑上邊的 code，盡快完成。

實際上，應有更好的解法，但是次擷圖目標只有 6 頁，不糾纏。成果亦已回覆朋友，狀甚滿意。