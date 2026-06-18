---
title: 修改penk的SliTaz.tw LiveCD.....嘿嘿
date: 2008-11-27
---

<h2>自製 SliTaz 作業用 LiveCD 實戰心得</h2>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;"><strong>🚀</strong> 最近因為工作需求，需要製作一片作業用的 LiveCD，花了三天來研究並測試，終於玩出一點心得，分享給大家囉！</p>
</div>

<hr/>

<h2>需求說明</h2>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> 需求很明確：遠端連線、遠端傳輸、圖形化 SCP、以及一套完整的文書處理軟體。</p>
</div>

<ul style="color:#374151;line-height:2;">
  <li><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">tsclient</code> + <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">rdesktop</code> — Windows 遠端桌面連線</li>
  <li><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">ssh</code> + <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">scp</code> — 安全的遠端指令與檔案傳輸</li>
  <li><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">gftp</code> — SCP 圖形化工具</li>
  <li><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">openoffice</code> — 文書處理套件</li>
  <li><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">中文輸入</code> — 中文環境必備</li>
</ul>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>⚠️</strong> 參考了 <a href="http://penkia.blogspot.com/2008/04/slitaztw.html" style="color:#c2410c;text-decoration:underline;">SliTaz.tw - 全世界最小的中文桌面環境</a>，原本想自己作中文化，不過看到前輩 penk 已做過，加上中文化後字體沒有 penk 版本清楚，就懶得再玩了...><</p>
  <p style="margin:8px 0 0 0;color:#9a3412;"><strong>💬</strong> 從以前玩 RedHat 到 Gentoo，一直都覺得中文化 GUI 是台灣人使用 Linux 的一大障礙。</p>
</div>

<hr/>

<h2>研究過程</h2>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;"><strong>🔍</strong> 先是收集一堆 LiveCD 的資訊及測試，發現符合需求的只有兩個版本，而 SliTaz 是最終選擇。</p>
</div>

<div style="background:#f9fafb;border-left:4px solid #a855f7;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#7e22ce;"><strong>💜</strong> 意外發現：SliTaz 竟然也可以玩 e17！在 HD 裝起來後很快很方便，不過要改進 LiveCD 的部分，等有空再來慢慢玩囉...^^||</p>
</div>

<hr/>

<h2>OpenOffice 安裝方式</h2>

<h3>方案一：裝進 rootfs（測試成功但有缺點）</h3>

<p>將 OpenOffice 2.4 裝進 rootfs 中，<strong>測試成功，但開機時 loading 到記憶體的時間過久</strong>。</p>

<h3>方案二：放進 rootcd（推薦 ✓）</h3>

<p>把 OpenOffice 2.4 放進 rootcd 中，開機時自動 mount cdrom，並寫好 <code>openoffice.desktop</code> 的捷徑。<strong>開機跟原本一樣快，又可以直接使用 OpenOffice！</strong></p>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>⚠️</strong> 本來想裝 OpenOffice 3，但測試失敗。<code>openoffice.org</code> 也沒有提供原本 2.4 的 package 方式之套件，故先直接使用 2.4 裝進來。</p>
</div>

<hr/>

<h2>系統環境變數與開機判斷方式</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># 先察看原本環境變數，及 /etc/init.d/ 下的 script 之判斷方式</span>
<span style="color:#569cd6;">cat</span> /etc/locale.conf
<span style="color:#6a9955;">===========================================</span>
<span style="color:#9cdcfe;">LANG</span>=<span style="color:#ce9178;">zh_TW.UTF-8</span>
<span style="color:#9cdcfe;">LC_ALL</span>=<span style="color:#ce9178;">zh_TW.UTF-8</span>

<span style="color:#569cd6;">cat</span> /etc/kmap.conf
<span style="color:#6a9955;">===========================================</span>
<span style="color:#9cdcfe;">LANG</span>=<span style="color:#ce9178;">zh_TW.UTF-8</span>
<span style="color:#9cdcfe;">KMAP</span>=<span style="color:#ce9178;">us.kmap</span>

<span style="color:#569cd6;">vi</span> /etc/rcS.conf
<span style="color:#6a9955;">===========================================</span>
<span style="color:#9cdcfe;">RUN_SCRIPTS</span>=<span style="color:#ce9178;">"bootopts.sh network.sh i18n.sh hwconf.sh local.sh"</span>

<span style="color:#569cd6;">find</span> /etc -name bootopts.sh
<span style="color:#6a9955;">/etc/init.d/bootopts.sh</span>

<span style="color:#569cd6;">cat</span> /proc/cmdline
<span style="color:#6a9955;">===========================================</span>
<span style="color:#ce9178;">root=/dev/hda1 lang=zh_TW.UTF-8 keymap=us</span>
</pre>

<hr/>

<h2>所需套件列表（土法煉鋼）</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># 列出需要的套件相依性</span>
<span style="color:#569cd6;">tazpkg</span> list-files dropbear
<span style="color:#569cd6;">tazpkg</span> list-files tsclient
<span style="color:#569cd6;">tazpkg</span> list-files rdesktop
<span style="color:#569cd6;">tazpkg</span> list-files libcrypto
<span style="color:#569cd6;">tazpkg</span> list-files tcpdump
<span style="color:#569cd6;">tazpkg</span> list-files nmap
<span style="color:#569cd6;">tazpkg</span> list-files openssl
</pre>

<h3>相依檔案清單</h3>

<table style="width:100%;max-width:900px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:12px;box-shadow:0 1px 3px rgba(0,0,0,0.08);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:8px 12px;text-align:left;font-weight:600;">分類</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:8px 12px;text-align:left;font-weight:600;">檔案路徑</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;white-space:nowrap;"><strong>Dropbear (SSH)</strong></td>
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;font-size:11px;"><code>/etc/dropbear/*</code> <code>/usr/bin/{dbclient,scp,ssh}</code> <code>/usr/sbin/dropbear*</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;white-space:nowrap;"><strong>tsclient</strong></td>
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;font-size:11px;"><code>/usr/bin/tsclient</code> <code>/usr/lib/tsclient/tsclient-applet</code> <code>/usr/share/{pixmaps/tsclient*,applications/tsclient.desktop}</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;white-space:nowrap;"><strong>rdesktop</strong></td>
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;font-size:11px;"><code>/usr/bin/rdesktop</code> <code>/usr/share/rdesktop/keymaps/*</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;white-space:nowrap;"><strong>OpenSSL</strong></td>
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;font-size:11px;"><code>/usr/lib/libcrypto.so*</code> <code>/usr/lib/libssl.so*</code> <code>/usr/bin/{openssl,c_rehash}</code> <code>/etc/ssl/*</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;white-space:nowrap;"><strong>Network Tools</strong></td>
      <td style="padding:8px 12px;border-bottom:1px solid #e5e7eb;color:#374151;font-family:monospace;font-size:11px;"><code>/usr/sbin/tcpdump</code> <code>/usr/bin/nmap</code> <code>/usr/share/nmap/*</code></td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>複製套件到 rootfs</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/dbclient usr/bin/dbclient
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/scp usr/bin/scp
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/ssh usr/bin/ssh
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /etc/dropbear etc/dropbear
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /etc/dropbear/banner etc/dropbear/banner
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /etc/init.d/dropbear etc/init.d/dropbear
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/sbin/dropbear usr/sbin/dropbear
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/dropbearconvert usr/bin/dropbearconvert
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/dropbearkey usr/bin/dropbearkey

<span style="color:#569cd6;">mkdir</span> usr/lib/tsclient
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/lib/tsclient/tsclient-applet usr/lib/tsclient/tsclient-applet
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/share/pixmaps/tsclient.png usr/share/pixmaps/tsclient.png
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-ra</span> /usr/share/pixmaps/tsclient usr/share/pixmaps/
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/share/locale/fr/LC_MESSAGES/tsclient.mo usr/share/locale/fr/LC_MESSAGES/tsclient.mo
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/share/applications/tsclient.desktop usr/share/applications/tsclient.desktop
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/tsclient usr/bin/tsclient
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-ar</span> /usr/share/rdesktop usr/share/

<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/rdesktop usr/bin/rdesktop
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/lib/libcrypto.so usr/lib/libcrypto.so
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/lib/libcrypto.so.0.9.8 usr/lib/libcrypto.so.0.9.8

<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/sbin/tcpdump usr/sbin/tcpdump
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-ar</span> /usr/share/nmap usr/share/
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/nmap usr/bin/nmap

<span style="color:#569cd6;">mkdir</span> etc/ssl
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-ar</span> /etc/ssl/* etc/ssl/
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/openssl usr/bin/openssl
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/bin/c_rehash usr/bin/c_rehash
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /usr/lib/libssl.so* usr/lib/
</pre>

<hr/>

<h2>修改 isolinux 選單設定</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># leafpad /home/slitaz/hacked/rootcd/boot/isolinux/isolinux.cfg</span>
<span style="color:#6a9955;">=====================================================================</span>
<span style="color:#569cd6;">display</span> isolinux.msg
<span style="color:#569cd6;">default</span> slitaz

<span style="color:#569cd6;">label</span> slitaz
<span style="color:#569cd6;">kernel</span> /boot/bzImage
<span style="color:#569cd6;">append</span> initrd=/boot/rootfs.gz rw root=/dev/null vga=normal sound=no lang=zh_TW.UTF-8 keymap=us screen=<span style="color:#b5cea8;">1024x768x24</span>

<span style="color:#569cd6;">label</span> be     <span style="color:#6a9955;">config be.cfg</span>
<span style="color:#569cd6;">label</span> ca     <span style="color:#6a9955;">config ca.cfg</span>
<span style="color:#569cd6;">label</span> ch     <span style="color:#6a9955;">config fr_CH.cfg</span>
<span style="color:#569cd6;">label</span> de     <span style="color:#6a9955;">config de_CH.cfg</span>
<span style="color:#569cd6;">label</span> en     <span style="color:#6a9955;">config en.cfg</span>
<span style="color:#569cd6;">label</span> es     <span style="color:#6a9955;">config es.cfg</span>
<span style="color:#569cd6;">label</span> fr     <span style="color:#6a9955;">config fr.cfg</span>
<span style="color:#569cd6;">label</span> it     <span style="color:#6a9955;">config it.cfg</span>
<span style="color:#569cd6;">label</span> reboot <span style="color:#569cd6;">com32</span> reboot.c32

<span style="color:#569cd6;">implicit</span> <span style="color:#b5cea8;">0</span>
<span style="color:#569cd6;">prompt</span> <span style="color:#b5cea8;">1</span>
<span style="color:#569cd6;">timeout</span> <span style="color:#b5cea8;">80</span>

<span style="color:#569cd6;">F1</span> help.txt
<span style="color:#569cd6;">F2</span> options.txt
<span style="color:#569cd6;">F3</span> isolinux.msg
<span style="color:#569cd6;">F4</span> display.txt
</pre>

<hr/>

<h2>中文化設定</h2>

<h3>locale.conf</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># leafpad /home/slitaz/hacked/rootfs/etc/locale.conf</span>
<span style="color:#6a9955;">-------------------------------------------</span>
<span style="color:#9cdcfe;">LANG</span>=<span style="color:#ce9178;">zh_TW.UTF-8</span>
<span style="color:#9cdcfe;">LC_ALL</span>=<span style="color:#ce9178;">zh_TW.UTF-8</span>
</pre>

<h3>kmap.conf</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># leafpad /home/slitaz/hacked/rootfs/etc/kmap.conf</span>
<span style="color:#6a9955;">-------------------------------------------</span>
<span style="color:#9cdcfe;">LANG</span>=<span style="color:#ce9178;">zh_TW.UTF-8</span>
<span style="color:#9cdcfe;">KMAP</span>=<span style="color:#ce9178;">us.kmap</span>
</pre>

<hr/>

<h2>設定開機自動掛載光碟</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># leafpad /home/slitaz/hacked/rootfs/etc/init.d/local.sh</span>
<span style="color:#6a9955;">=====================================================================</span>
<span style="color:#569cd6;">mount</span> <span style="color:#4ec9b0;">-t iso9660</span> /dev/cdrom /media/cdrom
</pre>

<hr/>

<h2>建立 OpenOffice 捷徑</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># leafpad /home/slitaz/hacked/rootfs/usr/share/applications/openoffice.desktop</span>
<span style="color:#6a9955;">-------------------------------------------</span>
<span style="color:#4ec9b0;">[Desktop Entry]</span>
<span style="color:#9cdcfe;">Encoding</span>=<span style="color:#ce9178;">UTF-8</span>
<span style="color:#9cdcfe;">Name</span>=<span style="color:#ce9178;">OPENOFFICE</span>
<span style="color:#9cdcfe;">Exec</span>=<span style="color:#ce9178;">/media/cdrom/openoffice.org/program/soffice.bin</span>
<span style="color:#9cdcfe;">Terminal</span>=<span style="color:#ce9178;">false</span>
<span style="color:#9cdcfe;">Type</span>=<span style="color:#ce9178;">Application</span>
<span style="color:#9cdcfe;">Categories</span>=<span style="color:#ce9178;">Office;</span>
</pre>

<hr/>

<h2>編譯 OpenOffice RPM 並放進 rootcd</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> ~
<span style="color:#569cd6;">wget</span> <span style="color:#ce9178;">OOo_2.4.0_LinuxIntel_install_zh-tw.tar.gz</span>
<span style="color:#569cd6;">tar</span> <span style="color:#4ec9b0;">zxvf</span> OOo_2.4.0_LinuxIntel_install_zh-tw.tar.gz
<span style="color:#569cd6;">cd</span> OOH680_m12_native_packed-1_zh-TW.9286/RPMS/

<span style="color:#6a9955;"># 將每個 RPM 解開</span>
<span style="color:#569cd6;">for</span> i <span style="color:#569cd6;">in</span> *.rpm ; <span style="color:#569cd6;">do</span>
  <span style="color:#569cd6;">rpm2cpio</span> $i | <span style="color:#569cd6;">cpio</span> <span style="color:#4ec9b0;">-id</span>
<span style="color:#569cd6;">done</span>

<span style="color:#6a9955;"># 移到 rootcd 中（su - 後執行）</span>
<span style="color:#569cd6;">su</span> -
<span style="color:#569cd6;">mv</span> /home/hacker/OOH680_m12_native_packed-1_zh-TW.9286/RPMS/opt/openoffice.org2.4 openoffice.org
</pre>

<hr/>

<h2>封裝 rootfs.gz</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">cd</span> /home/slitaz/hacked/rootfs
<span style="color:#569cd6;">find</span> . -print | <span style="color:#569cd6;">cpio</span> <span style="color:#4ec9b0;">-o -H newc</span> | <span style="color:#569cd6;">lzma</span> e <span style="color:#4ec9b0;">-si -so</span> > ../rootfs.gz

<span style="color:#569cd6;">cd</span> ..
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> rootfs.gz rootcd/boot

<span style="color:#569cd6;">genisoimage</span> <span style="color:#4ec9b0;">-R</span> <span style="color:#4ec9b0;">-o</span> slitaz-hacked.iso <span style="color:#4ec9b0;">-b</span> boot/isolinux/isolinux.bin \
  <span style="color:#4ec9b0;">-c</span> boot/isolinux/boot.cat <span style="color:#4ec9b0;">--no-emul-boot</span> <span style="color:#4ec9b0;">--boot-load-size 4</span> \
  <span style="color:#4ec9b0;">-V</span> <span style="color:#ce9178;">"SliTaz-Hacked"</span> <span style="color:#4ec9b0;">--input-charset iso8859-1</span> <span style="color:#4ec9b0;">--boot-info-table</span> rootcd
</pre>

<hr/>

<h2>一鍵生成腳本</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># leafpad /home/slitaz/hacked/gen_hacked_iso.sh</span>
<span style="color:#6a9955;">-------------------------------------------</span>
<span style="color:#4ec9b0;">#!/bin/sh</span>
<span style="color:#6a9955;"># Gen a new hacked ISO image.</span>
<span style="color:#6a9955;">#</span>
<span style="color:#569cd6;">echo</span> <span style="color:#ce9178;">"Rebuilding the image of the compressed system .... Please Wait!"</span>

<span style="color:#569cd6;">cd</span> /home/slitaz/hacked/rootfs
<span style="color:#569cd6;">find</span> . -print | <span style="color:#569cd6;">cpio</span> <span style="color:#4ec9b0;">-o -H newc</span> | <span style="color:#569cd6;">lzma</span> e <span style="color:#4ec9b0;">-si -so</span> > ../rootfs.gz

<span style="color:#569cd6;">sleep</span> <span style="color:#b5cea8;">1</span>
<span style="color:#569cd6;">cd</span> ..
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> rootfs.gz rootcd/boot
<span style="color:#569cd6;">sleep</span> <span style="color:#b5cea8;">1</span>

<span style="color:#569cd6;">echo</span> <span style="color:#ce9178;">"Generate a bootable ISO image .... Please Wait!"</span>
<span style="color:#9cdcfe;">ISO_NAME</span>=<span style="color:#ce9178;">"slitaz-hacked.iso"</span>
<span style="color:#9cdcfe;">ROOTCD</span>=<span style="color:#ce9178;">"rootcd"</span>

<span style="color:#569cd6;">genisoimage</span> <span style="color:#4ec9b0;">-R</span> <span style="color:#4ec9b0;">-o</span> <span style="color:#9cdcfe;">$ISO_NAME</span> <span style="color:#4ec9b0;">-b</span> boot/isolinux/isolinux.bin \
  <span style="color:#4ec9b0;">-c</span> boot/isolinux/boot.cat <span style="color:#4ec9b0;">--no-emul-boot</span> <span style="color:#4ec9b0;">--boot-load-size 4</span> \
  <span style="color:#4ec9b0;">-V</span> <span style="color:#ce9178;">"SliTaz-Hacked"</span> <span style="color:#4ec9b0;">--input-charset iso8859-1</span> <span style="color:#4ec9b0;">--boot-info-table</span> <span style="color:#9cdcfe;">$ROOTCD</span>
<span style="color:#6a9955;">-------------------------------------------</span>
<span style="color:#569cd6;">chmod</span> +x gen_hacked_iso.sh
</pre>

<hr/>

<h2>新發現：chroot 方式安裝套件</h2>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;"><strong>🎉</strong> 剛剛找到正確在 LiveCD 中安裝套件的方式了...XD 要用 <code>chroot</code> 的方式！</p>
</div>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;"><strong>💡</strong> 參考：<a href="http://www.slitaz.org/en/doc/handbook/chroot-env.html" style="color:#1e40af;text-decoration:underline;">SliTaz 官方文件 - Chroot Environment</a></p>
</div>

<div style="background:#fff7ed;border-left:4px solid #f97316;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#c2410c;"><strong>⚠️</strong> 以後還是先把文件慢慢看完，再來玩才對...=.=|| 這次土法煉鋼雖然成功，但走了不少冤枉路。</p>
</div>

<hr/>

<h3>最終成果總覽</h3>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.08);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">功能</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">工具</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">狀態</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">遠端桌面</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">tsclient + rdesktop</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#166534;"><strong>✓ 完成</strong></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">SSH 遠端連線</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">dropbear (ssh/scp/scp)</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#166534;"><strong>✓ 完成</strong></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">圖形化 SCP</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">gftp</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#166534;"><strong>✓ 完成</strong></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">文書處理</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">OpenOffice 2.4</code>（外掛光碟）</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#166534;"><strong>✓ 完成</strong></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">中文環境</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">locale + keymap</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#166534;"><strong>✓ 完成</strong></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">網路工具</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">tcpdump + nmap + openssl</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#166534;"><strong>✓ 完成</strong></td>
    </tr>
  </tbody>
</table>

<hr/>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;"><strong>💜</strong> SliTaz 是全世界最小的中文桌面環境，結合 PXE、Dropbear、OpenOffice 後，成為一套完整的遠端作業 LiveCD。建議文件還是要先看過再動手，才不會像我一樣土法煉鋼...^^||</p>
</div>