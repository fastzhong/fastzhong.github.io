+++
title = "Vagrant 小技巧"
date = 2018-08-22T21:26:19+08:00
readingTime = true
categories = ["DevOps"]
tags = []
toc = false
+++

断断续续的用着 Vagrant，本地开发依旧有着不可比拟的优势，POC，demo，hacking 等等。

<!--more-->

## plugins

-   _vagrant-cachier_
    这玩意在 VM 下载包的时候（agt，yum，etc.）可以缓存，如三台 centOS 的 VM 只需下载 centOS 一次。安装：

```cmd
$ vagrant plugin install vagrant-cachier
```

Vagantfile 里加入这段：

```ruby
	if Vagrant.has_plugin?("vagrant-cachier")
		config.cache.scope = :box
	end
```

-   _vagrant-hostsupdater_
    VM 和主机里的 hosts 文件都会得到更新，这样 VM 和主机里的都可以直接通过 hostname 通讯。安装：

```cmd
$ vagrant plugin install vagrant-hostmanager
```

Vagantfile 里加入这段：

```ruby
	if Vagrant.has_plugin?("vagrant-hostmanager")
		config.hostmanager.enabled = true
		config.hostmanager.manage_host = true
		config.hostmanager.ignore_private_ip = false
		config.hostmanager.include_offline = true
	end
```

## vagrant file

```ruby
ENV["LC_ALL"] = "en_US.UTF-8"

Vagrant.configure(2) do |config|

# 停止启动时的更新检查
	config.vm.box_check_update = false

# 指定虚机的OS
	config.vm.box = "centos/7"

# 控制VM的内存和CPU
	config.vm.provider "virtualbox" do |v|
		v.memory = 2048
		v.cpus = 1
	end

# 和主机共享目录
	if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
		config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=777,fmode=777"]
	else
		config.vm.synced_folder ".", "/vagrant"
	end

# 循环多个虚机
	(1..1).each do |i|
		config.vm.define "vm#{i}" do |node|
			node.vm.hostname = "vm1"
			node.vm.network "private_network", ip: "10.10.10.#{i}0"
			node.vm.provision "shell",
      			inline: "echo hello from node vm#{i}"
		end
	end
```

## swap

可以通过增加 swap 的方式增加虚机的内存，以免耗尽主机的内存，下面这段代码就是刚这件事的：

```bash
#!/bin/sh

#create the swap space 1GB
echo "Creating 1GB swap space in /swapfile..."
fallocate -l 1G /swapfile
ls -lh /swapfile

#secure the swapfile
echo "Securing the swapfile..."
chown root:root /swapfile
chmod 0600 /swapfile
ls -lh /swapfile

#turn the swapfile on
echo "Turning the swapfile on..."
mkswap /swapfile
swapon /swapfile


echo "Verifying..."
swapon -s
grep -i --color swap /proc/meminfo

echo "Adding swap entry to /etc/fstab"
echo "swapfile none swap sw 0 0" >> /etc/fstab

echo "Result: "
cat /etc/fstab

echo " ****** We are done !!! ********"
```
