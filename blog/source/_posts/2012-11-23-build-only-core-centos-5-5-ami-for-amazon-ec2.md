---
title: Build only Core CentOS 5.5 AMI for Amazon EC2
date: 2012-11-23
categories: 伺服器管理
tags: [CentOS, AWS, EC2]
---

<h2>在 EC2 上构建 CentOS 5.5 自定义 AMI</h2>

<p>本篇文章說明如何將本機虛擬機（VirtualBox / VMware）或 EC2 instance-store 實例轉換為 EBS-backed AMI。</p>

<hr />

<h2>前置需求</h2>

<ul>
  <li>一套已安裝的 <strong>CentOS</strong>（自己的 VM、VBox 或 EC2 instance 皆可）</li>
  <li>已安裝 <strong>rpm-python</strong> 及 <strong>Java</strong></li>
  <li>設定好 Script 前面的參數部分</li>
</ul>

<h3>Script 功能說明</h3>

<ul>
  <li>自動抓取及安裝 <code>ec2-api-tools.zip</code>、<code>ec2-ami-tools.zip</code></li>
  <li>自動上傳至所設定的 <code>S3_REGION</code> 之 <code>S3_BUCKET_NAME</code></li>
</ul>

<hr />

<h2>將 Instance-Store 轉換為 EBS AMI（VirtualBox / VMware 適用）</h2>

<ol>
  <li><strong>建立 10G EBS Volume</strong> → <code>vol-xxxxxxxx</code></li>
  <li><strong>Attach Volume 到 Instance</strong> → <code>/dev/sdf</code>
    <ul>
      <li>注意：較新的 Linux kernel 可能會將設備名稱重新命名為 <code>/dev/xvdf</code> ～ <code>/dev/xvdp</code></li>
    </ul>
  </li>
  <li><strong>確認是否認到該 Volume</strong></li>
  <li><strong>格式化為 ext3</strong></li>
  <li><strong>建立掛載目錄</strong></li>
</ol>

<pre style="background: rgb(30, 30, 30); border-radius: 8px; color: #e6e6e6; font-family: Consolas, Monaco, monospace; font-size: 14px; line-height: 1.6; margin: 16px 0px; overflow-x: auto; padding: 16px 20px;"><span style="color: #569cd6;">fdisk</span> <span style="color: #9cdcfe;">/dev/sdf</span>
<span style="color: #569cd6;">mkfs.ext3</span> <span style="color: #9cdcfe;">/dev/sdf</span>
<span style="color: #569cd6;">mkdir</span> <span style="color: #9cdcfe;">/mnt/target</span>
</pre>

<h3>（如有需要事後修改 image）</h3>

<pre style="background: rgb(30, 30, 30); border-radius: 8px; color: #e6e6e6; font-family: Consolas, Monaco, monospace; font-size: 14px; line-height: 1.6; margin: 16px 0px; overflow-x: auto; padding: 16px 20px;"><span style="color: #569cd6;">mount</span> <span style="color: #9cdcfe;">/dev/sdf</span> <span style="color: #9cdcfe;">/mnt/target</span>
<span style="color: #569cd6;">rsync</span> <span style="color: #4ec9b0;">-avHx</span> <span style="color: #9cdcfe;">/</span> <span style="color: #9cdcfe;">/mnt/target</span>
<span style="color: #569cd6;">rsync</span> <span style="color: #4ec9b0;">-avHx</span> <span style="color: #9cdcfe;">/dev</span> <span style="color: #9cdcfe;">/mnt/target</span>
<span style="color: #569cd6;">sync</span>;<span style="color: #569cd6;">sync</span>;<span style="color: #569cd6;">sync</span>;<span style="color: #569cd6;">sync</span> <span style="color: #567d46;">&amp;&amp; umount /mnt/target</span>
</pre>

<h3>從 Volume 建立 Snapshot</h3>

<p>（實測為無須 Detach Volume，亦可直接做 Snapshots）</p>

<pre style="background: rgb(30, 30, 30); border-radius: 8px; color: #e6e6e6; font-family: Consolas, Monaco, monospace; font-size: 14px; line-height: 1.6; margin: 16px 0px; overflow-x: auto; padding: 16px 20px;"><span style="color: #4ec9b0;">Volumes → Snapshots:</span> <span style="color: #ce9178;">vol-xxxxxxxx</span> <span style="color: #4ec9b0;">→</span> <span style="color: #ce9178;">CentOS5.5 EBS (Snapshots)</span>
</pre>

<h3>從 Snapshot 建立 AMI 並調整大小</h3>

<ol>
  <li>Create Image from Snapshot → <strong>CentOS 5.5 (Root Volume) resize to 500G</strong></li>
  <li>Launch Instance</li>
  <li>進入 Instance 後執行：</li>
</ol>

<pre style="background: rgb(30, 30, 30); border-radius: 8px; color: #e6e6e6; font-family: Consolas, Monaco, monospace; font-size: 14px; line-height: 1.6; margin: 16px 0px; overflow-x: auto; padding: 16px 20px;"><span style="color: #567d46;"># resize2fs /dev/sda1    # 費時，視空間大小而定</span>
</pre>

<hr />

<h2>Script 文件：EC2_CentOS5.5_AMI.sh</h2>

<pre style="background: rgb(30, 30, 30); border-radius: 8px; color: #e6e6e6; font-family: Consolas, Monaco, monospace; font-size: 13px; line-height: 1.5; margin: 16px 0px; overflow-x: auto; padding: 16px 20px;"><span style="color: #567d46;">#!/bin/bash</span>

<span style="color: #569cd6;">echo</span> <span style="color: #ce9178;">-e "\n *** Build only Core CentOS 5.5 AMI for Amazon EC2 *** \n"</span>

<span style="color: #567d46;"># yum install rpm-python</span>
<span style="color: #569cd6;">echo</span> <span style="color: #ce9178;">-e "匯入RPM-GPG-KEY-centos4."</span>
<span style="color: #569cd6;">rpm</span> <span style="color: #4ec9b0;">--import</span> <span style="color: #9cdcfe;">http://vault.centos.org/RPM-GPG-KEY-centos4</span>

<span style="color: #567d46;">#============================================================#</span>
<span style="color: #567d46;"># 需先設定好Script的參數部分</span>
<span style="color: #567d46;">#============================================================#</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">AMI_NAME</span>=<span style="color: #ce9178;">"centos-5.5-x86_64-core"</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">AMI_IMG</span>=<span style="color: #ce9178;">"${AMI_NAME}.img"</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">MNT_DIR</span>=<span style="color: #ce9178;">"fs-${AMI_NAME}"</span>

<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">JAVA_HOME</span>=<span style="color: #9cdcfe;">/usr</span>   <span style="color: #567d46;"># for rpm</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">EC2_CERT</span>=<span style="color: #ce9178;">"/root/.aws/x509-cert-xxxx.pem"</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">EC2_PRIVATE_KEY</span>=<span style="color: #ce9178;">"/root/.aws/x509-pk-xxxx.pem"</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">ACCOUNT_NUM</span>=<span style="color: #ce9178;">"xxxxxxxxxxxx"</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">S3_BUCKET_NAME</span>=<span style="color: #ce9178;">"centos-5.5-david"</span>
<span style="color: #567d46;"># S3_BUCKET_NAME 需使用小寫，方不會出現錯誤提示!!!</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">S3_REGION</span>=<span style="color: #ce9178;">"ap-southeast-1"</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">ACCESS_KEY_ID</span>=<span style="color: #ce9178;">"xxxxxxxxxxxxxxxxxxxxxxx"</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">SECRET_ACCESS_KEY</span>=<span style="color: #ce9178;">"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"</span>
<span style="color: #567d46;">#============================================================#</span>

<span style="color: #569cd6;">cd</span> <span style="color: #9cdcfe;">/mnt</span>
<span style="color: #569cd6;">dd</span> <span style="color: #4ec9b0;">if</span>=<span style="color: #9cdcfe;">/dev/zero</span> <span style="color: #4ec9b0;">of</span>=${<span style="color: #9cdcfe;">AMI_IMG</span>} <span style="color: #4ec9b0;">count</span>=<span style="color: #b5cea8;">1024</span> <span style="color: #4ec9b0;">bs</span>=<span style="color: #b5cea8;">1M</span>

<span style="color: #569cd6;">mke2fs</span> <span style="color: #4ec9b0;">-F -j</span> ${<span style="color: #9cdcfe;">AMI_IMG</span>}
<span style="color: #569cd6;">mkdir</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}
<span style="color: #569cd6;">mount</span> <span style="color: #4ec9b0;">-o loop</span> ${<span style="color: #9cdcfe;">AMI_IMG</span>} ${<span style="color: #9cdcfe;">MNT_DIR</span>}

<span style="color: #569cd6;">mkdir</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/dev</span>
<span style="color: #569cd6;">mkdir</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/etc</span>

<span style="color: #569cd6;">/sbin/MAKEDEV</span> <span style="color: #4ec9b0;">-d</span> <span style="color: #9cdcfe;">/mnt/${MNT_DIR}/dev/</span> <span style="color: #4ec9b0;">-x</span> console
<span style="color: #569cd6;">/sbin/MAKEDEV</span> <span style="color: #4ec9b0;">-d</span> <span style="color: #9cdcfe;">/mnt/${MNT_DIR}/dev/</span> <span style="color: #4ec9b0;">-x</span> null
<span style="color: #569cd6;">/sbin/MAKEDEV</span> <span style="color: #4ec9b0;">-d</span> <span style="color: #9cdcfe;">/mnt/${MNT_DIR}/dev/</span> <span style="color: #4ec9b0;">-x</span> zero

<span style="color: #569cd6;">cat</span> &gt; ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/etc/fstab</span> <span style="color: #4ec9b0;">&lt;&lt; EOS</span>
<span style="color: #ce9178;">/dev/sda1 / ext3 defaults 1 1
/dev/sda2 /mnt ext3 defaults 0 0
/dev/sda3 swap swap defaults 0 0
none /dev/pts devpts gid=5,mode=620 0 0
none /dev/shm tmpfs defaults 0 0
none /proc proc defaults 0 0
none /sys sysfs defaults 0 0</span>
<span style="color: #4ec9b0;">EOS</span>

<span style="color: #569cd6;">mkdir</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/proc</span>
<span style="color: #569cd6;">mount</span> <span style="color: #4ec9b0;">-t proc</span> none ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/proc</span>

<span style="color: #9cdcfe;">YUMCNF</span>=<span style="color: #9cdcfe;">./yum.conf</span>
<span style="color: #569cd6;">wget</span> <span style="color: #9cdcfe;">http://vault.centos.org/4.9/CentOS-Base.repo</span>

<span style="color: #569cd6;">cat</span> &gt; ${<span style="color: #9cdcfe;">YUMCNF</span>} <span style="color: #4ec9b0;">&lt;&lt; EOS</span>
<span style="color: #ce9178;">[main]
cachedir=/var/cache/yum
debuglevel=2
logfile=/var/log/yum.log
exclude=*-debuginfo
gpgcheck=0
pkgpolicy=newest
distroverpkg=redhat-release
tolerant=1
exactarch=1
obsoletes=1
plugins=1
reposdir=/dev/null
metadata_expire=1800</span>
<span style="color: #4ec9b0;">EOS</span>

<span style="color: #569cd6;">cat</span> CentOS-Base.repo &gt;&gt; ${<span style="color: #9cdcfe;">YUMCNF</span>}
<span style="color: #569cd6;">sed</span> <span style="color: #4ec9b0;">-i</span> <span style="color: #ce9178;">'s/4.9/5.5/g'</span> ${<span style="color: #9cdcfe;">YUMCNF</span>}
<span style="color: #569cd6;">sed</span> <span style="color: #4ec9b0;">-i</span> <span style="color: #ce9178;">'s/gpgcheck=1/gpgcheck=0/g'</span> ${<span style="color: #9cdcfe;">YUMCNF</span>}
<span style="color: #569cd6;">yum</span> <span style="color: #4ec9b0;">-c</span> ${<span style="color: #9cdcfe;">YUMCNF</span>} <span style="color: #4ec9b0;">--installroot</span>=<span style="color: #9cdcfe;">/mnt/${MNT_DIR}</span> <span style="color: #4ec9b0;">-y groupinstall</span> Core
<span style="color: #569cd6;">yum</span> <span style="color: #4ec9b0;">-c</span> ${<span style="color: #9cdcfe;">YUMCNF</span>} <span style="color: #4ec9b0;">--installroot</span>=<span style="color: #9cdcfe;">/mnt/${MNT_DIR}</span> <span style="color: #4ec9b0;">-y install</span> curl

<span style="color: #567d46;"># SSH 設定</span>
<span style="color: #569cd6;">sed</span> <span style="color: #4ec9b0;">-i</span> <span style="color: #ce9178;">"s/#PermitRootLogin yes/PermitRootLogin yes/g"</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/etc/ssh/sshd_config</span>
<span style="color: #569cd6;">echo</span> <span style="color: #ce9178;">"UseDNS  no"</span> &gt;&gt; ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/etc/ssh/sshd_config</span>
<span style="color: #569cd6;">echo</span> <span style="color: #ce9178;">"PermitRootLogin without-password"</span> &gt;&gt; ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/etc/ssh/sshd_config</span>

<span style="color: #567d46;"># 停用多餘的 tty</span>
<span style="color: #569cd6;">sed</span> <span style="color: #4ec9b0;">-i</span> <span style="color: #ce9178;">'s/2:2345:respawn:\/sbin\/mingetty tty2/#2:2345:respawn:\/sbin\/mingetty tty2/g'</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/etc/inittab</span>
<span style="color: #567d46;"># ... 其餘 tty3~tty6 同上</span>

<span style="color: #567d46;"># Xen 相關</span>
<span style="color: #569cd6;">echo</span> <span style="color: #ce9178;">"hwcap 0 nosegneg"</span> &gt; ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/etc/ld.so.conf.d/libc6-xen.conf</span>

<span style="color: #567d46;"># 安裝 EC2 自定義核心模組</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">MFILE</span>=<span style="color: #ce9178;">"ec2-modules-2.6.16.33-xenU-x86_64"</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">MODULE</span>=<span style="color: #ce9178;">"http://s3.amazonaws.com/ec2-downloads/${MFILE}.tgz"</span>
<span style="color: #569cd6;">wget</span> ${<span style="color: #9cdcfe;">MODULE</span>}
<span style="color: #569cd6;">gunzip</span> ${<span style="color: #9cdcfe;">MFILE</span>}.tgz
<span style="color: #569cd6;">tar</span> <span style="color: #4ec9b0;">-xvf</span> ${<span style="color: #9cdcfe;">MFILE</span>}.tar <span style="color: #4ec9b0;">-C</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}
<span style="color: #569cd6;">rm</span> <span style="color: #4ec9b0;">-f</span> ${<span style="color: #9cdcfe;">MFILE</span>}.tar

<span style="color: #567d47;"># 進入 chroot 環境</span>
<span style="color: #569cd6;">chroot</span> <span style="color: #9cdcfe;">/mnt/${MNT_DIR}</span> /bin/sh
<span style="color: #569cd6;">/sbin/depmod</span> <span style="color: #4ec9b0;">-ae</span> <span style="color: #b5cea8;">2.6.16.33-xenU</span>
<span style="color: #569cd6;">exit</span>

<span style="color: #567d47;"># 帳號與權限設定</span>
<span style="color: #569cd6;">chroot</span> <span style="color: #9cdcfe;">/mnt/${MNT_DIR}</span> /bin/sh
<span style="color: #569cd6;">chkconfig</span> <span style="color: #4ec9b0;">--level 345</span> sshd on
<span style="color: #569cd6;">pwconv</span>
<span style="color: #569cd6;">echo</span> <span style="color: #ce9178;">'root:'</span> | <span style="color: #569cd6;">chpasswd</span>
<span style="color: #569cd6;">useradd</span> david; <span style="color: #569cd6;">echo</span> <span style="color: #ce9178;">'david:davidpw'</span> | <span style="color: #569cd6;">chpasswd</span>
<span style="color: #569cd6;">echo</span> <span style="color: #ce9178;">'david ALL=(ALL) NOPASSWD: ALL'</span> &gt;&gt; /etc/sudoers
<span style="color: #569cd6;">exit</span>

<span style="color: #569cd6;">umount</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}<span style="color: #9cdcfe;">/proc/</span>
<span style="color: #569cd6;">umount</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}
<span style="color: #569cd6;">rmdir</span> ${<span style="color: #9cdcfe;">MNT_DIR</span>}

<span style="color: #567d47;"># 安裝 EC2 工具</span>
<span style="color: #569cd6;">wget</span> <span style="color: #ce9178;">"http://...ec2-api-tools.zip"</span>
<span style="color: #569cd6;">wget</span> <span style="color: #9cdcfe;">http://s3.amazonaws.com/ec2-downloads/ec2-ami-tools.zip</span>
<span style="color: #569cd6;">mv</span> *.zip /opt/
<span style="color: #569cd6;">cd</span> /opt
<span style="color: #569cd6;">unzip</span> ec2-api-tools.zip
<span style="color: #569cd6;">unzip</span> ec2-ami-tools.zip
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">EC2_HOME</span>=<span style="color: #9cdcfe;">/opt/ec2-api-tools-1.6.5.1</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">EC2_AMITOOL_HOME</span>=<span style="color: #9cdcfe;">/opt/ec2-ami-tools-1.4.0.7</span>
<span style="color: #569cd6;">export</span> <span style="color: #9cdcfe;">PATH</span>=<span style="color: #9cdcfe;">$EC2_HOME/bin:$EC2_AMITOOL_HOME/bin:$PATH</span>

<span style="color: #567d47;"># 打包並上傳至 S3</span>
<span style="color: #569cd6;">cd</span> /mnt
<span style="color: #569cd6;">ec2-bundle-image</span> <span style="color: #4ec9b0;">-i</span> ${<span style="color: #9cdcfe;">AMI_IMG</span>} <span style="color: #4ec9b0;">-c</span> ${<span style="color: #9cdcfe;">EC2_CERT</span>} <span style="color: #4ec9b0;">-k</span> ${<span style="color: #9cdcfe;">EC2_PRIVATE_KEY</span>} <span style="color: #4ec9b0;">-u</span> ${<span style="color: #9cdcfe;">ACCOUNT_NUM</span>} <span style="color: #4ec9b0;">-r</span> x86_64
<span style="color: #569cd6;">ec2-upload-bundle</span> <span style="color: #4ec9b0;">-b</span> ${<span style="color: #9cdcfe;">S3_BUCKET_NAME</span>} <span style="color: #4ec9b0;">-m</span> <span style="color: #9cdcfe;">/tmp/${AMI_IMG}.manifest.xml</span> <span style="color: #4ec9b0;">-a</span> ${<span style="color: #9cdcfe;">ACCESS_KEY_ID</span>} <span style="color: #4ec9b0;">-s</span> ${<span style="color: #9cdcfe;">SECRET_ACCESS_KEY</span>} <span style="color: #4ec9b0;">--location</span> ${<span style="color: #9cdcfe;">S3_REGION</span>}
<span style="color: #569cd6;">ec2-register</span> <span style="color: #4ec9b0;">--region</span> ${<span style="color: #9cdcfe;">S3_REGION</span>} <span style="color: #4ec9b0;">-C</span> ${<span style="color: #9cdcfe;">EC2_CERT</span>} <span style="color: #4ec9b0;">-K</span> ${<span style="color: #9cdcfe;">EC2_PRIVATE_KEY</span>} <span style="color: #4ec9b0;">-n</span> ${<span style="color: #9cdcfe;">AMI_NAME</span>} ${<span style="color: #9cdcfe;">S3_BUCKET_NAME</span>}<span style="color: #9cdcfe;">/${AMI_IMG}.manifest.xml</span>
</pre>

<hr />


<h3>重要參數備註</h3>

<table style="width:100%;max-width:700px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background: rgb(51, 65, 85);">
      <th style="border: 1px solid rgb(71, 85, 105); color: #f1f5f9; padding: 10px 16px; text-align: left;">參數</th>
      <th style="border: 1px solid rgb(71, 85, 105); color: #f1f5f9; padding: 10px 16px; text-align: left;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">S3_BUCKET_NAME</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;"><strong>需使用小寫</strong>，否則出現錯誤：<em style="color:#dc2626;">The specified bucket is not S3 v2 safe</em></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">S3_REGION</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">可選：<code style="background:#eff6ff;padding:2px 6px;border-radius:4px;color:#1d4ed8;">EU</code>, <code style="background:#eff6ff;padding:2px 6px;border-radius:4px;color:#1d4ed8;">US</code>, <code style="background:#eff6ff;padding:2px 6px;border-radius:4px;color:#1d4ed8;">us-west-1</code>, <code style="background:#eff6ff;padding:2px 6px;border-radius:4px;color:#1d4ed8;">us-west-2</code>, <code style="background:#eff6ff;padding:2px 6px;border-radius:4px;color:#1d4ed8;">ap-southeast-1</code>, <code style="background:#eff6ff;padding:2px 6px;border-radius:4px;color:#1d4ed8;">ap-northeast-1</code>, <code style="background:#eff6ff;padding:2px 6px;border-radius:4px;color:#1d4ed8;">sa-east-1</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">resize2fs</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;color:#374151;">Launch 後執行，<strong>費時</strong>，視空間大小而定</td>
    </tr>
  </tbody>
</table>