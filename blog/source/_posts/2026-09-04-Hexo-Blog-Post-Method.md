---
title: Hexo Blog Post Method
date: 2026-09-04 21:49:59
tags: [Hexo, Blogging, Workflow, Tutorial]
---
<div class="post-content">

<h1>Hexo Blog Post Method</h1>

<h2>建立新文章</h2>

```bash
cd ~/git/gentoobreaking/blog
npx hexo n "文章標題"
```

輸出類似：

```
INFO  Validating config
INFO  Created: ~/git/gentoobreaking/blog/source/_posts/文章標題.md
```

<h2>編輯文章</h2>

```bash
vim ~/git/gentoobreaking/blog/source/_posts/文章標題.md
```

輸入 Markdown 內容後，保存並退出。

<h2>本地預覽</h2>

```bash
npx hexo clean ; npx hexo s
```

開啟 <code>http://localhost:4000</code> 在瀏覽器中預覽效果。

<h2>生成與發佈</h2>

確認無誤後，輸入 <code>hexo g</code> 產生靜態檔案，再透過部署指令發佈。

```bash
npx hexo g
```

輸出類似：

```
INFO  Validating config
....
INFO  Generated:....
```

<h2>快速提示</h2>

<p>當然也可以直接叫 AI agent 幫你排版及分類及發送。只是記錄一下手工流程～</p>

</div>
