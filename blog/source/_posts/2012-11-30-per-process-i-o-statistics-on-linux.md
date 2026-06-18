---
title: per-process I/O statistics on Linux
date: 2012-11-30
---

<!-- 建議：在 Blogger 后台「新增页面」→「HTML 视图」贴上此代码 -->

<h2>Linux Kernel I/O 統計：per-process I/O statistics</h2>

<p>Linux Kernel 2.6.20 以後，kernel 本身就支援 <strong>per-process I/O statistics</strong>。以前的版本則需要上 kernel patch。</p>

<h3>iotop 工具</h3>

<p>官方網站：<a href="http://guichaz.free.fr/iotop/" target="_blank">http://guichaz.free.fr/iotop/</a></p>

<h4>安裝需求</h4>

<ul>
  <li>Linux &gt;= 2.6.20 with
    <ul>
      <li>I/O accounting support</li>
      <li><code>CONFIG_TASKSTATS</code></li>
      <li><code>CONFIG_TASK_DELAY_ACCT</code></li>
      <li><code>CONFIG_TASK_IO_ACCOUNTING</code></li>
    </ul>
  </li>
  <li>Python &gt;= 2.5 或 Python 2.4 + ctypes module</li>
</ul>

<h4>參考文章</h4>

<p><a href="http://lserinol.blogspot.tw/2009/09/io-usage-per-process-on-linux.html" target="_blank">Linux I/O per process (Linux &gt;= 2.6.20)</a></p>

<hr/>

<h2>Kernel &lt; 2.6.20 的方式</h2>

<p>測試環境：<strong>Kernel 2.6.16.33-xenU</strong></p>

<p>使用 <code>iodump</code> 工具（由 XtraDB 提供）：</p>

<pre style="color: #1e293b; background: #fff7ed; border-left: 4px solid #f97316; padding: 16px 20px; margin: 20px 0;">
wget http://aspersa.googlecode.com/svn/trunk/iodump
</pre>

<h3>startio.sh 脚本</h3>

<p>可以使用 <code>./startio.sh</code> 來呼叫 <code>iodump</code> 執行：</p>

<pre style="background:#1e1e1e;color:#d4d4d4;padding:12px;border-radius:6px;overflow-x:auto;line-height:1.6;">
================= startio.sh =================
#!/bin/bash
dmesg -c
echo 1 &gt; /proc/sys/vm/block_dump
sleep 30
go=0
while [ $go = 0 ]; do
  sleep 1
  clear
  dmesg | perl iodump
  trap "go=1" SIGINT
done
echo 0 &gt; /proc/sys/vm/block_dump
dmesg -c
clear
echo Caught SIGINT.
================= startio.sh =================
</pre>

<h3>顯示結果</h3>

<p><strong>執行後須等約 10 秒</strong>才會看到輸出：</p>

<table style="width:auto;border-collapse:collapse;margin:16px 0;">
  <thead>
    <tr style="background:#1e1e1e;">
      <th style="padding:8px 12px;border:1px solid #ccc;text-align:left;">TASK</th>
      <th style="padding:8px 12px;border:1px solid #ccc;text-align:right;">PID</th>
      <th style="padding:8px 12px;border:1px solid #ccc;text-align:right;">TOTAL</th>
      <th style="padding:8px 12px;border:1px solid #ccc;text-align:right;">READ</th>
      <th style="padding:8px 12px;border:1px solid #ccc;text-align:right;">WRITE</th>
      <th style="padding:8px 12px;border:1px solid #ccc;text-align:right;">DIRTY</th>
      <th style="padding:8px 12px;border:1px solid #ccc;text-align:left;">DEVICES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding:8px 12px;border:1px solid #ccc;">kjournald</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">686</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">246</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">0</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">246</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">0</td>
      <td style="padding:8px 12px;border:1px solid #ccc;">sda1</td>
    </tr>
    <tr style="background:#1e1e1e;">
      <td style="padding:8px 12px;border:1px solid #ccc;">pdflush</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">69</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">18</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">0</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">18</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">0</td>
      <td style="padding:8px 12px;border:1px solid #ccc;">sda1</td>
    </tr>
    <tr>
      <td style="padding:8px 12px;border:1px solid #ccc;">syslogd</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">1758</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">1</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">0</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">1</td>
      <td style="padding:8px 12px;border:1px solid #ccc;text-align:right;">0</td>
      <td style="padding:8px 12px;border:1px solid #ccc;">sda1</td>
    </tr>
  </tbody>
</table>

<p>此方法適用於 <strong>Kernel &lt; 2.6.20</strong> 的舊系統，透過 <code>block_dump</code> 機制即時監控每個程序的 I/O 活動。</p>