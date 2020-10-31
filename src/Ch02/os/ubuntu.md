# CheatSheet Ubuntu 20.04

## 0x01 基础设置

## 0x02 软件安装

```
sudo apt-get --purge autoremove
```

```
# 更新
sudo apt update
sudo apt install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-devsudo apt-get install zsh tree htopsudo apt-get install build-essential acl ntp htop git libpq-dev libmysqlclient-dev libffi-dev libfreetype6-dev libjpeg8-dev liblcms2-dev libtiff5-dev libwebp-dev libxml2-dev libxslt1-dev tcl8.6-dev tk8.6-dev zlib1g-dev python-dev python-pip python-pycurl python-tk ipython supervisor python3.5 python3.5-dev python3-pip python3-lxml python3-tk ipython3sudo apt-get install mysql-server mysql-client libmysqlclient-dev slurm

# GIT 配置
git config --global color.ui true
git config --global user.name "twocucao"
git config --global user.email "twocucao@gmail.com"
ssh-keygen -t rsa -b 4096 -C "twocucao@gmail.com"
```

第一步，更新源：

### 2.1 设置无登录密钥

```
# 刚开始用了一个很蠢的方法
scp ~/.ssh/id_rsa.pub twocucao@192.168.2.156:.ssh/id_rsa.pub
ssh twocucao@192.168.2.156 "mkdir .ssh;chmod 0700 .ssh"
# 现在想想，可以直接 ssh-copy-id
ssh-copy-id twocucao@192.168.2.156
```

## 0x02 了解 Linux 服务器运行情况

```
# 运行时间 uptime
# 内存情况 free -h
# 网络类
## 实时流量监控 iftop
## 进程占用带宽 nethogs
## sudo nethogs eth0
iptraf
# 磁盘类 iotop
## 当 dstat 的 wai 字段值比较大时，可以使用 iotop 找出哪些进程出了问题
# 综合类 之 监控进程，进程管理
top
htop
glances # PS , 这个监控粒度更细
# 综合类 可以取代 vmstat , iostat , netstat , ifstatdstat
# 综合类
# 约等于 strace + tcpdump + htop + iftop + lsofsysdig
```

## 0x04 踩坑集合

### 3.1 磁盘问题

```
df -h 查看磁盘块占用的文件（block）
df -i 查看索引节点的占用（Inodes）
find / -size +100M |xargs ls -lh
# 删除 5 天前的文件
find /path/to/files* -mtime +5 -exec rm {} \;
du -h
rm xxx.log
echo "" > xxx.log
```

