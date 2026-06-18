---
title: Netscreen OR JunOS 模擬器 (轉貼)
date: 2008-10-29
categories: 網路技術
tags: [Netscreen, Juniper, 模擬器, JunOS]
---

<h2>Netscreen 與 JunOS 模擬器資源彙整 (轉貼)</h2>

<p style="color:#6b7280;font-size:14px;margin-top:-8px;">Juniper 網路設備模擬環境搭建指南</p>

<hr/>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#1e40af;font-weight:600;">資源介紹：Olive: JUNOS 8.5 + JWEB 8.5 虛擬機下載 (極限優化版)</p>
  <p style="margin:0;font-size:14px;color:#374151;">這是一個經過深度優化的 Olive 鏡像，壓縮包僅 98M，解壓後 159M，適合放在隨身碟中隨時進行實驗。</p>
</div>

<hr/>

<h2>1. 虛擬機特色與說明</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">項目</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;font-weight:600;">基底系統</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">從 FreeBSD 6.0 遷移至 FreeBSD 4.11 以縮減體積。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;font-weight:600;">JunOS 版本</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">jinstall-8.5R1.4-domestic-olive。</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;font-weight:600;">連線方式</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">支援命名管道 (Named Pipe) 控制，可透過 Telnet 進行操作。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;font-weight:600;">J-Web 支援</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">已內建 JWEB 網管介面，支援瀏覽器登入管理。</td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>2. 快速啟動與連線設定</h2>

<p>為了正確看到 Console 輸出，建議使用 <strong>Pipe Proxy</strong> 工具建立管道連線：</p>

<ol style="line-height:1.8; color:#374151;">
  <li>使用 <strong>Pipe Proxy</strong> 新建管道名稱：<code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">\\.\pipe\rocisky</code></li>
  <li>設定連接埠：<code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">2001</code></li>
  <li>使用終端機連線：<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">telnet 127.0.0.1 2001</code></li>
</ol>

<hr/>

<h2>3. JunOS 初始配置範例</h2>

<p>安裝好 JunOS 後，FreeBSD 的原始配置會被覆蓋，需透過以下指令重新設定網路與帳號：</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#567d46;"># 設定 Root 密碼</span>
<span style="color:#569cd6;">set</span> system root-authentication plain-text-password

<span style="color:#567d46;"># 建立管理帳號</span>
<span style="color:#569cd6;">set</span> system login user rocisky uid <span style="color:#b5cea8;">2004</span> class super-user authentication plain-text-password

<span style="color:#567d46;"># 設定主機名稱與網域</span>
<span style="color:#569cd6;">set</span> system host-name olive
<span style="color:#569cd6;">set</span> system domain-name juniper.net

<span style="color:#567d46;"># 設定管理網口 IP (em0)</span>
<span style="color:#569cd6;">set</span> interface em0 unit <span style="color:#b5cea8;">0</span> family inet address <span style="color:#b5cea8;">192.168.1.11/24</span>

<span style="color:#567d46;"># 設定預設閘道</span>
<span style="color:#569cd6;">set</span> system backup-router <span style="color:#b5cea8;">192.168.1.254</span>

<span style="color:#567d46;"># 啟用 J-Web 服務</span>
<span style="color:#569cd6;">set</span> system services web-management http
</pre>

<hr/>

<h2>4. 已知問題與解決方案</h2>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#c2410c;font-weight:600;">⚠ 關於組播 (Multicast) 未固化問題</p>
  <p style="margin:0;font-size:14px;color:#374151;">
    目前版本的 Olive 存在組播配置無法固化的現象。若需要進行 OSPF 或組播相關實驗，每次啟動後可能需要進入單用戶模式執行特定的打補丁指令。
  </p>
</div>

<hr/>

<h3>檔案下載與參考資源</h3>

<ul>
  <li><strong>Rapidshare 下載網址：</strong> <a href="http://rapidshare.com/files/109424406/OliveCOM2002.rar" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://rapidshare.com/files/109424406/OliveCOM2002.rar</a></li>
  <li><strong>原始轉貼出處：</strong> <a href="http://www.junipers.cn/Simulation/755.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">Juniper 中國模擬資源站</a></li>
</ul>
