+++
title = "Vagrant 小技巧"
date = 2018-08-22T21:26:19+08:00
readingTime = true
categories = ["DevOps"]
tags = ["vagrant"]
toc = true
+++

断断续续的用着 Vagrant，本地开发依旧有着不可比拟的优势，POC，demo，hacking 等等。

<!--more-->

## plugins

✦ _vagrant-cachier_
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

✦ _vagrant-hostsupdater_
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

## vagrant file 模版

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["LC_ALL"] = "en_US.UTF-8"

# ---- 虚机配置 ----
N = 1

nodes = [
  {
    :node => "node0",
    :box => "centos/7",
    :cpu => 1,
    :mem => 1024
  }
]

# ---- 各种控制变量 ----
box = "centos/7"
custom_cpu_mem = "yes"
enable_custom_boxes = "yes"
enable_port_forwards = "yes"
linked_clones = "no"
server_cpus = 1
server_memory = 512
additional_nics = "yes"       # yes | no
additional_nics_dhcp = "yes"
additional_nics_num = 1
subnet = "192.168.202."
subnet_ip_start = 200
ansible_groups = {
  "test-nodes" => [
    "node[0:#{N-1}"
  ]
}
port_forwards = [
  {
    :node => "node0",
    :guest => 80,
    :host => 8080
  },
  {
    :node => "node0",
    :guest => 8000,
    :host => 8000
  }
]
provision_nodes = "yes"
synced_folder == "yes"

Vagrant.configure(2) do |config|
  (1..N).each do |node_id|
    nid = (node_id - 1)
	config.vm.define "node#{nid}" do |node|
	  ### 指定 vagrant box
      if enable_custom_boxes == "yes"
        box_set = "no"  #Initially set to no so it can be set to true if found in custom box defined
        nodes.each do |cust_box|
          if cust_box[:node] == "node#{nid}"
            node.vm.box = cust_box[:box]
            box_set = "yes"
          end
        end
        if box_set == "no"
          node.vm.box = box
        end
      end
	  if enable_custom_boxes == "no"
        node.vm.box = box
      end
	  node.vm.provider "virtualbox" do |vb|
        if linked_clones == "yes"
          vb.linked_clone = true
		end
		### 指定 cpu 数
        if custom_cpu_mem == "no"
          vb.customize ["modifyvm", :id, "--cpus", server_cpus]
          vb.customize ["modifyvm", :id, "--memory", server_memory]
		end
		### 指定内存
        if custom_cpu_mem == "yes"
          nodes.each do |cust_node|
            if cust_node[:node] == "node#{nid}"
              vb.customize ["modifyvm", :id, "--cpus", cust_node[:cpu]]
              vb.customize ["modifyvm", :id, "--memory", cust_node[:mem]]
            end
          end
		end
		### 指定 desktop
        if desktop == "yes"
          vb.gui = true
          vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxvga"]
          vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
          vb.customize ["modifyvm", :id, "--ioapic", "on"]
          vb.customize ["modifyvm", :id, "--vram", "128"]
          vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
        end
      end
      node.vm.hostname = "node#{nid}"
      ### 指定网卡
      if additional_nics == "yes"
        if additional_nics_dhcp == "no"
          (1..additional_nics_num).each do |nic_num|
            nnum = Random.rand(0..50)
            node.vm.network :private_network, ip: subnet+"#{subnet_ip_start + nid + nnum}"
          end
        end
        if additional_nics_dhcp == "yes"
          (1..additional_nics_num).each do |nic_num|
            node.vm.network :private_network, type: "dhcp"
          end
        end
      end
	  ### 指定端口
	  if enable_port_forwards == "yes"
        port_forwards.each do |pf|
          if pf[:node] == "node#{nid}"
            node.vm.network "forwarded_port", guest: pf[:guest], host: pf[:host] + nid
          end
        end
	  end
	  ### 指定 ansible 初始化
      if provision_nodes == "yes"
        if node_id == N
          node.vm.provision "ansible" do |ansible|  #runs bootstrap Ansible playbook
            ansible.limit = "all"
            ansible.playbook = "bootstrap.yml"
          end
          node.vm.provision "ansible" do |ansible|  #runs Ansible playbook for installing roles/executing tasks
            ansible.limit = "all"
            ansible.playbook = "playbook.yml"
            ansible.groups = ansible_groups
          end
        end
      end
    end
  end
  if provision_nodes == "yes"
    config.vm.provision :shell, path: "bootstrap.sh", keep_color: "true"  #runs initial shell script
  end
  # 读写共享目录
  if synced_folder == "yes"
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=774,fmode=775"]
  end
end
```

## swap

可以通过增加 swap 的方式增加虚机的内存，以免耗尽主机的内存，下面这段代码就是做这件事的：

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

