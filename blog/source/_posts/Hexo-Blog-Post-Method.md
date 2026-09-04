---
title: Hexo Blog Post Method
date: 2026-09-04 21:49:59
tags:
---

# Hexo Blog Post Method

```
cd ~/git/gentoobreaking/blog
npx hexo n "文章標題"
```
INFO  Validating config
INFO  Created: ~/git/gentoobreaking/blog/source/_posts/文章標題.md

```
vim ~/git/gentoobreaking/blog/source/_posts/文章標題.md
```

輸入 hexo s 在本地預覽效果。
```
npx hexo s
```

確認無誤後，輸入 hexo g 產生靜態檔案，再透過部署指令發佈。
```
npx hexo g
```
INFO  Validating config
....
INFO  Generated:....

當然也可以直接叫ai agent幫你排版及分類及發送。只是記錄一下手工流程～
