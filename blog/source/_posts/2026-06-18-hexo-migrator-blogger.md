---
title: hexo-migrator-blogger
date: 2026-06-18
---

## Install hexo
根據https://hexo.io/zh-tw/docs/setup

```
$ cd ~/git/gentoobreaking/
$ npx hexo init blog
$ cd blog
$ npm install
$ npx hexo clean && npx hexo server
```

## Enable HTML support

$ vim ~/git/gentoobreaking/blog/_config.yml
```
# 允許 Markdown 內使用 HTML 標籤
marked:
  gfm: true
  breaks: true
  pedantic: false
  mangle: false
  headerIds: true
highlight:
  enable: false
  line_number: false
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
  htmlify: false
# 允許原始 HTML
unsafe: true
pjax: true

# Search (for local_search plugin)
search:
  path: search.xml
  field: post
  content: true
  format: html
```

## Convert blog post into hexo 

# 如果數量不對，先砍掉重建
```
rm -f ~/git/gentoobreaking/blog/source/_posts/*

cd ~/git/gentoobreaking/blog

node -e "
const fs = require('fs');
const { JSDOM } = require('jsdom');

const feed = fs.readFileSync('/Users/david/Downloads/Takeout/Blogger/Blogs/IT@DOG/feed.atom', 'utf8');
const dom = new JSDOM(feed, { contentType: 'application/xml' });
const doc = dom.window.document;
const entries = doc.querySelectorAll('entry');

entries.forEach((entry, i) => {
  const title = entry.querySelector('title')?.textContent || '';
  const published = entry.querySelector('published')?.textContent || '';
  const content = entry.querySelector('content')?.textContent || '';
  const pubDate = published ? published.slice(0,10) : '1970-01-01';
  const slug = title.toLowerCase().replace(/[^\w\u4e00-\u9fa5]+/g, '-').replace(/(^-|-$)/g, '');
  const filename = pubDate + '-' + slug + '.md';
  const md = '---\ntitle: ' + title + '\ndate: ' + pubDate + '\n---\n\n' + content;
  fs.writeFileSync('source/_posts/' + filename, md);
});

console.log('Total: ' + entries.length + ' posts');
"
```

## Theme Configuration

Hexo-Theme-NexT (最經典：極簡、大氣)
   * 特點：Hexo 的標誌性主題。走極簡黑白風，功能極其強大且外掛生態最豐富。
   * 優勢：因為使用者最多，任何問題（如 RSS、SEO）隨便搜尋都有現成解答。
   * 網址：https://github.com/next-theme/hexo-theme-next

```
npx hexo new page archives
npx hexo new page categories
npx hexo new page tags
npx hexo new page about
tree -L 2 source

cd ~/git/gentoobreaking/blog
npm install hexo-theme-next
npm install hexo-generator-feed hexo-generator-search --save
npx hexo config theme next
npx hexo clean && npx hexo server

vim ~/git/gentoobreaking/blog/_config.yml 
language: zh-TW
```

## Back to default theme

```
npx hexo config theme landscape
npm uninstall hexo-theme-next
```

