---
title: logging all user command history (已解決winscp/sftp問題)
date: 2012-11-23
---

<h2>Linux TTY 稽核工具測試案例</h2>

<p>本篇記錄三種 Linux 指令執行稽核工具的測試心得：<code>pam_tty_audit.so</code>、<code>sudosh2</code>、<code>snoopylogger</code>。</p>

<hr/>

<h2>Testing Case 1：pam_tty_audit.so</h2>

<p>參考：<a href="http://jaredrobinson.com/blog/linux-tty-auditing/" target="_blank">jaredrobinson.com/blog/linux-tty-auditing</a></p>

<h3>設定方式</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#567d46;"># vi /etc/pam.d/system-auth (add)</span>
<span style="color:#569cd6;">session</span>     required      <span style="color:#9cdcfe;">pam_tty_audit.so enable=*</span>

<span style="color:#567d46;"># yum install psacct audit sudo system-config-audit.x86_64</span>
<span style="color:#569cd6;">/etc/audit/auditd.conf</span>
<span style="color:#567d46;"># check for selinux</span>
<span style="color:#569cd6;">sestatus</span>
</pre>

<h3>測試結論</h3>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>一般實體伺服器可正常運作，但 AWS 虛擬伺服器無法使用！</strong>（因為無法正式登入）這是個重大 bug！</p>
  <p style="margin:8px 0 0 0;color:#9a3412;">新版本 RHEL 已修正，但 CentOS 5.5 只能以 patch 方式處理。<strong>未測試此方式，因風險較大！</strong></p>
</div>

<hr/>

<h2>Testing Case 2：sudosh2</h2>

<p>下載：<a href="http://sourceforge.net/projects/sudosh2/" target="_blank">sudosh2-1.0.4.tgz</a></p>

<h3>編譯與安裝</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">yum install</span> <span style="color:#4ec9b0;">-y</span> gcc make
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">xvf</span> sudosh2-1.0.4.tgz
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix=/usr/local/sudosh2</span> <span style="color:#4ec9b0;">--enable-recordinput</span>
<span style="color:#569cd6;">make</span> ; <span style="color:#569cd6;">make check</span> ; <span style="color:#569cd6;">make install</span>
<span style="color:#569cd6;">cd</span> <span style="color:#9cdcfe;">/usr/local/sudosh2</span>
<span style="color:#569cd6;">man</span> <span style="color:#4ec9b0;">-M</span> <span style="color:#9cdcfe;">/usr/local/sudosh2/share/man/</span> sudosh
</pre>

<h3>設定檔</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#567d46;"># vi /etc/sudosh.conf</span>
<span style="color:#9cdcfe;">default shell</span>           = /bin/bash

<span style="color:#567d46;"># vi /etc/sudoers (add)</span>
<span style="color:#9cdcfe;">User_Alias</span>      <span style="color:#b5cea8;">ADMINS</span>=david,linda
<span style="color:#9cdcfe;">User_Alias</span>      <span style="color:#b5cea8;">OTHERS</span>=myop
<span style="color:#9cdcfe;">Cmnd_Alias</span>      <span style="color:#b5cea8;">SUDOSH</span>=/usr/local/bin/sudosh

<span style="color:#b5cea8;">ADMINS</span>          <span style="color:#9cdcfe;">ALL=SUDOSH</span>
<span style="color:#b5cea8;">OTHERS</span>          <span style="color:#9cdcfe;">ALL=/usr/local/bin/sudosh</span>

<span style="color:#567d46;"># vi /etc/shells</span>
<span style="color:#9cdcfe;">/usr/local/sudosh2/bin/sudosh</span>
</pre>

<h3>錄製與回放</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#b5cea8;">[root@my500g bin]# ./sudosh-replay</span>
Date                Duration From         To           ID
====                ======== ====         ==           ==
11/09/2012 01:54:02 3s       root         david        root-david-1352444042-q3mrDMJn0hjhORsQ
11/09/2012 01:54:12 1s       root         david        root-david-1352444052-hO8j2wvJzDbgyG33
11/09/2012 01:56:23 4m55s    linda        linda        linda-linda-1352444183-QNs12b1YPXYoavId
11/09/2012 02:01:25 1s       linda        linda        linda-linda-1352444485-AqXxLFF8sjeDNDbU

<span style="color:#b5cea8;">Usage: sudosh-replay ID [MULTIPLIER] [MAXWAIT]</span>
Example: <span style="color:#9cdcfe;">sudosh-replay root-david-1352444052-hO8j2wvJzDbgyG33 1 2</span>
<span style="color:#b5cea8;">[root@my500g bin]# ./sudosh-replay root-david-1352444042-q3mrDMJn0hjhORsQ 5</span>
<span style="color:#567d46;">(動態演繹其作業及指令)</span>
</pre>

<h3>測試結論</h3>

<ol style="color:#374151;line-height:1.8;">
  <li>基本測試正常</li>
  <li>紀錄方式為 <strong>hash</strong>，依 session 紀錄</li>
  <li>須以 <code>sudosh-replay</code> 進行解碼，會演繹所有指令及行為，可調整演繹速度</li>
  <li>可針對 user 設定是否紀錄（<code>/etc/passwd</code>），並使用 native shell</li>
</ol>

<hr/>

<h2>Testing Case 3：snoopylogger</h2>

<p>提供指令執行日誌。專案：<a href="http://sourceforge.net/projects/snoopylogger/" target="_blank">snoopylogger</a>、<a href="http://sourceforge.net/projects/rootsh/" target="_blank">rootsh</a></p>

<h3>編譯與啟用</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">wget</span> <span style="color:#9cdcfe;">http://downloads.sourceforge.net/project/snoopylogger/snoopy-1.8.0.tar.gz</span>
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">xvf</span> snoopy-1.8.0.tar.gz
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix=/usr/local/snoopy</span>
<span style="color:#569cd6;">make</span> ; <span style="color:#569cd6;">make install</span>
<span style="color:#569cd6;">make enable</span>
<span style="color:#569cd6;">./enable.sh</span> <span style="color:#9cdcfe;">/usr/local/snoopy/lib</span>

<span style="color:#ce9178;">Snoopy enabled in /etc/ld.so.preload. Check syslog messages for output.</span>
</pre>

<h3>錄製範例（/var/log/secure）</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#9cdcfe;">Nov  9 02:13:14 my500g snoopy[10115]</span>: <span style="color:#4ec9b0;">[uid:503 sid:10113 tty:/dev/pts/2 cwd:/home/david filename:/usr/bin/id]</span>: id -un
<span style="color:#9cdcfe;">Nov  9 02:13:14 my500g snoopy[10117]</span>: <span style="color:#4ec9b0;">[uid:503 sid:10113 tty:/dev/pts/2 cwd:/home/david filename:/bin/hostname]</span>: /bin/hostname
<span style="color:#9cdcfe;">Nov  9 02:13:20 my500g snoopy[10137]</span>: <span style="color:#4ec9b0;">[uid:503 sid:10113 tty:/dev/pts/2 cwd:/var/log filename:/usr/bin/vim]</span>: vim secure
<span style="color:#9cdcfe;">Nov  9 02:13:24 my500g snoopy[10140]</span>: <span style="color:#4ec9b0;">[uid:503 sid:10113 tty:/dev/pts/2 cwd:/var/log filename:/usr/bin/sudo]</span>: sudo su -
<span style="color:#9cdcfe;">Nov  9 02:13:24 my500g snoopy[10143]</span>: <span style="color:#4ec9b0;">[uid:0 sid:10113 tty:/dev/pts/2 cwd:/var/log filename:/bin/su]</span>: su -
<span style="color:#9cdcfe;">Nov  9 02:13:25 my500g snoopy[10158]</span>: <span style="color:#4ec9b0;">[uid:0 sid:10113 tty: cwd:/root filename:/bin/egrep]</span>: /bin/egrep -q (^|:)/sbin($|:)
<span style="color:#9cdcfe;">Nov  9 02:13:31 my500g snoopy[10182]</span>: <span style="color:#4ec9b0;">[uid:0 sid:10113 tty:/dev/pts/2 cwd:/var/log filename:/usr/bin/vim]</span>: vim messages
<span style="color:#9cdcfe;">Nov  9 02:14:09 my500g snoopy[10188]</span>: <span style="color:#4ec9b0;">[uid:0 sid:10113 tty:/dev/pts/2 cwd:/var/log filename:/bin/grep]</span>: grep sid:10113 secure
</pre>

<h3>測試結論</h3>

<ol style="color:#374151;line-height:1.8;">
  <li>基本測試正常</li>
  <li>紀錄方式為 <strong>clear text</strong>，統一寫入 <code>/var/log/secure</code></li>
  <li>可導入 remote server via <strong>syslog</strong>，調整 <code>syslog-facility</code> 和 <code>syslog-level</code></li>
  <li>可過濾紀錄，但須先設定 <strong>filter-command</strong></li>
</ol>

<hr/>

<h2>最終結論</h2>

<table style="width:100%;max-width:700px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">情境</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">建議工具</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">原因</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">少數伺服器及人員監控</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">sudosh2</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">方便查詢、易於管理</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">大量伺服器及人員監控</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">snoopylogger</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">可統一導入 syslog server</td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>附錄：sudosh2 的 SFTP 支援修正</h2>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#166534;"><strong>問題：</strong>sudosh2 無法搭配 sftp 使用</p>
  <p style="margin:0;color:#15803d;">參考 <a href="https://github.com/wojo/sudosh2/blob/master/README" target="_blank" style="color:#166534;text-decoration:underline;">sudosh2 README</a> 說明：</p>
  <p style="margin:8px 0 0 0;color:#166534;font-size:13px;"><code>4) To allow things like scp, sftp, cvs, rsync, etc, use the "-c arg allow" in sudosh.conf.</code></p>
</div>

<h3>發現：系統有兩支 sftp-server</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#b5cea8;">[root@tw log]# find / -type f -name "sftp-server"</span>
<span style="color:#9cdcfe;">/usr/libexec/openssh/sftp-server</span>   <span style="color:#567d46;"># 預設安裝及 sshd_config 設定</span>
<span style="color:#9cdcfe;">/usr/libexec/sftp-server</span>            <span style="color:#567d46;"># 真正使用，但其他 AWS server 並無此檔</span>

<span style="color:#b5cea8;">[root@tw ~]# rpm -q -l openssh-server | grep sftp-server</span>
<span style="color:#9cdcfe;">/usr/libexec/openssh/sftp-server</span>
</pre>

<h3>修改 /etc/sudosh.conf</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#567d46;"># vi /etc/sudosh.conf</span>
<span style="color:#9cdcfe;">logdir</span>                  = /var/log/sudosh
<span style="color:#9cdcfe;">default shell</span>           = /bin/bash
<span style="color:#9cdcfe;">delimiter</span>               = -
<span style="color:#9cdcfe;">syslog.priority</span>         = <span style="color:#b5cea8;">LOG_INFO</span>
<span style="color:#9cdcfe;">syslog.facility</span>         = <span style="color:#b5cea8;">LOG_LOCAL2</span>
<span style="color:#9cdcfe;">clearenvironment</span>        = yes

<span style="color:#567d46;"># Allow Sudosh to execute -c arguments? If so, what?</span>
<span style="color:#9cdcfe;">-c arg allow</span> = /usr/libexec/sftp-server
<span style="color:#9cdcfe;">-c arg allow</span> = scp
<span style="color:#9cdcfe;">-c arg allow</span> = rsync
</pre>

<div style="background:#dcfce7;border-left:4px solid #22c55e;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;"><strong>測試成功：</strong></p>
  <pre style="margin:8px 0 0 0;background:#1e1e1e;color:#22c55e;padding:8px 12px;border-radius:4px;font-family:monospace;font-size:13px;">
sftp -v david@2xx.xxx.xxx.xxx</pre>
</div>