---
title: CentOS 自動化安裝：PXE 網路開機實戰
date: 2008-12-29
categories: [伺服器管理]
tags: [CentOS, PXE, Kickstart]
---

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;font-weight:600;">📌 文章性質：此為企業內部 Linux 自動化安裝技術文，介紹如何透過 PXE 網路開機與 Kickstart 工具進行 CentOS 的批次自動部署，屬於標準 IT 系統管理技術，僅供學習與內部使用。</p>
</div>

<h2>PXE + Kickstart 安裝實戰心得</h2>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;font-weight:600;">🚀 今天安裝並測試 PXE + kickstart，之前只有成功過 kickstart 的部分，終於成功結合了 PXE + kickstart + tools 介面！棒棒啦～高興～^^</p>
</div>

<hr/>

<h2>DHCP 設定 /etc/dhcpd.conf</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/dhcpd.conf</span>
<span style="color:#6a9955;">==================================================</span>
<span style="color:#569cd6;">ddns-update-style</span> interim;
<span style="color:#569cd6;">ignore client-updates</span>;

<span style="color:#569cd6;">subnet</span> <span style="color:#b5cea8;">192.168.0.0</span> <span style="color:#569cd6;">netmask</span> <span style="color:#b5cea8;">255.255.255.0</span> {

  <span style="color:#6a9955;"># --- default gateway</span>
  <span style="color:#569cd6;">option routers</span> <span style="color:#b5cea8;">192.168.0.1</span>;
  <span style="color:#569cd6;">option subnet-mask</span> <span style="color:#b5cea8;">255.255.255.0</span>;

  <span style="color:#6a9955;">#option nis-domain "xxx.com.tw";</span>
  <span style="color:#6a9955;">#option domain-name "xxx.com.tw";</span>
  <span style="color:#6a9955;">#option domain-name-servers 192.168.1.1;</span>

  <span style="color:#569cd6;">option time-offset</span> <span style="color:#b5cea8;">-18000</span>; <span style="color:#6a9955;"># Eastern Standard Time</span>
  <span style="color:#569cd6;">option ntp-servers</span> <span style="color:#b5cea8;">xxx.xxx.xxx.100</span>;

  <span style="color:#569cd6;">range dynamic-bootp</span> <span style="color:#b5cea8;">192.168.0.2 192.168.0.254</span>;
  <span style="color:#569cd6;">default-lease-time</span> <span style="color:#b5cea8;">21600</span>;
  <span style="color:#569cd6;">max-lease-time</span> <span style="color:#b5cea8;">43200</span>;

  <span style="color:#6a9955;"># we want the nameserver to appear at a fixed address</span>
  <span style="color:#6a9955;">#host ns {</span>
  <span style="color:#6a9955;"># next-server marvin.redhat.com;</span>
  <span style="color:#6a9955;"># hardware ethernet 12:34:56:78:AB:CD;</span>
  <span style="color:#6a9955;"># fixed-address 207.175.42.254;</span>
  <span style="color:#6a9955;">#}</span>
}

<span style="color:#6a9955;"># PXE</span>
<span style="color:#569cd6;">allow booting</span>;
<span style="color:#569cd6;">allow bootp</span>;
<span style="color:#569cd6;">option option-128</span> <span style="color:#569cd6;">code</span> <span style="color:#b5cea8;">128</span> = <span style="color:#ce9178;">string</span>;
<span style="color:#569cd6;">option option-129</span> <span style="color:#569cd6;">code</span> <span style="color:#b5cea8;">129</span> = <span style="color:#ce9178;">text</span>;
<span style="color:#569cd6;">next-server</span> <span style="color:#b5cea8;">192.168.0.1</span>; <span style="color:#6a9955;"># PXE Server 的 IP</span>
<span style="color:#569cd6;">filename</span> <span style="color:#ce9178;">"/pxelinux.0"</span>;
</pre>

<hr/>

<h2>NFS 匯出設定 /etc/exports</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/exports</span>
<span style="color:#6a9955;">==================================================</span>
<span style="color:#9cdcfe;">/exports/clonezilla</span> <span style="color:#b5cea8;">192.168.0.0/24</span>(<span style="color:#4ec9b0;">ro,sync,root_squash</span>)
</pre>

<hr/>

<h2>主機存取控制</h2>

<h3>/etc/hosts.allow</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/hosts.allow</span>
<span style="color:#6a9955;">==================================================</span>
<span style="color:#9cdcfe;">sshd:</span> <span style="color:#b5cea8;">xxx.xxx.xxx.xxx</span>
<span style="color:#9cdcfe;">httpd:</span> <span style="color:#b5cea8;">xxx.xxx.xxx.xxx,192.168.0.</span>
<span style="color:#9cdcfe;">dhcpd:</span> <span style="color:#b5cea8;">192.168.0.</span>
<span style="color:#9cdcfe;">mountd :</span> <span style="color:#b5cea8;">192.168.0.</span>
</pre>

<h3>/etc/hosts.deny</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /etc/hosts.deny</span>
<span style="color:#6a9955;">==================================================</span>
<span style="color:#9cdcfe;">sshd:</span> <span style="color:#b5cea8;">all</span>
<span style="color:#9cdcfe;">httpd:</span> <span style="color:#b5cea8;">all</span>
<span style="color:#9cdcfe;">dhcpd:</span> <span style="color:#b5cea8;">all</span>
<span style="color:#9cdcfe;">mountd :</span> <span style="color:#b5cea8;">all</span>
<span style="color:#6a9955;">#all:all</span>
</pre>

<hr/>

<h2>PXE 選單設定</h2>

<h3>/tftpboot/pxelinux.cfg/default（主選單）</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /tftpboot/pxelinux.cfg/default</span>
<span style="color:#6a9955;">==================================================</span>
<span style="color:#569cd6;">default</span> menu.c32
<span style="color:#569cd6;">prompt</span> <span style="color:#b5cea8;">0</span>
<span style="color:#569cd6;">timeout</span> <span style="color:#b5cea8;">300</span>
<span style="color:#569cd6;">ONTIMEOUT</span> local

<span style="color:#569cd6;">MENU TITLE</span> PXE Main Menu

<span style="color:#569cd6;">LABEL</span> KS Auto Install - CentOS 5 x86 With KS eth0
<span style="color:#569cd6;">MENU LABEL</span> KS Auto Install2 - CentOS 5 x86 NO KS eth0
<span style="color:#569cd6;">KERNEL</span> images/centos/i386/5.2/vmlinuz
<span style="color:#569cd6;">APPEND</span> ks=<span style="color:#ce9178;">http://192.168.0.1/KSCFG/ks.CentOS5.x86.cfg</span> initrd=images/centos/i386/5.2/initrd.img ramdisk_size=<span style="color:#b5cea8;">100000</span> text ksdevice=eth0

<span style="color:#569cd6;">LABEL</span> Install CentOS 5
<span style="color:#569cd6;">MENU LABEL</span> Install CentOS 5
<span style="color:#569cd6;">KERNEL</span> images/vmlinuz
<span style="color:#569cd6;">APPEND</span> initrd=images/initrd.img ramdisk_size=<span style="color:#b5cea8;">100000</span>

<span style="color:#569cd6;">LABEL</span> x86 Servers
<span style="color:#569cd6;">MENU LABEL</span> x86 Servers
<span style="color:#569cd6;">KERNEL</span> menu.c32
<span style="color:#569cd6;">APPEND</span> pxelinux.cfg/x86_Servers

<span style="color:#569cd6;">LABEL</span> x86_64 Servers
<span style="color:#569cd6;">MENU LABEL</span> x86_64 Servers
<span style="color:#569cd6;">KERNEL</span> menu.c32
<span style="color:#569cd6;">APPEND</span> pxelinux.cfg/x86_64_Servers

<span style="color:#569cd6;">LABEL</span> Tools
<span style="color:#569cd6;">MENU LABEL</span> Tools
<span style="color:#569cd6;">KERNEL</span> menu.c32
<span style="color:#569cd6;">APPEND</span> pxelinux.cfg/tools

<span style="color:#569cd6;">LABEL</span> local
<span style="color:#569cd6;">MENU LABEL</span> Boot local hard drive
<span style="color:#569cd6;">LOCALBOOT</span> <span style="color:#b5cea8;">0</span>
</pre>

<h3>/tftpboot/pxelinux.cfg/clonezilla</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /tftpboot/pxelinux.cfg/clonezilla</span>
<span style="color:#6a9955;">==================================================</span>
<span style="color:#569cd6;">MENU DEFAULT</span>
<span style="color:#569cd6;">MENU LABEL</span> Clonezilla live

<span style="color:#569cd6;">LABEL</span> clonezilla
<span style="color:#569cd6;">KERNEL</span> images/clonezilla/vmlinuz
<span style="color:#6a9955;"># Older Clonezilla</span>
<span style="color:#6a9955;"># append initrd=images/clonezilla/initrd.gz boot=casper netboot nfsroot=$NFSSERVER:$NFSEXPORT</span>
<span style="color:#6a9955;"># Clonezilla 1.1.0-8</span>
<span style="color:#6a9955;">#append initrd=images/clonezilla/initrd.gz boot=live union=aufs netboot=nfs nfsroot=$NFSSERVER:$NFSEXPORT</span>
<span style="color:#569cd6;">append</span> initrd=images/clonezilla/initrd.gz boot=live union=aufs netboot=nfs nfsroot=<span style="color:#b5cea8;">192.168.0.1</span>:/exports/clonezilla
</pre>

<h3>/tftpboot/pxelinux.cfg/x86_Servers</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /tftpboot/pxelinux.cfg/x86_Servers</span>
<span style="color:#6a9955;">==================================================</span>
<span style="color:#569cd6;">MENU TITLE</span> x86 Server Menu

<span style="color:#569cd6;">LABEL</span> PXE Main Menu
<span style="color:#569cd6;">MENU LABEL</span> PXE Main Menu
<span style="color:#569cd6;">KERNEL</span> menu.c32
<span style="color:#569cd6;">APPEND</span> pxelinux.cfg/default

<span style="color:#569cd6;">LABEL</span> KS Auto Install - CentOS 5 x86 + KS + eth0
<span style="color:#569cd6;">MENU LABEL</span> KS Auto Install2 - CentOS 5 x86 NO KS eth0
<span style="color:#569cd6;">KERNEL</span> images/centos/i386/5.2/vmlinuz
<span style="color:#569cd6;">APPEND</span> ks=<span style="color:#ce9178;">http://192.168.0.1/KSCFG/ks.CentOS5.x86.cfg</span> initrd=images/centos/i386/5.2/initrd.img ramdisk_size=<span style="color:#b5cea8;">100000</span> text ksdevice=eth0

<span style="color:#569cd6;">LABEL</span> Manual Install - CentOS 5 x86 + eth0
<span style="color:#569cd6;">MENU LABEL</span> Manual Install - CentOS 5 x86 NO KS eth0
<span style="color:#569cd6;">KERNEL</span> images/centos/i386/5.2/vmlinuz
<span style="color:#569cd6;">APPEND</span> ks initrd=images/centos/i386/5.2/initrd.img ramdisk_size=<span style="color:#b5cea8;">100000</span> ksdevice=eth0 ip=dhcp url --url <span style="color:#ce9178;">http://192.168.0.1/CentOS5/</span>
</pre>

<h3>/tftpboot/pxelinux.cfg/x86_64_Servers</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># vi /tftpboot/pxelinux.cfg/x86_64_Servers</span>
<span style="color:#6a9955;">==================================================</span>
<span style="color:#569cd6;">MENU TITLE</span> x86_64 Server Menu

<span style="color:#569cd6;">LABEL</span> PXE Main Menu
<span style="color:#569cd6;">MENU LABEL</span> PXE Main Menu
<span style="color:#569cd6;">KERNEL</span> menu.c32
<span style="color:#569cd6;">APPEND</span> pxelinux.cfg/default

<span style="color:#569cd6;">LABEL</span> KS Auto Install - CentOS 5 x86_64 + KS + eth0
<span style="color:#569cd6;">MENU LABEL</span> KS Auto Install2 - CentOS 5 x86_64 NO KS eth0
<span style="color:#569cd6;">KERNEL</span> images/centos/x86_86/5.2/vmlinuz
<span style="color:#569cd6;">APPEND</span> ks=<span style="color:#ce9178;">http://192.168.0.1/KSCFG/ks.CentOS5.x86_64.cfg</span> initrd=images/centos/x86_86/5.2/initrd.img ramdisk_size=<span style="color:#b5cea8;">100000</span> text ksdevice=eth0

<span style="color:#569cd6;">LABEL</span> Manual Install - CentOS 5 x86_64 NO KS eth0
<span style="color:#569cd6;">MENU LABEL</span> Manual Install - CentOS 5 x86_64 NO KS eth0
<span style="color:#569cd6;">KERNEL</span> images/centos/x86_86/5.2/vmlinuz
<span style="color:#569cd6;">APPEND</span> ks initrd=images/centos/x86_86/5.2/initrd.img ramdisk_size=<span style="color:#b5cea8;">100000</span> ksdevice=eth0 ip=dhcp url --url <span style="color:#ce9178;">http://192.168.0.1/CentOS5_64/</span>
</pre>

<hr/>

<h2>工具下載</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">wget</span> <span style="color:#ce9178;">http://downloads.sourceforge.net/partedmagic/pmagic-pxe-3.4.zip</span>
<span style="color:#569cd6;">wget</span> <span style="color:#ce9178;">http://downloads.sourceforge.net/clonezilla/clonezilla-live-1.2.1-23.iso</span>
<span style="color:#569cd6;">wget</span> <span style="color:#ce9178;">http://s93616405.onlinehome.us/bootdisk/622c.zip</span>
</pre>

<hr/>

<h2>附錄：PXE + Clonezilla 完整設定（NFS + TFTP）</h2>

<p>轉貼自 CentOS wiki，的好文章轉貼留存。</p>

<h3>NFS Server 設定</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># Setup directories</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /mnt/isoimage
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /exports/clonezilla

<span style="color:#6a9955;"># Download the clonezilla-live-$LATESTVERSION.iso to /tmp</span>
<span style="color:#6a9955;"># Mount the iso image and copy contents to the export directory</span>
<span style="color:#569cd6;">mount</span> <span style="color:#4ec9b0;">-o loop</span> /tmp/clonezilla-live-$LATESTVERSION.iso /mnt/isoimage
<span style="color:#569cd6;">cp</span> <span style="color:#4ec9b0;">-a</span> /mnt/isoimage/. /exports/clonezilla
<span style="color:#569cd6;">umount</span> /mnt/isoimage

<span style="color:#6a9955;"># Restart NFS</span>
<span style="color:#569cd6;">add</span> /exports/clonezilla *(ro,sync) /etc/exports
<span style="color:#569cd6;">service nfs restart</span>
<span style="color:#569cd6;">exportfs</span> <span style="color:#4ec9b0;">-ra</span>
</pre>

<h3>TFTP Server 設定</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># Setup directories</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tmp/clonezilla
<span style="color:#569cd6;">mkdir</span> /mnt/isoimage
<span style="color:#569cd6;">mkdir</span> /tftpboot/images/clonezilla/

<span style="color:#6a9955;"># Mount the Clonezilla iso image and copy the boot files to the tftp server directory</span>
<span style="color:#569cd6;">mount</span> <span style="color:#4ec9b0;">-o loop</span> clonezilla-live-$LATESTVERSION.iso /mnt/isoimage
<span style="color:#569cd6;">cp</span> /mnt/isoimage/casper/initrd1.img /tftpboot/images/clonezilla/initrd.gz
<span style="color:#569cd6;">cp</span> /mnt/isoimage/casper/vmlinuz1 /tftpboot/images/clonezilla/vmlinuz

<span style="color:#569cd6;">umount</span> /mnt/isoimage
</pre>

<hr/>

<h2>附錄：Parted Magic PXE 設定</h2>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;">💡 <strong>前置需求：</strong>已具備 PXE 伺服器、DHCP 伺服器，以及 Parted Magic PXE 版本。</p>
</div>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># mkdir -p /tftpboot/images/pmagic</span>
<span style="color:#6a9955;"># unzip the partedmagic-pxe-$VERSION.zip to /tmp</span>
<span style="color:#6a9955;"># cp the contents on the pmagic directory to /tftpboot/images/pmagic</span>

<span style="color:#6a9955;"># Add the following lines to the pxelinux-configuration-file</span>
<span style="color:#569cd6;">default</span> pmagic

<span style="color:#569cd6;">label</span> pmagic
<span style="color:#569cd6;">kernel</span> images/pmagic/bzImage
<span style="color:#569cd6;">append</span> noapic initrd=images/pmagic/initrd.gz root=/dev/ram0 init=/linuxrc ramdisk_size=<span style="color:#b5cea8;">100000</span>
</pre>

<hr/>

<h2>附錄：PXE CentOS Rescue Mode</h2>

<div style="background:#eff6ff;border-left:4px solid #3b82f6;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#1e40af;">💡 <strong>前置需求：</strong>已具備 PXE 伺服器與 DHCP 伺服器。</p>
</div>

<h3>環境準備</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># mkdir -p /tftpboot/images/centos/4.4/</span>
<span style="color:#6a9955;"># Copy vmlinuz and initrd.img from /images/pxeboot/ directory on "disc 1" to /tftpboot/images/centos/4.4/</span>
</pre>

<h3>HTTP 方式（Kickstart 設定檔）</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># Kickstart configuration file CentOS 4.4 Rescue Mode</span>
<span style="color:#569cd6;">lang</span> en_US.UTF-8
<span style="color:#569cd6;">langsupport</span> --default=en_US.UTF-8 en_US.UTF-8
<span style="color:#569cd6;">keyboard</span> us
<span style="color:#569cd6;">mouse</span> none
<span style="color:#6a9955;"># nfs --server=$yournfsserverip --dir=/directory/that/contains/disc1/CentOS/RPMS/</span>
<span style="color:#569cd6;">url</span> --url <span style="color:#ce9178;">http://$yourwebserver/directory/that/contains/disc1/CentOS/RPMS/</span>
<span style="color:#569cd6;">network</span> --bootproto=dhcp
</pre>

<p>PXE 選單項目：</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">LABEL</span> CentOS 4.4 Rescue via HTTP
<span style="color:#569cd6;">KERNEL</span> images/centos/4.4/vmlinuz
<span style="color:#569cd6;">MENU LABEL</span> ^CentOS 4.4 Rescue via HTTP
<span style="color:#569cd6;">APPEND</span> initrd=images/centos/4.4/initrd.img ramdisk_size=<span style="color:#b5cea8;">10000</span> text rescue ks=<span style="color:#ce9178;">http://$yourwebserver/path/to/your/ks.cfg</span>
</pre>

<h3>NFS 方式（Kickstart 設定檔）</h3>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># Kickstart configuration file CentOS 4.4 Rescue Mode</span>
<span style="color:#569cd6;">lang</span> en_US.UTF-8
<span style="color:#569cd6;">langsupport</span> --default=en_US.UTF-8 en_US.UTF-8
<span style="color:#569cd6;">keyboard</span> us
<span style="color:#569cd6;">mouse</span> none
<span style="color:#569cd6;">nfs</span> --server=$yournfsserverip --dir=/directory/that/contains/disc1/CentOS/RPMS/
<span style="color:#6a9955;"># url --url http://$yourwebserver/directory/that/contains/disc1/CentOS/RPMS/</span>
<span style="color:#569cd6;">network</span> --bootproto=dhcp
</pre>

<p>PXE 選單項目：</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#569cd6;">LABEL</span> CentOS 4.4 Rescue via NFS
<span style="color:#569cd6;">KERNEL</span> images/centos/4.4/vmlinuz
<span style="color:#569cd6;">MENU LABEL</span> ^CentOS 4.4 Rescue via NFS
<span style="color:#569cd6;">APPEND</span> initrd=images/centos/4.4/initrd.img ramdisk_size=<span style="color:#b5cea8;">10000</span> text rescue ks=nfs:$yournfsserverip:/path/to/your/ks.cfg
</pre>

<hr/>

<h2>PXE Server 完整架設流程</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
<span style="color:#6a9955;"># 安裝所需套件</span>
<span style="color:#569cd6;">yum install</span> tftp-server

<span style="color:#6a9955;"># vi /etc/xinetd.d/tftp → disable = 'no'</span>
<span style="color:#6a9955;"># restart xinetd</span>
<span style="color:#569cd6;">service xinetd restart</span>

<span style="color:#6a9955;"># 安裝 syslinux</span>
<span style="color:#569cd6;">yum install</span> syslinux

<span style="color:#6a9955;"># 複製所需檔案到 tftpboot 目錄</span>
<span style="color:#569cd6;">cp</span> /usr/lib/syslinux/pxelinux.0 /tftpboot
<span style="color:#569cd6;">cp</span> /usr/lib/syslinux/menu.c32 /tftpboot
<span style="color:#569cd6;">cp</span> /usr/lib/syslinux/memdisk /tftpboot
<span style="color:#569cd6;">cp</span> /usr/lib/syslinux/mboot.c32 /tftpboot
<span style="color:#569cd6;">cp</span> /usr/lib/syslinux/chain.c32 /tftpboot

<span style="color:#6a9955;"># 建立 PXE 選單目錄</span>
<span style="color:#569cd6;">mkdir</span> /tftpboot/pxelinux.cfg

<span style="color:#6a9955;"># 建立 CentOS 各版本目錄結構</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/i386/<span style="color:#b5cea8;">3.0</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/i386/<span style="color:#b5cea8;">3.1</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/x86_64/<span style="color:#b5cea8;">3.0</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/x86_64/<span style="color:#b5cea8;">3.1</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/i386/<span style="color:#b5cea8;">4.0</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/i386/<span style="color:#b5cea8;">4.1</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/x86_64/<span style="color:#b5cea8;">4.0</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/x86_64/<span style="color:#b5cea8;">4.1</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/i386/<span style="color:#b5cea8;">5.0</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/i386/<span style="color:#b5cea8;">5.1</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/x86_64/<span style="color:#b5cea8;">5.0</span>
<span style="color:#569cd6;">mkdir</span> <span style="color:#4ec9b0;">-p</span> /tftpboot/images/centos/x86_64/<span style="color:#b5cea8;">5.1</span>

<span style="color:#6a9955;"># 複製各版本 vmlinuz + initrd.img（每個 Release + ARCH 都要做）</span>
<span style="color:#6a9955;"># cp vmlinuz initrd.img from /images/pxeboot/ (disc 1) to /tftpboot/images/centos/$ARCH/$RELEASE</span>

<span style="color:#6a9955;"># DHCP 設定加入 PXE 參數（xxx.xxx.xxx.xxx 為 PXE Server IP）</span>
<span style="color:#569cd6;">allow booting</span>;
<span style="color:#569cd6;">allow bootp</span>;
<span style="color:#569cd6;">option option-128</span> <span style="color:#569cd6;">code</span> <span style="color:#b5cea8;">128</span> = <span style="color:#ce9178;">string</span>;
<span style="color:#569cd6;">option option-129</span> <span style="color:#569cd6;">code</span> <span style="color:#b5cea8;">129</span> = <span style="color:#ce9178;">text</span>;
<span style="color:#569cd6;">next-server</span> <span style="color:#b5cea8;">xxx.xxx.xxx.xxx</span>;
<span style="color:#569cd6;">filename</span> <span style="color:#ce9178;">"/pxelinux.0"</span>;

<span style="color:#6a9955;"># 重新啟動 DHCP</span>
<span style="color:#569cd6;">service dhcpd restart</span>
</pre>

<hr/>

<h3>工具支援總表</h3>

<table style="width:100%;max-width:900px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.08);">
  <caption style="text-align:left;padding:0 0 8px 4px;font-weight:600;color:#374151;font-size:15px;">PXE 工具與對應作業系統</caption>
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">工具</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">用途</th>
      <th style="border:1px solid #e5e7eb;color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Clonezilla</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#1e40af;">系統複製 / 還原</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#1e40af;">NFS + PXE 開機，支援完整硬碟或分割區複製</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Parted Magic</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#1e40af;">磁碟分割 / 救援</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#1e40af;">包含 GParted、磁碟工具，PXE 开機無需光碟</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">CentOS Rescue</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#1e40af;">系統救援模式</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#1e40af;">可選 HTTP 或 NFS 方式取得救援系統</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Kickstart</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#1e40af;">自動化安裝</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#1e40af;">結合 HTTP/NFS，自動完成 CentOS 安裝設定</td>
    </tr>
  </tbody>
</table>

<hr/>

<h3>PXE + Kickstart 流程圖</h3>

<div style="background:#f8fafc;border:1px solid #e2e8f0;border-radius:8px;padding:20px;margin:16px 0;text-align:center;font-family:monospace;font-size:13px;line-height:2;color:#374151;">
  <strong style="color:#1e40af;">用戶端開機</strong> → <strong style="color:#059669;">DHCP 取得 IP</strong> → <strong style="color:#d97706;">TFTP 下載 pxelinux.0</strong> → <strong style="color:#7c3aed;">載入選單</strong><br>
  ↓<br>
  選擇 <code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">KS Auto Install</code><br>
  ↓<br>
  <strong style="color:#2563eb;">HTTP/NFS 下載 ks.cfg</strong> + <strong style="color:#dc2626;">CentOS 安裝程式</strong><br>
  ↓<br>
  <strong style="color:#059669;">Kickstart 自動完成安裝設定</strong>
</div>

<hr/>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:14px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;"><strong>💜 小結：</strong>PXE + Kickstart 結合可以大幅簡化大量伺服器的安裝流程，搭配 Clonezilla、Parted Magic 等工具還能延伸做系統複製、救援與還原，是管理多台 Linux 伺服器必備的技能！</p>
</div>