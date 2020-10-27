# ubuntu

# CheatSheet Ubuntu 18.04

## 0x01 基础设置

## 0x02 软件安装

    sudo apt-get --purge autoremove

第一步，更新源：

    # deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricteddeb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added by software-propertiesdeb http://mirrors.aliyun.com/ubuntu/ xenial main restricteddeb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe #Added by software-propertiesdeb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricteddeb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-propertiesdeb http://mirrors.aliyun.com/ubuntu/ xenial universedeb http://mirrors.aliyun.com/ubuntu/ xenial-updates universedeb http://mirrors.aliyun.com/ubuntu/ xenial multiversedeb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiversedeb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiversedeb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-propertiesdeb http://archive.canonical.com/ubuntu xenial partnerdeb-src http://archive.canonical.com/ubuntu xenial partnerdeb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricteddeb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-propertiesdeb http://mirrors.aliyun.com/ubuntu/ xenial-security universedeb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse

    # 更换源 sudo apt-get updatesudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-devsudo apt-get install zsh tree htopsudo apt-get install build-essential acl ntp htop git libpq-dev libmysqlclient-dev libffi-dev libfreetype6-dev libjpeg8-dev liblcms2-dev libtiff5-dev libwebp-dev libxml2-dev libxslt1-dev tcl8.6-dev tk8.6-dev zlib1g-dev python-dev python-pip python-pycurl python-tk ipython supervisor python3.5 python3.5-dev python3-pip python3-lxml python3-tk ipython3sudo apt-get install mysql-server mysql-client libmysqlclient-dev slurm# GIT 配置 git config --global color.ui truegit config --global user.name "twocucao"git config --global user.email "twocucao@gmail.com"ssh-keygen -t rsa -b 4096 -C "twocucao@gmail.com"

### 2.1 设置无登录密钥

    # 刚开始用了一个很蠢的方法 scp ~/.ssh/id_rsa.pub twocucao@192.168.2.156:.ssh/id_rsa.pubssh twocucao@192.168.2.156 "mkdir .ssh;chmod 0700 .ssh"# 现在想想，可以直接 ssh-copy-idssh-copy-id twocucao@192.168.2.156

http://askubuntu.com/questions/46930/how-can-i-set-up-password-less-ssh-login

    # 服务器 sudo apt-get install openssh-serversudo vi /etc/ssh/sshd_config # 找到 PermitRootLogin no 一行，改为 PermitRootLogin yessudo service ssh restartsudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-devsudo adduser deploysudo adduser deploy sudosu deploy# 开发机复制 ssh 公钥。# 可以用下面的命令，汗，之前都是在服务器上面创建.ssh 文件夹，然后在本地 scp 拷贝过去，现在想想这个方法还是挺笨的。# 就像这样 scp ~/.ssh/id_rsa.pub deploy@192.168.1.143:/webapps/xxxapp/.ssh/authorized_keys# 其实这个命令就 OK 了。ssh-copy-id deploy@IPADDRESS# 服务器 sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7sudo apt-get install -y apt-transport-https ca-certificatessudo apt-get install -y nginx-extrassudo service nginx start

## 0x02 了解 Linux 服务器运行情况

### 如何查看占用 TCP/UDP 端口

    sudo lsof -i -P -n | grep LISTEN
    sudo netstat -tulpn | grep LISTEN
    sudo lsof -i:22 ## see a specific port such as 22 ##
    sudo nmap -sTU -O IP-address-Here

    # 运行时间 uptime
    # 内存情况 free -h
    # 网络类## 实时流量监控 iftop## 进程占用带宽 nethogs## sudo nethogs eth0iptraf# 磁盘类 iotop## 当 dstat 的 wai 字段值比较大时，可以使用 iotop 找出哪些进程出了问题# 综合类 之 监控进程，进程管理 tophtopglances # PS , 这个监控粒度更细# 综合类 可以取代 vmstat , iostat , netstat , ifstatdstat# 综合类# 约等于 strace + tcpdump + htop + iftop + lsofsysdig

### DNS

    dig ns baidu.com

## 0x04 踩坑集合

### 3.1 磁盘问题

    df -h 查看磁盘块占用的文件（block）
    df -i 查看索引节点的占用（Inodes）
    find / -size +100M |xargs ls -lh
    # 删除 5 天前的文件
    find /path/to/files* -mtime +5 -exec rm {} \;
    du -h
    rm xxx.log
    echo "" > xxx.log
