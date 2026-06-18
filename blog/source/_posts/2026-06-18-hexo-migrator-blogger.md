
https://hexo.io/zh-tw/docs/setup

$ cd ~/git/gentoobreaking/
$ npx hexo init blog
$ cd blog
$ npm install
$ npx hexo clean && npx hexo server

$ vim ~/git/gentoobreaking/blog/_config.yml
```
# 允許 Markdown 內使用 HTML 標籤
marked:
  gfm: true
  breaks: true
  pedantic: false
  mangle: false
  headerIds: true

# 允許原始 HTML
unsafe: true
```


# 如果數量不對，先砍掉重建
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


