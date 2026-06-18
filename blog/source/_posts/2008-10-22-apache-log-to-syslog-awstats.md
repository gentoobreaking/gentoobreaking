---
title: Apache Log To Syslog & AWStats
date: 2008-10-22
categories: 伺服器管理
tags: [Apache, AWStats, Syslog]
---

<h2>Apache Log To Syslog &amp; AWStats</h2>

<h3>目的：統計多站台檔案存取次數及流量</h3>

<hr/>

<h2>一、Apache Log To Syslog</h2>

<h3>參考資源</h3>

<ul style="color:#374151;line-height:2;">
  <li><a href="http://www.oreillynet.com/pub/a/sysadmin/2006/10/12/httpd-syslog.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">O'Reilly - HTTPd Syslog</a></li>
  <li><a href="http://www.devshed.com/c/a/Apache/Logging-in-Apache/4/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">DevShed - Logging in Apache</a></li>
  <li><a href="http://phorum.study-area.org/index.php?topic=26137.0" target="_blank" style="color:#1d4ed8;text-decoration:underline;">Study-Area 討論串</a></li>
  <li><a href="http://linux.chinaunix.net/techdoc/net/2007/11/30/973513.shtml" target="_blank" style="color:#1d4ed8;text-decoration:underline;">Linux ChinaUnix Techdoc</a></li>
</ul>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>⚠️</strong> <strong>Remote Log Server IP：</strong><code style="background:#fff7ed;color:#c2410c;padding:2px 6px;border-radius:4px;">@remotelogsrvip</code></p>
</div>

<h3>開機時自動啟動 tail + logger</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/rc.local 加入下列</span>
<span style="color:#569cd6;">nohup</span> tail <span style="color:#4ec9b0;">-f</span> /var/log/httpd/error_log | /usr/bin/logger <span style="color:#4ec9b0;">-p</span> local1.info &amp;
<span style="color:#569cd6;">nohup</span> tail <span style="color:#4ec9b0;">-f</span> /var/log/httpd/access_log | /usr/bin/logger <span style="color:#4ec9b0;">-p</span> local2.info &amp;
</pre>

<h3>Syslog 設定：傳送到遠端伺服器</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/syslog.conf</span>
<span style="color:#9cdcfe;">local1.*</span>  <span style="color:#ce9178;">@remotelogsrvip</span>
<span style="color:#9cdcfe;">local2.*</span>  <span style="color:#ce9178;">@remotelogsrvip</span>
<span style="color:#9cdcfe;">*.*</span>       <span style="color:#ce9178;">@remotelogsrvip</span>
</pre>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> 需手動執行一次（開機後若 rc.local 尚未觸發）：p>
</div>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">nohup</span> tail <span style="color:#4ec9b0;">-f</span> /var/log/httpd/error_log | /usr/bin/logger <span style="color:#4ec9b0;">-p</span> local1.info &amp;
<span style="color:#569cd6;">nohup</span> tail <span style="color:#4ec9b0;">-f</span> /var/log/httpd/access_log | /usr/bin/logger <span style="color:#4ec9b0;">-p</span> local2.info &amp;

<span style="color:#569cd6;">service</span> syslog restart
</pre>

<h3>Syslog 設定：寫入本地端檔案</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/syslog.conf</span>
<span style="color:#9cdcfe;">local1.*</span>  /var/log/apache/error_log
<span style="color:#9cdcfe;">local2.*</span>  /var/log/apache/access_log
</pre>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># 需手動執行一次</span>
<span style="color:#569cd6;">mkdir</span> /var/log/apache
<span style="color:#569cd6;">service</span> syslog restart
</pre>

<h3>修改 Log 自動封存時間</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/logrotate.conf</span>

<span style="color:#9cdcfe;">/var/log/httpd/access_log</span> {
  <span style="color:#9cdcfe;">daily</span>
  <span style="color:#569cd6;">create</span> <span style="color:#b5cea8;">0664</span> root root
  <span style="color:#9cdcfe;">rotate</span> <span style="color:#b5cea8;">90</span>
}

<span style="color:#9cdcfe;">/var/log/httpd/error_log</span> {
  <span style="color:#9cdcfe;">daily</span>
  <span style="color:#569cd6;">create</span> <span style="color:#b5cea8;">0664</span> root root
  <span style="color:#9cdcfe;">rotate</span> <span style="color:#b5cea8;">90</span>
}
</pre>

<hr/>

<h2>二、AWStats 安裝與設定</h2>

<h3>參考資源</h3>

<ul style="color:#374151;line-height:2;">
  <li><a href="http://awstats.sourceforge.net/docs/awstats_setup.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">AWStats 官方設定文件</a></li>
  <li><a href="http://www.chedong.com/tech/awstats.html" target="_blank" style="color:#1d4ed8;text-decoration:underline;">chedong.com AWStats 教學</a></li>
  <li><a href="http://dennys.tiger2.net/zh-hant/blog/2007/02/02/awstats" target="_blank" style="color:#1d4ed8;text-decoration:underline;">Denny's AWStats 繁體中文設定</a></li>
</ul>

<h3>安裝</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">rpm</span> <span style="color:#4ec9b0;">-i</span> awstats-6.8-1.noarch.rpm
</pre>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> AWStats 安裝三步驟：</p>
  <ol style="margin:8px 0 0 0;padding-left:20px;color:#1e40af;line-height:1.8;">
    <li>Install and Setup with <code style="background:#dbeafe;padding:2px 4px;border-radius:3px;color:#1e40af;">awstats_configure.pl</code></li>
    <li>Build/Update Statistics with <code style="background:#dbeafe;padding:2px 4px;border-radius:3px;color:#1e40af;">awstats.pl</code></li>
    <li>Read Statistics</li>
  </ol>
</div>

<h3>設定 Apache Alias</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/httpd/conf/httpd.conf 加入下列</span>

<span style="color:#569cd6;">Alias</span> /awstatsclasses <span style="color:#ce9178;">"/usr/local/awstats/wwwroot/classes/"</span>
<span style="color:#569cd6;">Alias</span> /awstatscss <span style="color:#ce9178;">"/usr/local/awstats/wwwroot/css/"</span>
<span style="color:#569cd6;">Alias</span> /awstatsicons <span style="color:#ce9178;">"/usr/local/awstats/wwwroot/icon/"</span>
<span style="color:#569cd6;">ScriptAlias</span> /awstats/ <span style="color:#ce9178;">"/usr/local/awstats/wwwroot/cgi-bin/"</span>

<span style="color:#569cd6;">Options</span> None
<span style="color:#569cd6;">AllowOverride</span> None
<span style="color:#569cd6;">Order</span> allow,deny
<span style="color:#569cd6;">Allow from</span> all
</pre>

<h3>執行 awstats_configure.pl</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> /usr/local/awstats/
<span style="color:#569cd6;">perl</span> awstats_configure.pl
</pre>

<p style="color:#374151;margin-top:16px;">畫面輸出：</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;">-----> Create config file '/etc/awstats/awstats.ftp.xxx.com.tw.conf'</span>
<span style="color:#4ec9b0;">Config file /etc/awstats/awstats.ftp.xxx.com.tw.conf created.</span>

<span style="color:#6a9955;">-----> Add update process inside a scheduler</span>
<span style="color:#f97316;">Sorry, configure.pl does not support automatic add to cron yet.</span>
<span style="color:#6a9955;">You can do it manually by adding the following command to your cron:</span>
/usr/local/awstats/wwwroot/cgi-bin/awstats.pl -update -config=ftp.xxx.com.tw
<span style="color:#6a9955;">Or if you have several config files:</span>
/usr/local/awstats/tools/awstats_updateall.pl now

<span style="color:#6a9955;"># 瀏覽 AWStats 報表</span>
<span style="color:#ce9178;">http://localhost/awstats/awstats.pl?config=ftp.xxx.com.tw</span>
</pre>

<h3>修改 AWStats 設定檔</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/awstats/awstats.ftp.xxx.com.tw.conf</span>
<span style="color:#6a9955;">##LogFile="/var/log/httpd/mylog.log"</span>
<span style="color:#9cdcfe;">LogFile</span>=<span style="color:#ce9178;">"/var/log/apache/access_log"</span>
<span style="color:#6a9955;">##DirData="/var/lib/awstats"</span>
<span style="color:#9cdcfe;">DirData</span>=<span style="color:#ce9178;">"/usr/local/awstats/"</span>
</pre>

<h3>手動更新統計資料</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">/usr/local/awstats/wwwroot/cgi-bin/awstats.pl</span> <span style="color:#4ec9b0;">-update</span> <span style="color:#4ec9b0;">-config</span>=ftp.xxx.com.tw
<span style="color:#569cd6;">/usr/local/awstats/wwwroot/cgi-bin/awstats.pl</span> <span style="color:#4ec9b0;">-update</span> <span style="color:#4ec9b0;">-config</span>=autopatch2.xxx.com.tw
</pre>

<h3>瀏覽報表</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#ce9178;">http://localhost/awstats/awstats.pl?config=ftp.xxx.com.tw</span>
</pre>

<h3>加入 Crontab 自動更新</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># crontab -e</span>
<span style="color:#b5cea8;">5</span> * * * * /usr/local/awstats/tools/awstats_updateall.pl now
</pre>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>⚠️</strong> <strong>注意：</strong><code style="background:#fff7ed;color:#c2410c;padding:2px 6px;border-radius:4px;">awstats_configure.pl</code> 目前還不支援自動寫入 crontab，需手動自行新增。</p>
</div>

<hr/>

<h3>流程總覽</h3>

<div style="background:#f8fafc;border:1px solid #e2e8f0;border-radius:8px;padding:20px;margin:16px 0;text-align:center;font-family:monospace;font-size:13px;line-height:2.2;color:#374151;">
  <strong style="color:#1e40af;">Apache Log</strong>（access_log / error_log）
  → <strong style="color:#d97706;">tail + logger</strong>（Local1 / Local2）
  → <strong style="color:#7c3aed;">Syslog Server</strong>
  → <strong style="color:#dc2626;">AWStats</strong>（每小時更新）
  → <strong style="color:#059669;">Web UI 報表</strong>
</div>

<hr/>

<h3>功能對照表</h3>

<table style="width:100%;max-width:850px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.08);">
  <caption style="text-align:left;padding:0 0 8px 4px;font-weight:600;color:#374151;font-size:15px;">Apache Log To Syslog &amp; AWStats 功能對照</caption>
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">項目</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">功能說明</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">相關設定</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">logger -p local1.info</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Apache Error Log 轉 syslog（local1）</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">/etc/syslog.conf</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">logger -p local2.info</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Apache Access Log 轉 syslog（local2）</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">/etc/syslog.conf</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">logrotate</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Log 自動封存（預設 90 天）</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">/etc/logrotate.conf</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">awstats_configure.pl</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">快速建立站台設定檔</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">/usr/local/awstats/</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">awstats.pl -update</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">更新統計資料庫</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">/etc/awstats/*.conf</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">awstats_updateall.pl</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">一次更新所有站台統計</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">加入 crontab 每小時執行</td>
    </tr>
  </tbody>
</table>

<hr/>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;"><strong>💜</strong> Apache Log To Syslog + AWStats 組合可以完整掌握多站台的流量、訪客、檔案存取次數等資訊，適合需要統計分析與流量監控的網站管理需求。</p>
</div>