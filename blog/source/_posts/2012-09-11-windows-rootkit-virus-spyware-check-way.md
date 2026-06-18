---
title: Windows Rootkit & Virus & Spyware Check Way
date: 2012-09-11
categories: 資安技術
tags: [Rootkit, Windows]
---

<h2>Windows Rootkit &amp; Virus &amp; Spyware 檢測工具彙整</h2>

<p style="color:#6b7280;font-size:14px;margin-top:-8px;">by David · 2008/11/12</p>

<hr/>

<h2>Rootkit 檢測工具</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">工具名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">GMER</code> ⭐ 推薦</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">全方位的 Rootkit 檢測與移除工具<br><a href="http://www.gmer.net/rootkit.php" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://www.gmer.net/rootkit.php</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">System Virginity Verifier</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">檢查 Windows 系統核心元件是否被惡意軟體篡改，確保系統完整性並發現潛在入侵。<br><a href="http://invisiblethings.org/code.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://invisiblethings.org/code.html</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">modGREPER</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Windows 2000/XP/2003 隱藏模組偵測器，搜尋核心記憶體中的模組描述物件。<br>使用方式：<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">modgreper.exe –h</code><br><a href="http://invisiblethings.org/code.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://invisiblethings.org/code.html</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">FLISTER</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">利用 <code>ZwQueryDirectoryFile()</code> 的 <code>ReturnSingleEntry=TRUE</code> 漏洞來偵測被 User-mode 與 Kernel-mode Rootkit 隱藏的檔案。支援 Windows 2000, XP, 2003。<br><a href="http://invisiblethings.org/code.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://invisiblethings.org/code.html</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Panda Anti-Rootkit</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Panda Security 免費工具，網友回報踴躍，協助改善並提交新發現的 Rootkit 樣本。<br><a href="http://www.majorgeeks.com/Panda_Anti-Rootkit_d5457.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">MajorGeeks 下載</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Trend Micro RootkitBuster</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">趨勢科技出品，徹底清除電腦中的 Rootkit。<br><a href="http://www.trendmicro.com/download/rbuster.asp" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://www.trendmicro.com/download/rbuster.asp</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Sophos Anti-Rootkit</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">使用先進的 Rootkit 偵測技術找出並移除隱藏的 Rootkit。即使在安裝防毒軟體前已有 Rootkit 存在，也可能不被發現。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Rootkit Hook Analyzer</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><a href="http://www.resplendence.com/hookanalyzer" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://www.resplendence.com/hookanalyzer</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">RootkitRevealer</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Microsoft Sysinternals 工具，掃描系統中的 Rootkit 型惡意軟體。<br><a href="http://technet.microsoft.com/zh-tw/sysinternals/bb897445.aspx" target="_blank" style="color:#1d4ed8;text-decoration:underline;">Technet 下載</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Process Hunter</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><a href="http://programmerstools.org/node/190" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://programmerstools.org/node/190</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">HiJackThis</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">經典的系統分析工具，可掃描並手動移除 Hijackers、Spyware、Adware、Trojans、Worms。</td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>Virus &amp; Spyware 檢測工具</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">工具名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">iClean</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">趨勢科技出品，有效清除台灣地區常見變種病毒及惡意程式，防止持續變種的惡意程式再次寫入系統。可收集系統資訊及病毒記錄檔回傳分析。<br>
        <span style="background:#fff7ed;color:#c2410c;padding:2px 6px;border-radius:4px;font-size:12px;">⚠ 解壓縮密碼：novirus</span><br>
        <a href="http://www.trendmicro.com.tw/iclean/index.htm" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://www.trendmicro.com.tw/iclean/index.htm</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">可疑程式分析工具 (SIC Tool)</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">當電腦有異常但未偵測到病毒時使用，依操作文件收集系統資訊提供給趨勢科技分析。<br>
        <span style="background:#fff7ed;color:#c2410c;padding:2px 6px;border-radius:4px;font-size:12px;">⚠ 解壓縮密碼：novirus</span><br>
        <a href="http://www.trendmicro.com/download/zh-tw/sic.asp" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://www.trendmicro.com/download/zh-tw/sic.asp</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Off-Line Trend Scan Virus</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">非趨勢科技用戶適用的離線掃描工具。下載 <strong>SYSCLEAN.COM</strong> 搭配病毒碼進行離線掃描。<br>
        <a href="http://www.trendmicro.com/download/zh-tw/tsc.asp" target="_blank" style="color:#1d4ed8;text-decoration:underline;">離線掃描下載</a><br>
        <a href="http://www.trendmicro.com/download/zh-tw/pattern.asp" target="_blank" style="color:#1d4ed8;text-decoration:underline;">病毒碼下載</a></td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>系統檢測工具</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">工具名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">a-squared HiJackFree</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">詳細的系統分析工具，幫助進階用戶偵測並移除所有類型的 HiJackers、Spyware、Adware、Trojans、Worms。<br><a href="http://www.hijackfree.com/en/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://www.hijackfree.com/en/</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Protector Plus Vulnerability Scanner</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Windows 漏洞掃描器，檢測系統安全性弱點。<br><a href="http://www.pspl.com/download/winvulscan.htm" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://www.pspl.com/download/winvulscan.htm</a></td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>Sysinternals 工具組 ⭐ 必備</h2>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#1e40af;"><strong>微軟 Sysinternals 工具組</strong> — 系統管理者必備瑞士刀</p>
  <a href="http://technet.microsoft.com/en-us/sysinternals/default.aspx" target="_blank" style="color:#1d4ed8;text-decoration:underline;">http://technet.microsoft.com/en-us/sysinternals/default.aspx</a>
</div>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">工具名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">功能說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Autoruns</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">檢視系統開機及登入時自動執行的程式，完整列出所有 Registry 與檔案位置的開機設定。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Process Explorer</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">強大的程序檢視工具，可查看程序開啟的檔案、Registry 鍵、已載入的 DLL，並顯示每個程序的擁有者。</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">AccessEnum</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">簡單但強大的安全性工具，顯示目錄、檔案、Registry 鍵的存取權限，幫助發現權限漏洞。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">TCPView</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">即時的網路連線檢視工具，顯示所有 TCP/UDP 連線的詳細資訊。</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">RootkitRevealer</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">掃描系統中的 Rootkit 型惡意軟體。</td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>必備工具精選 ⭐</h2>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#166534;font-weight:600;">下列這幾種，是強烈建議一定要會使用的工具：</p>
  <ol style="margin:0;padding-left:20px;color:#15803d;line-height:1.8;">
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">Autoruns</code> — 開機自動執行程式檢視</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">Process Explorer</code> — 程序詳細檢視</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">AccessEnum</code> — 權限漏洞檢測</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">TCPView</code> — 網路連線監控</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">RootkitRevealer</code> — Rootkit 掃描</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">GMER</code> — 全方位 Rootkit 檢測</li>
  </ol>
</div>