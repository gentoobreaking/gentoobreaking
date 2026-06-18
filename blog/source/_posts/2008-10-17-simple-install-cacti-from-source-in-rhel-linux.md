---
title: Simple Install Cacti from source in RHEL / Linux
date: 2008-10-17
---

<h2>從原始碼編譯安裝 Cacti（RHEL / Linux）</h2>

<p>完整記錄從原始碼編譯安裝 Cacti 網路監控系統的流程，含 MySQL、Apache、PHP、RRDTool、Net-SNMP 等必要元件。</p>

<hr/>

<h2>Cacti 需求環境</h2>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> 參考：<a href="http://www.cacti.net/downloads/docs/html/requirements.html" target="_blank" style="color:#1e40af;text-decoration:underline;">Cacti 官方需求文件</a></p>
</div>

<table style="width:100%;max-width:700px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.08);">
  <caption style="text-align:left;padding:0 0 8px 4px;font-weight:600;color:#374151;font-size:15px;">軟體需求版本</caption>
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">軟體</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">最低版本</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">RRDTool</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">1.0.49 或 1.2.x 以上</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">MySQL</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">4.1.x 或 5.x 以上</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">PHP</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">4.3.6 以上，強烈建議 5.x</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Web Server</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Apache 或 IIS</td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>下載軟體套件</h2>

<p>將所有套件放置於 <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">/usr/local/src</code> 目錄。</p>

<table style="width:100%;max-width:900px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:13px;box-shadow:0 1px 3px rgba(0,0,0,0.08);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">軟體</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">版本</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">來源</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">httpd</code></td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">2.2.9</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;"><a href="http://www.apache.org/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">apache.org</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">PHP</code></td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">5.2.6</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;"><a href="http://www.php.net/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">php.net</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">MySQL</code></td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">5.0.67</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;"><a href="http://www.mysql.com/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">mysql.com</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">RRDTool</code></td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">1.3.4</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;"><a href="http://oss.oetiker.ch/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">oetiker.ch</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">net-snmp</code></td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">5.4.2</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;"><a href="http://www.net-snmp.org/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">net-snmp.org</a></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">libxml2</code></td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">2.7.2</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;"><a href="ftp://xmlsoft.org/libxml2/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">xmlsoft.org</a></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">pango, cairo, freetype, pixman</code></td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">1.21.1 / 1.6.4 / 2.3.5 / 0.10.0</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;"><a href="http://oss.oetiker.ch/rrdtool/pub/libs/" target="_blank" style="color:#1d4ed8;text-decoration:underline;">oetiker.ch/rrdtool/libs</a></td>
    </tr>
  </tbody>
</table>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>⚠️</strong> 所有套件放置於 <code style="background:#fff7ed;color:#c2410c;padding:2px 6px;border-radius:4px;">/usr/local/src</code> 並切換至此目錄後開始編譯。</p>
</div>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> /usr/local/src
</pre>

<hr/>

<h2>Step 1：安裝 net-snmp-5.4.2</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> net-snmp-5.4.2.tar.gz
<span style="color:#569cd6;">cd</span> net-snmp-5.4.2
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix</span>=/usr/local/net-snmp-5.4.2 <span style="color:#4ec9b0;">--with-perl-modules</span>
<span style="color:#569cd6;">make</span>
<span style="color:#569cd6;">make test</span>   <span style="color:#6a9955;"># 請注意是否有 error msg</span>
<span style="color:#569cd6;">make install</span>
</pre>

<hr/>

<h2>Step 2：安裝 Apache HTTP Server</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> httpd-2.2.9.tar.gz
<span style="color:#569cd6;">cd</span> httpd-2.2.9
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix</span>=/usr/local/httpd-2.2.9
<span style="color:#569cd6;">make</span>
<span style="color:#569cd6;">make install</span>
<span style="color:#569cd6;">ln</span> <span style="color:#4ec9b0;">-s</span> /usr/local/httpd-2.2.9 /usr/local/apache
<span style="color:#569cd6;">mkdir</span> /var/www
</pre>

<hr/>

<h2>Step 3：安裝 MySQL 5.0.67</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> /usr/local/src
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> mysql-5.0.67-linux-i686.tar.gz
<span style="color:#569cd6;">mv</span> mysql-5.0.67-linux-i686 /usr/local/
<span style="color:#569cd6;">ln</span> <span style="color:#4ec9b0;">-s</span> /usr/local/mysql-5.0.67-linux-i686 /usr/local/mysql
<span style="color:#569cd6;">cd</span> /usr/local/mysql
<span style="color:#569cd6;">groupadd</span> mysql
<span style="color:#569cd6;">useradd</span> <span style="color:#4ec9b0;">-g</span> mysql mysql
<span style="color:#569cd6;">chown</span> <span style="color:#4ec9b0;">-R</span> mysql .
<span style="color:#569cd6;">chgrp</span> <span style="color:#4ec9b0;">-R</span> mysql .
<span style="color:#569cd6;">scripts/mysql_install_db</span> <span style="color:#4ec9b0;">--user</span>=mysql
<span style="color:#569cd6;">chown</span> <span style="color:#4ec9b0;">-R</span> root .
<span style="color:#569cd6;">chown</span> <span style="color:#4ec9b0;">-R</span> mysql data
<span style="color:#569cd6;">bin/mysqld_safe</span> <span style="color:#4ec9b0;">--user</span>=mysql &amp;
<span style="color:#569cd6;">cd</span> bin/
<span style="color:#569cd6;">./mysqladmin</span> password <span style="color:#ce9178;">p@ssw0rd</span>
</pre>

<hr/>

<h2>Step 4：安裝 libxml2（PHP 編譯前置需求）</h2>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>⚠️</strong> PHP 5.x 編譯需要 <code style="background:#fff7ed;color:#c2410c;padding:2px 6px;border-radius:4px;">libxml2 version 2.6.11</code> 以上，否則會出現：<em>configure: error: libxml2 version 2.6.11 or greater required.</em></p>
</div>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">yum install</span> libxml2-devel
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> libxml2-2.7.2.tar.gz
<span style="color:#569cd6;">cd</span> libxml2-2.7.2
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix</span>=/usr/local/libxml2-2.7.2
<span style="color:#569cd6;">make</span>
<span style="color:#569cd6;">make test</span>   <span style="color:#6a9955;"># 請注意是否有 error msg</span>
<span style="color:#569cd6;">make install</span>
</pre>

<hr/>

<h2>Step 5：安裝 PHP 5.2.6</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> php-5.2.6.tar.gz
<span style="color:#569cd6;">cd</span> php-5.2.6
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">\
  --prefix</span>=/usr/local/php-5.2.6 <span style="color:#4ec9b0;">\
  --with-mysql</span> <span style="color:#4ec9b0;">\
  --with-apxs2</span>=/usr/local/apache/bin/apxs <span style="color:#4ec9b0;">\
  --with-libxml-dir</span>=/usr/local/libxml2-2.7.2 <span style="color:#4ec9b0;">\
  --with-mysql</span>=/usr/local/mysql <span style="color:#4ec9b0;">\
  --enable-sockets</span>
<span style="color:#569cd6;">make</span>
<span style="color:#569cd6;">make test</span>
<span style="color:#569cd6;">make install</span>
<span style="color:#569cd6;">cp</span> php.ini-dist /usr/local/lib/php.ini
</pre>

<h3>修改 httpd.conf 加入 PHP 支援</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /usr/local/apache/conf/httpd.conf</span>
<span style="color:#569cd6;">LoadModule</span> php5_module libexec/libphp5.so
<span style="color:#569cd6;">AddModule</span> mod_php5.c
<span style="color:#569cd6;">AddType</span> application/x-httpd-php .php .phtml
<span style="color:#569cd6;">AddType</span> application/x-httpd-php-source .phps
<span style="color:#569cd6;">AddHandler</span> php5-script .php
<span style="color:#569cd6;">AddType</span> text/html .php
<span style="color:#569cd6;">DirectoryIndex</span> index.php
</pre>

<h3>測試 Apache + PHP</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> /usr/local/apache/bin/
<span style="color:#569cd6;">./apachectl</span> start
</pre>

<p style="color:#374151;margin-top:12px;">建立測試檔案：</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /usr/local/apache/htdocs/test.php</span>
<span style="color:#4ec9b0;">&lt;?php</span>
<span style="color:#569cd6;">phpinfo</span>();
<span style="color:#4ec9b0;">?&gt;</span>
</pre>

<p style="color:#374151;">從瀏覽器存取：<code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">http://192.168.70.54/test.php</code></p>

<hr/>

<h2>Step 6：安裝圖形庫依賴（RRDTool 前置）</h2>

<h3>pixman-0.10.0</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> pixman-0.10.0.tar.gz
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix</span>=/usr/local/pixman-0.10.0
<span style="color:#569cd6;">make</span>
<span style="color:#569cd6;">make install</span>
</pre>

<h3>freetype-2.3.5（.tar.bz2）</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">jxvf</span> freetype-2.3.5.tar.bz2
<span style="color:#569cd6;">cd</span> freetype-2.3.5
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix</span>=/usr/local/freetype-2.3.5
<span style="color:#569cd6;">make</span>
<span style="color:#569cd6;">make install</span>
</pre>

<h3>cairo-1.6.4</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> cairo-1.6.4.tar.gz
<span style="color:#569cd6;">cd</span> cairo-1.6.4
<span style="color:#569cd6;">yum install</span> pkgconfig
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix</span>=/usr/local/cairo-1.6.4
<span style="color:#569cd6;">make</span>
<span style="color:#569cd6;">make install</span>
</pre>

<h3>pango-1.21.1（RRDTool 圖形依賴）</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">jxvf</span> pango-1.21.1.tar.bz2
<span style="color:#569cd6;">cd</span> pango-1.21.1
<span style="color:#569cd6;">./configure</span> <span style="color:#4ec9b0;">--prefix</span>=/usr/local/pango-1.21.1
<span style="color:#569cd6;">make</span>
<span style="color:#569cd6;">make check</span>
<span style="color:#569cd6;">make install</span>
</pre>

<hr/>

<h2>Step 7：安裝 RRDTool（RPM 方式）</h2>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>⚠️</strong> 由於原始碼編譯相依性太過複雜，改用 RPM 安裝。</p>
</div>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">yum install</span> perl-Time-HiRes
<span style="color:#569cd6;">rpm</span> <span style="color:#4ec9b0;">-ivh</span> rrdtool-1.2.23-1.el3.rf.i386.rpm perl-rrdtool-1.2.23-1.el3.rf.i386.rpm
</pre>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> 若要從原始碼編譯 RRDTool，請在完成上方圖形庫安裝後執行：<br>
  <code style="background:#dbeafe;padding:2px 6px;border-radius:4px;color:#1e40af;">./configure --prefix=/usr/local/rrdtool-1.3.4</code></p>
</div>

<hr/>

<h2>Step 8：安裝 Cacti 主程式</h2>

<h3>建立資料庫與使用者</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> /usr/local/src
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> cacti-0.8.7b.tar.gz
<span style="color:#569cd6;">cd</span> /usr/local/mysql/bin
<span style="color:#569cd6;">./mysql</span> <span style="color:#4ec9b0;">-u root -p</span> cacti &lt; /usr/local/src/cacti-0.8.7b/cacti.sql

<span style="color:#569cd6;">./mysql</span> <span style="color:#4ec9b0;">-u root -p</span> mysql
<span style="color:#4ec9b0;">mysql&gt;</span> <span style="color:#569cd6;">GRANT ALL ON</span> cacti.* TO cactiuser@localhost <span style="color:#569cd6;">IDENTIFIED BY</span> <span style="color:#ce9178;">'p@ssw0rd'</span>;
<span style="color:#4ec9b0;">mysql&gt;</span> <span style="color:#569cd6;">flush privileges</span>;
<span style="color:#4ec9b0;">mysql&gt;</span> quit
</pre>

<h3>部署網頁目錄</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> /usr/local/src
<span style="color:#569cd6;">mv</span> cacti-0.8.7b /usr/local/apache/htdocs/
<span style="color:#569cd6;">ln</span> <span style="color:#4ec9b0;">-s</span> /usr/local/apache/htdocs/cacti-0.8.7b /usr/local/apache/htdocs/cacti
</pre>

<h3>設定 config.php</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /usr/local/apache/htdocs/cacti/include/config.php</span>
<span style="color:#9cdcfe;">$database_type</span>     = <span style="color:#ce9178;">"mysql"</span>;
<span style="color:#9cdcfe;">$database_default</span> = <span style="color:#ce9178;">"cacti"</span>;
<span style="color:#9cdcfe;">$database_hostname</span> = <span style="color:#ce9178;">"localhost"</span>;
<span style="color:#9cdcfe;">$database_username</span> = <span style="color:#ce9178;">"cactiuser"</span>;
<span style="color:#9cdcfe;">$database_password</span> = <span style="color:#ce9178;">"p@ssw0rd"</span>;
</pre>

<h3>建立 cactiuser 並設定權限</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">groupadd</span> cactiuser
<span style="color:#569cd6;">useradd</span> cactiuser <span style="color:#4ec9b0;">-g</span> cactiuser <span style="color:#4ec9b0;">-s</span> /bin/sh <span style="color:#4ec9b0;">-d</span> /home/users/cactiuser
<span style="color:#569cd6;">cd</span> /usr/local/apache/htdocs/cacti
<span style="color:#569cd6;">chown</span> <span style="color:#4ec9b0;">-R</span> cactiuser rra/ log/
</pre>

<h3>加入 Crontab</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/crontab</span>
<span style="color:#b5cea8;">*/5</span> * * * * cactiuser /usr/local/php/bin/php /usr/local/apache/htdocs/cacti/poller.php &gt; /dev/null <span style="color:#b5cea8;">2&gt;&amp;1</span>
</pre>

<hr/>

<h2>Step 9：安裝 Cacti Plugin</h2>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> 下載位置：<a href="http://cactiusers.org/" target="_blank" style="color:#1e40af;text-decoration:underline;">cactiusers.org</a></p>
  <p style="margin:8px 0 0 0;color:#1e40af;">所需插件：<code style="background:#dbeafe;padding:2px 6px;border-radius:4px;color:#1e40af;">cacti-plugin-arch.tar.gz</code>、<code style="background:#dbeafe;padding:2px 6px;border-radius:4px;color:#1e40af;">discovery-0.8.4.tar.gz</code>、<code style="background:#dbeafe;padding:2px 6px;border-radius:4px;color:#1e40af;">monitor-0.8.2.tar.gz</code></p>
</div>

<h3>Plugin Architecture（必備，優先安裝）</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> cacti-plugin-arch.tar.gz
<span style="color:#569cd6;">cd</span> cacti-plugin-arch
<span style="color:#569cd6;">cp</span> cacti-plugin-0.8.7b-PA-v2.1.diff /usr/local/apache/htdocs/cacti/
<span style="color:#569cd6;">cd</span> /usr/local/apache/htdocs/cacti
<span style="color:#569cd6;">patch</span> <span style="color:#4ec9b0;">-p1 -N</span> &lt;&gt;
<span style="color:#569cd6;">cd</span> /usr/local/src/plugin/cacti-plugin-arch/
<span style="color:#569cd6;">/usr/local/mysql/bin/mysql</span> <span style="color:#4ec9b0;">-u root -p</span> cacti &lt;&gt;
</pre>

<p style="color:#374151;margin-top:12px;">修改 global.php：</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /usr/local/apache/htdocs/cacti/include/global.php</span>
<span style="color:#9cdcfe;">$config</span>[<span style="color:#ce9178;">'url_path'</span>] = <span style="color:#ce9178;">'/'</span>;    <span style="color:#6a9955;">→</span>   <span style="color:#9cdcfe;">$config</span>[<span style="color:#ce9178;">'url_path'</span>] = <span style="color:#ce9178;">'/cacti/'</span>;
</pre>

<h3>安裝 Discovery 插件</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> discovery-0.8.4.tar.gz
<span style="color:#569cd6;">mv</span> discovery /usr/local/apache/htdocs/cacti/plugins/
<span style="color:#569cd6;">cd</span> /usr/local/apache/htdocs/cacti/plugins/discovery/
<span style="color:#569cd6;">/usr/local/mysql/bin/mysql</span> <span style="color:#4ec9b0;">-u root -p</span> cacti &lt;&gt;
<span style="color:#6a9955;"># vi /usr/local/apache/htdocs/cacti/include/global.php</span>
<span style="color:#9cdcfe;">$plugins</span>[] = <span style="color:#ce9178;">'discovery'</span>;
</pre>

<h3>安裝 Monitor 插件</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> monitor-0.8.2.tar.gz
<span style="color:#569cd6;">mv</span> monitor /usr/local/apache/htdocs/cacti/plugins/
<span style="color:#569cd6;">cd</span> /usr/local/apache/htdocs/cacti/plugins/monitor/
<span style="color:#569cd6;">/usr/local/mysql/bin/mysql</span> <span style="color:#4ec9b0;">-u root -p</span> cacti &lt;&gt;
<span style="color:#6a9955;"># vi /usr/local/apache/htdocs/cacti/include/global.php</span>
<span style="color:#9cdcfe;">$plugins</span>[] = <span style="color:#ce9178;">'monitor'</span>;
</pre>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> 其他 Cacti 插件的安裝方式都差不多，以此類推即可。</p>
</div>

<hr/>

<h2>Tuning 優化</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">chmod</span> <span style="color:#b5cea8;">666</span> /usr/local/apache/htdocs/cacti/log/cacti.log
<span style="color:#569cd6;">chown</span> cactiuser.cactiuser /usr/local/apache/htdocs/cacti/log/cacti.log
</pre>

<hr/>

<h2>Syslog 整合至 Cacti（網路設備適用）</h2>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> 此功能適合用於 Network Device，大量 Host Device 不建議使用。</p>
</div>

<h3>安裝 Syslog 插件</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> syslog-0.5.2.tar.gz
<span style="color:#569cd6;">mv</span> syslog /usr/local/apache/htdocs/cacti/plugins/
<span style="color:#6a9955;"># vi /usr/local/apache/htdocs/cacti/include/global.php</span>
<span style="color:#9cdcfe;">$plugins</span>[] = <span style="color:#ce9178;">'syslog'</span>;
</pre>

<h3>建立 syslog 資料庫</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">mysql</span> <span style="color:#4ec9b0;">-u root -p</span>
<span style="color:#4ec9b0;">mysql&gt;</span> <span style="color:#569cd6;">create database</span> syslog;
<span style="color:#4ec9b0;">mysql&gt;</span> <span style="color:#569cd6;">GRANT ALL ON</span> syslog.* TO cactiuser@localhost <span style="color:#569cd6;">IDENTIFIED BY</span> <span style="color:#ce9178;">'p@ssw0rd'</span>;
<span style="color:#4ec9b0;">mysql&gt;</span> <span style="color:#569cd6;">flush privileges</span>;

<span style="color:#6a9955;"># mysql –u root –p syslog < [syslog sql file]</span>
</pre>

<h3>修改 Syslog 插件設定</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># cd /usr/local/apache/htdocs/cacti/plugins/syslog</span>
<span style="color:#6a9955;"># vi config.php</span>
<span style="color:#9cdcfe;">$syslogdb_type</span>     = <span style="color:#ce9178;">'mysql'</span>;
<span style="color:#9cdcfe;">$syslogdb_default</span> = <span style="color:#ce9178;">'syslog'</span>;
<span style="color:#9cdcfe;">$syslogdb_hostname</span> = <span style="color:#ce9178;">'localhost'</span>;
<span style="color:#9cdcfe;">$syslogdb_username</span> = <span style="color:#ce9178;">'cactiuser'</span>;
<span style="color:#9cdcfe;">$syslogdb_password</span> = <span style="color:#ce9178;">'p@ssw0rd'</span>;
</pre>

<h3>syslog-ng 設定：寫入 MySQL</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:11px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /usr/local/etc/syslog-ng/syslog-ng.conf</span>
<span style="color:#6a9955;">### 20080116 David ###</span>

<span style="color:#6a9955;">### vlan16-1, vlan16-2 ###</span>
<span style="color:#569cd6;">destination</span> d_mysql { <span style="color:#569cd6;">pipe</span>(<span style="color:#ce9178;">"/var/log/mysql.pipe"</span> <span style="color:#569cd6;">template</span>(<span style="color:#ce9178;">"INSERT INTO syslog_incoming (facility, priority, date, time, host, message, seq, status) VALUES ( '$FACILITY', '$PRIORITY', '$YEAR-$MONTH-$DAY', '$HOUR:$MIN:$SEC', '$HOST', '$MSG', '$SEQ', '$STATUS' );\n"</span>) <span style="color:#569cd6;">template-escape</span>(yes));};
<span style="color:#569cd6;">destination</span> f_local5 { <span style="color:#569cd6;">file</span>(<span style="color:#ce9178;">"/var/log/vlan16_switch.log"</span>); };
<span style="color:#569cd6;">filter</span> f_local5 { <span style="color:#569cd6;">facility</span>(local5); };
<span style="color:#569cd6;">log</span> { <span style="color:#569cd6;">source</span>(src); <span style="color:#569cd6;">filter</span>(f_local5); <span style="color:#569cd6;">destination</span>(f_local5); <span style="color:#569cd6;">destination</span>(d_mysql); };

<span style="color:#6a9955;">### 4503-1, 4503-2 ###</span>
<span style="color:#569cd6;">destination</span> f_local6 { <span style="color:#569cd6;">file</span>(<span style="color:#ce9178;">"/var/log/vlan57_switch.log"</span>); };
<span style="color:#569cd6;">filter</span> f_local6 { <span style="color:#569cd6;">facility</span>(local6); };
<span style="color:#569cd6;">log</span> { <span style="color:#569cd6;">source</span>(src); <span style="color:#569cd6;">filter</span>(f_local6); <span style="color:#569cd6;">destination</span>(f_local6); <span style="color:#569cd6;">destination</span>(d_mysql); };
</pre>

<h3>建立 Syslog to MySQL 轉換腳本</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /root/bin/syslog2mysql.sh</span>
<span style="color:#6a9955;">================= syslog2mysql.sh =================</span>
<span style="color:#4ec9b0;">#!/bin/sh</span>

<span style="color:#569cd6;">if</span> [ <span style="color:#4ec9b0;">!</span> <span style="color:#4ec9b0;">-e</span> /var/log/mysql.pipe ]; <span style="color:#569cd6;">then</span>
  <span style="color:#569cd6;">mkfifo</span> /var/log/mysql.pipe
<span style="color:#569cd6;">fi</span>

<span style="color:#569cd6;">while</span> [ <span style="color:#4ec9b0;">-e</span> /var/log/mysql.pipe ]; <span style="color:#569cd6;">do</span>
  /usr/local/mysql/bin/mysql <span style="color:#4ec9b0;">-u cactiuser</span> <span style="color:#4ec9b0;">--password</span>=p@ssw0rd syslog &lt; /var/log/mysql.pipe &gt;/dev/null
<span style="color:#569cd6;">done</span>
<span style="color:#6a9955;">================= syslog2mysql.sh =================</span>
</pre>

<h3>加入 Crontab（開機自動啟動 + 每分鐘處理）</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/crontab</span>
<span style="color:#6a9955;">### cacti syslog ###</span>
<span style="color:#9cdcfe;">@reboot</span> root /root/bin/syslog2mysql.sh
<span style="color:#b5cea8;">*/1</span> * * * * root /usr/local/bin/php <span style="color:#4ec9b0;">-q</span> /usr/local/share/cacti2/plugins/syslog/syslog_process.php
</pre>

<hr/>

<h3>syslog_incoming 資料表結構</h3>

<table style="width:100%;max-width:700px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:13px;box-shadow:0 1px 3px rgba(0,0,0,0.08);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">欄位</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">型別</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;">facility</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">varchar(10)</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">設施（local5/local6...）</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;">priority</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">varchar(10)</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">優先等級</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;">date / time</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">date / time</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">時間戳記</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;">host</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">varchar(128)</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">來源主機</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;">message</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">text</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">訊息內容</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;">seq</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">int(10) unsigned AUTO_INCREMENT</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">序號（PK）</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;">status</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">tinyint(4) DEFAULT 0</td>
      <td style="padding:10px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">處理狀態</td>
    </tr>
  </tbody>
</table>

<hr/>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;"><strong>✅</strong> 從原始碼編譯 Cacti 的完整流程記錄完畢，涵蓋 Apache + PHP + MySQL + RRDTool + SNMP 的串接，以及 Plugin 架構與 Syslog 整合。</p>
  <p style="margin:8px 0 0 0;color:#15803d;">> 歡迎大家參考，若有錯誤請不吝指教，謝謝囉。</p>
</div>