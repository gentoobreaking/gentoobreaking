---
title: Hexo Migrator for Blogger 遷移指南
date: 2026-06-18
categories: 開發工具與自動化
tags: [Hexo, Blogger]
---

<h2>Hexo Migrator for Blogger 遷移指南</h2>

<p style="color:#6b7280;font-size:14px;margin-top:-8px;">從 Blogger 搬家到 Hexo 的完整實戰紀錄</p>

<hr/>

<h2>1. 安裝與初始化 Hexo</h2>

<p>參考 <a href="https://hexo.io/zh-tw/docs/setup" target="_blank" style="color:#1d4ed8;text-decoration:underline;">Hexo 官方文件</a> 進行基本環境架設。</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> ~/git/gentoobreaking/
<span style="color:#569cd6;">npx</span> hexo init blog
<span style="color:#569cd6;">cd</span> blog
<span style="color:#569cd6;">npm</span> install
<span style="color:#569cd6;">npx</span> hexo clean && <span style="color:#569cd6;">npx</span> hexo server
</pre>

<hr/>

<h2>2. 開啟 HTML 與搜尋支援</h2>

<p>修改 <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">_config.yml</code> 以支援 Markdown 內的 HTML 標籤及本地搜尋。</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#567d46;"># 允許 Markdown 內使用 HTML 標籤</span>
<span style="color:#9cdcfe;">marked:</span>
  <span style="color:#9cdcfe;">gfm:</span> <span style="color:#b5cea8;">true</span>
  <span style="color:#9cdcfe;">breaks:</span> <span style="color:#b5cea8;">true</span>
  <span style="color:#9cdcfe;">unsafe:</span> <span style="color:#b5cea8;">true</span>

<span style="color:#567d46;"># 關閉預設高亮以相容主題</span>
<span style="color:#9cdcfe;">highlight:</span>
  <span style="color:#9cdcfe;">enable:</span> <span style="color:#b5cea8;">false</span>

<span style="color:#567d46;"># 搜尋外掛配置</span>
<span style="color:#9cdcfe;">search:</span>
  <span style="color:#9cdcfe;">path:</span> <span style="color:#ce9178;">search.xml</span>
  <span style="color:#9cdcfe;">field:</span> <span style="color:#ce9178;">post</span>
  <span style="color:#9cdcfe;">content:</span> <span style="color:#b5cea8;">true</span>
  <span style="color:#9cdcfe;">format:</span> <span style="color:#ce9178;">html</span>
</pre>

<hr/>

<h2>3. Blogger 文章遷移 (Node.js 腳本)</h2>

<p>使用 JSDOM 解析 Blogger 導出的 <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">feed.atom</code> 並轉換為 Hexo 格式。</p>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#1e40af;font-weight:600;">遷移核心邏輯</p>
</div>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#567d46;"># 執行遷移腳本 (需先安裝 jsdom: npm install jsdom)</span>
<span style="color:#569cd6;">node</span> -e <span style="color:#ce9178;">"
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
  
  // 生成 Slug 並處理中文字元與特殊符號
  const slug = title.toLowerCase()
    .replace(/[^\w\u4e00-\u9fa5]+/g, '-')
    .replace(/(^-|-$)/g, '');
  
  const filename = pubDate + '-' + slug + '.md';
  const md = '---\\ntitle: ' + title + '\\ndate: ' + pubDate + '\\n---\\n\\n' + content;
  
  fs.writeFileSync('source/_posts/' + filename, md);
});

console.log('Total: ' + entries.length + ' posts');
"</span>
</pre>

<hr/>

<h2>4. 主題配置 (NexT)</h2>

<p>推薦使用穩定且功能強大的 <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">NexT</code> 主題。</p>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">步驟</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">指令 / 設定</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;font-weight:600;">安裝主題</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">npm install hexo-theme-next</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;font-weight:600;">功能組件</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">npm install hexo-generator-feed hexo-generator-search --save</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;font-weight:600;">語系設定</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">將 <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">language</code> 設為 <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">zh-TW</code> 避免語系回退錯誤。</td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>5. 常用指令總結</h2>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#166534;font-weight:600;">維護建議流程：</p>
  <ol style="margin:0;padding-left:20px;color:#374151;line-height:1.8;">
    <li><code style="background:#111827;color:#fbbf24;padding:2px 6px;border-radius:4px;font-family:monospace;">npx hexo clean</code> — 清除快取</li>
    <li><code style="background:#111827;color:#fbbf24;padding:2px 6px;border-radius:4px;font-family:monospace;">npx hexo g</code> — 生成靜態檔案</li>
    <li><code style="background:#111827;color:#fbbf24;padding:2px 6px;border-radius:4px;font-family:monospace;">npx hexo s</code> — 啟動本地預覽</li>
  </ol>
</div>

<hr/>

<h2>6. 還原預設主題</h2>

<p>如果需要換回 Hexo 預設的 <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">landscape</code> 主題，可執行以下指令：</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">npx</span> hexo config theme landscape
<span style="color:#569cd6;">npm</span> uninstall hexo-theme-next
</pre>