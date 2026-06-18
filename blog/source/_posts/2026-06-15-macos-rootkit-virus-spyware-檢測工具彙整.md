---
title: macOS Rootkit & Virus & Spyware 檢測工具彙整
date: 2026-06-15
---

<h2>macOS Rootkit &amp; Virus &amp; Spyware 檢測工具彙整</h2>

<p style="color:#6b7280;font-size:14px;margin-top:-8px;">macOS 安全檢測工具指南</p>

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
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Knockknock</code> ⭐ 推薦</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Objective-See 出品的開機項目掃描工具，可識別隱藏的開機執行項目、核心擴充套件、Login Items、Launch Agents/Daemons。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">brew install knockknock</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">TaskExplorer</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Objective-See 出品，可視化檢視所有執行中的程序、下載的檔案、網路連線、使用的 DLL（dylib）。<br>
        <a href="https://objective-see.org/products/taskexplorer.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://objective-see.org/products/taskexplorer.html</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">RansomWhere?</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Objective-See 出品，即時監控檔案系統，偵測勒索軟體加密行為並自動封鎖可疑程序。<br>
        <a href="https://objective-see.org/products/ransomwhere.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://objective-see.org/products/ransomwhere.html</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">BlockBlock</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Objective-See 出品，即時監控系統的持續性安裝行為（Launch Agents、Login Items、Startup Items、Kernel Extensions）。<br>
        <a href="https://objective-see.org/products/blockblock.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://objective-see.org/products/blockblock.html</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">OiusForm</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Objective-See 出品，監控應用程式是否未經授權即嘗試截圖或錄製螢幕（防間諜軟體）。<br>
        <a href="https://objective-see.org/products/oversight.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://objective-see.org/products/oversight.html</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">LuLu</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Objective-See 出品的免費開源防火牆，偵測未經授權的網路連線（類似 Little Snitch）。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">brew install lulu</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">KextViewr</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">列出系統中所有已載入的 Kernel Extensions，可發現隱藏或可疑的 KEXT。<br>
        <a href="https://objective-see.org/products/kextviewr.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://objective-see.org/products/kextviewr.html</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">PE-sieve (macOS)</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">用於偵測在運行程序中注入惡意程式碼的工具（Process Elevation injection）。<br>
        <a href="https://github.com/hasherezade/pe-sieve" target="_blank" style="color:#1d4ed8;text-decoration:underline;">GitHub - PE-sieve</a></td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>病毒與惡意軟體檢測工具</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">工具名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Malwarebytes for Mac</code> ⭐ 推薦</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">知名惡意軟體移除工具的 macOS 版本，專精於廣告軟體、間諜軟體、PUP 偵測與移除。<br>
        <a href="https://www.malwarebytes.com/mac/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://www.malwarebytes.com/mac/</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Avast Security for Mac</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">免費防毒軟體，即時防護、郵件掃描、Wifi 安全檢測。<br>
        <a href="https://www.avast.com/antivirus-mac" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://www.avast.com/antivirus-mac</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">ClamXav</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">基於開源 ClamAV 的 macOS 防毒工具，支援自訂掃描範圍與即時監控。<br>
        <a href="https://www.clamxav.com/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://www.clamxav.com/</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Bitdefender Virus Scanner</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Bitdefender 出品的免費 macOS 掃描工具，內建龐大病毒資料庫。<br>
        <a href="https://www.bitdefender.com/mac/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://www.bitdefender.com/mac/</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Kaspersky Virus Scanner</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">卡巴斯基 macOS 病毒掃描器，支援自訂區域與深度掃描。<br>
        <a href="https://www.kaspersky.com/mac-security" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://www.kaspersky.com/mac-security</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Combo-Jockey / Combofix (macOS)</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">類似 Windows 版 Combofix，用於移除複雜的 Mac 惡意軟體與廣告軟體。<br>
        <span style="background:#fff7ed;color:#c2410c;padding:2px 6px;border-radius:4px;font-size:12px;">⚠ 使用前建議先時光機備份</span></td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>系統分析與程序監控工具</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">工具名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Activity Monitor</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">macOS 內建程序監控工具（<code>/Applications/Utilities/Activity Monitor.app</code>），可查看 CPU、記憶體、網路、磁碟使用情形。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">top / htop</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">終端機程序監控工具，htop 可用 <code>brew install htop</code> 安裝增強版。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">htop</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">netstat / lsof</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">終端機內建網路工具，查看網路連線與佔用埠號。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">lsof -i</code>、<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">netstat -an</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">lsop</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">macOS 指令列工具，查看程序開啟的檔案，類似 Windows Process Explorer。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">lsof -p [PID]</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">MenuMeters</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">將 CPU、記憶體、網路、磁碟監控資訊顯示在 menu bar 上，即時掌握系統資源。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">brew install menumeters</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">iStat Menus</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">付費的 menu bar 系統監控工具，可顯示 CPU、記憶體、網路、磁碟、溫度、風扇、電池等詳細資訊。<br>
        <a href="https://bjango.com/mac/istatmenus/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://bjango.com/mac/istatmenus/</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Console.app</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">macOS 內建日誌檢視工具（<code>/Applications/Utilities/Console.app</code>），可查看系統與應用程式日誌，偵測異常行為。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">log show</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">終端機查詢系統日誌的指令。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">log show --predicate 'process == "kernel"' --last 1h</code></td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>開機項目與持續性監控</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">工具名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">launchctl</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">macOS 內建指令，管理 Launch Agents、Launch Daemons。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">launchctl list</code> — 列出所有載入的程序<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">ls ~/Library/LaunchAgents</code> — 使用者登入啟動項目</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">System Preferences</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">「使用者與群組」→「登入項目」可檢視帳號的開機啟動程式。</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">crontab</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">排程任務可能作為持續性機制，檢查方式：<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">crontab -l</code>、<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">sudo crontab -l</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">plist / launchd.plist</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">macOS 開機項目的核心機制。檢查位置：<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">/Library/LaunchAgents/</code>、<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">/Library/LaunchDaemons/</code><br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">~/Library/LaunchAgents/</code>、<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">/System/Library/LaunchAgents/</code></td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>網路安全工具</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">工具名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Little Snitch</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">付費個人防火牆，即時監控並攔截應用程式的網路連線，類似 Windows 的 Process Firewall。<br>
        <a href="https://www.obdev.at/products/littlesnitch/index.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://www.obdev.at/products/littlesnitch/</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Radio Silence</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">輕量級網路監控工具，可一鍵封鎖應用程式的網路連線。<br>
        <a href="https://radiosilenceapp.com/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://radiosilenceapp.com/</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Hands Off</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">另一款知名個人防火牆，可控制應用程式的網路與檔案存取。<br>
        <a href="https://www.oneperiodic.com/products/handsoff/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">https://www.oneperiodic.com/products/handsoff/</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">WireShark</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">網路封包分析器，可深入分析網路流量，偵測異常連線。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">brew install wireshark</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">nmap</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">網路掃描工具，可掃描埠號與偵測服務。<br>
        <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">brew install nmap</code></td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>終端機指令列工具</h2>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#1e40af;font-weight:600;">macOS 內建指令列工具 — 快速檢測系統狀態</p>
</div>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#567d46;"># 查看開機啟動項目</span>
<span style="color:#569cd6;">ls</span> <span style="color:#4ec9b0;">-la</span> ~/Library/LaunchAgents
<span style="color:#569cd6;">ls</span> <span style="color:#4ec9b0;">-la</span> /Library/LaunchAgents
<span style="color:#569cd6;">launchctl</span> list

<span style="color:#567d46;"># 查看網路連線</span>
<span style="color:#569cd6;">lsof</span> <span style="color:#4ec9b0;">-i -P</span>
<span style="color:#569cd6;">netstat</span> <span style="color:#4ec9b0;">-an</span>
<span style="color:#569cd6;">networksetup</span> <span style="color:#4ec9b0;">-listallhardwareports</span>

<span style="color:#567d46;"># 查看程序與資源</span>
<span style="color:#569cd6;">ps</span> <span style="color:#4ec9b0;">aux</span>
<span style="color:#569cd6;">top</span> <span style="color:#4ec9b0;">-l</span> <span style="color:#b5cea8;">1</span>
<span style="color:#569cd6;">vm_stat</span>
<span style="color:#569cd6;">df</span> <span style="color:#4ec9b0;">-h</span>

<span style="color:#567d46;"># 查看已安裝的 KEXT（核心擴充）</span>
<span style="color:#569cd6;">kextstat</span> <span style="color:#4ec9b0;">-l</span> <span style="color:#4ec9b0;">-b</span> <span style="color:#b5cea8;">[bundle-id]</span>

<span style="color:#567d46;"># 查看系統擴充（macOS 10.14+）</span>
<span style="color:#569cd6;">systemextensionsctl</span> list

<span style="color:#567d46;"># 查看 Safari 擴充</span>
<span style="color:#569cd6;">defaults</span> <span style="color:#4ec9b0;">read</span> com.apple.Safari ExtensionUniqueIdentifiers

<span style="color:#567d46;"># 查看已簽署的程式碼</span>
<span style="color:#569cd6;">codesign</span> <span style="color:#4ec9b0;">-dvvv</span> <span style="color:#9cdcfe;">/Applications/[AppName].app</span>

<span style="color:#567d46;"># 審計系統完整性保護（SIP）狀態</span>
<span style="color:#569cd6;">csrutil</span> status

<span style="color:#567d46;"># 查看系統日誌（終端機）</span>
<span style="color:#569cd6;">log</span> show <span style="color:#4ec9b0;">--predicate</span> <span style="color:#ce9178;">'process == "kernel"'</span> <span style="color:#4ec9b0;">--last</span> <span style="color:#b5cea8;">1h</span>
</pre>

<hr/>

<h2>macOS 內建安全機制</h2>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:30%;">功能</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">XProtect</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">macOS 內建的惡意軟體掃描引擎，自動在背景更新病毒碼，Spotlight 整合偵測。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Gatekeeper</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">防止安裝未經 Apple 簽署的應用程式。可在「系統偏好設定」→「安全性與隱私」調整等級。</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">System Integrity Protection (SIP)</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">防止 Root 層級的篡改攻擊，限制Root使用者對系統目錄的寫入。開機時可用 <code>csrutil disable</code> 暫時關閉（不建議）。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">FileVault 2</code> ⭐ 內建</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">全磁碟加密，保護靜態資料。可在「系統偏好設定」→「安全性與隱私」啟用。</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Notarization</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Apple 的程式公證機制，公證後的軟體可通過 Gatekeeper 不出現安全性警告。</td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>必備工具精選 ⭐</h2>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#166534;font-weight:600;">macOS 安全檢測的建議組合：</p>
  <ol style="margin:0;padding-left:20px;color:#15803d;line-height:1.8;">
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">Knockknock</code> — 開機項目掃描（必備）</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">Malwarebytes</code> — 惡意軟體移除</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">LuLu / Little Snitch</code> — 網路連線監控</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">RansomWhere?</code> — 勒索軟體防護</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">BlockBlock</code> — 持續性安裝監控</li>
    <li><code style="background:#dcfce7;padding:2px 6px;border-radius:4px;">Console / log show</code> — 日誌分析</li>
  </ol>
</div>

<hr/>

<h3>與 Windows 版本對照參考</h3>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">Windows 工具</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">macOS 對應工具</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Autoruns</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Knockknock</code>、<code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">launchctl list</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Process Explorer</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Activity Monitor</code>、<code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">TaskExplorer</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">TCPView</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">lsof -i</code>、<code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">netstat</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">RootkitRevealer</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">KextViewr</code>、<code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">kextstat</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">HiJackThis</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Knockknock</code>、<code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">BlockBlock</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Malwarebytes</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Malwarebytes for Mac</code>（同一家）</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">GMER</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">TaskExplorer</code> + <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Knockknock</code> 組合</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">AccessEnum</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">ls -la</code> + <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">chmod/chown</code>（Unix 權限檢查）</td>
    </tr>
  </tbody>
</table>