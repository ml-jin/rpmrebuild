# RPM Rebuild 过程

[TOC]

## 文章目录

## 1. rpmrebuild 安装

**rpmrebuild-2.11-3.el7.noarch.rpm 详情页**
**https://centos.pkgs.org/7/epel-x86_64/rpmrebuild-2.11-3.el7.noarch.rpm.html**

**安装步骤：**

```bash
#1. Download latest epel-release rpm from http://download-ib01.fedoraproject.org/pub/epel/7/x86_64/
wget https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-12.noarch.rpm
#2. Install epel-release rpm:
rpm -Uvh epel-release*rpm
#3. Install rpmrebuild rpm package:
yum install rpmrebuild
123456
```

## 2. rpmrebuild 提取rpm的spec文件

执行命令，这时候会打开一个spec的vim文件，我们使用vim的另存为将它保存下来(shift+: w文件名)

```
rpmrebuild -e -p hadoop_3_1_0_0_78-3.1.1.3.1.0.0-78.x86_64.rpm
```

我的另存为了hadoop_3_1_0_0_78-3.1.1.3.1.0.0-78.spec
然后将hadoop_3_1_0_0_78-3.1.1.3.1.0.0-78.spec拷贝至rpmbuild/SPECS目录等待使用

## 3. rpm2cpio 解压rpm包

解压到了当前目录下usr（这个目录就像你安装完后的安装目录一样，e.g usr/hdp/3.1.0.0-78…）
`rpm2cpio hadoop_3_1_0_0_78-3.1.1.3.1.0.0-78.x86_64.rpm |cpio -idv`

到rrpmbuild/BUILDROOT目录下，创建hadoop_3_1_0_0_78-3.1.1.3.1.0.0-78.x86_64目录
`mkdir hadoop_3_1_0_0_78-3.1.1.3.1.0.0-78.x86_64`
(至于为什么是到BUILDROOT下来建，我想可能是是因为这个目录原本是最终虚拟安装目录，而我们rpm解压开的目录就是安装目录的样子，解压出的rpm.spec文件中看一下files目录并不是SOURCE)

但是注意：BUILDROOT目录下，每次build rpm后都会清空，所以还是先把解压的源码放在SOURCE目录下，然后拷贝到BUILDROOT下吧

下边儿就是在如果要改源码就改rpmbuild/BUILDROOT/hadoop_3_1_0_0_78-3.1.1.3.1.0.0-78.x86_64/usr下就行了，然后spec有需要改就去改

## 4. rpmbuild 重新build rpm包

```bash
[hdp01@hdp01 SPECS]$ rpmbuild -bb hadoop_3_1_0_0_78-hdfs.spec 
1
```

好了相应的rpm包会重新生成