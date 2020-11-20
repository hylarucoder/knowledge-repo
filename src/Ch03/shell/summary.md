# Shell

## 0x00 前言

## 0x01 快捷键

```
- 「c-c」 : 中断当前命令。
- 「c-z」 : 当前程序暂停，bg 切换后台运行，使用 fg 可以调回
- 「tab」 : 补全
- 「tabx2」 : 补全提示
- 「c-r」 : 搜索命令行
- 「c-w」 : 同 vim
- 「c-u」 : 删除整行
- 「a-b/a-f」 : 移动一个词
- 「c-a」 : 移动至行首
- 「c-e」 : 移动至行尾
- 「c-k」 : 删除光标到行尾
- 「c-l」 : 清屏
- 「c-x,c-e」 : 用默认编辑器编辑当前命令（这样就可以把其他文本移动扔掉了。)
```

## 0x02 帮助

查找帮助

```
- man
- whatis / which / where
- tldr
```

值得一提的就是 tldr, 直接可以在上面查看命令的常规使用。实在是碉堡了。

```
> tldr ssh
Local data is older than two weeks, use --update to update it.

ssh

Secure Shell is a protocol used to securely log onto remote systems.
It can be used for logging or executing commands on a remote server.

- Connect to a remote server:
    ssh username@remote_host

- Connect to a remote server with a specific identity (private key):
    ssh -i path/to/key_file username@remote_host

- Connect to a remote server using a specific port:
    ssh username@remote_host -p 2222

- Run a command on a remote server:
    ssh remote_host command -with -flags

- SSH tunneling: Dynamic port forwarding (SOCKS proxy on localhost:9999):
    ssh -D 9999 -C username@remote_host

- SSH tunneling: Forward a specific port (localhost:9999 to slashdot.org:80) along with disabling pseudo-[t]ty allocation and executio[n] of remote commands:
    ssh -L 9999:slashdot.org:80 -N -T username@remote_host

- SSH jumping: Connect through a jumphost to a remote server (Multiple jump hops may be specified separated by comma characters):
    ssh -J username@jump_host username@remote_host

- Agent forwarding: Forward the authentication information to the remote machine (see `man ssh_config` for available options):
    ssh -A username@remote_host
```

## 0x03 macOS 用户

如果你和我一样使用的是 mac 系统

可以考虑将部分 FreeBSD 的工具换成 gnu

```
brew install autoconf bash binutils coreutils diffutils ed findutils flex gawk \
    gnu-indent gnu-sed gnu-tar gnu-which gpatch grep gzip less m4 make nano \
    screen watch wdiff wget
```

bashrc/zshrc 加上如下命令

```
if type brew &>/dev/null; then
  HOMEBREW_PREFIX=$(brew --prefix)
  # gnubin; gnuman
  for d in ${HOMEBREW_PREFIX}/opt/*/libexec/gnubin; do export PATH=$d:$PATH; done
  # I actually like that man grep gives the BSD grep man page
  #for d in ${HOMEBREW_PREFIX}/opt/*/libexec/gnuman; do export MANPATH=$d:$MANPATH; done
fi
```

经过上面一步，则基本上 find sed tar which 这些命令使用的 gnu 版本 (linux 版本）, 而非系统自带的 unix 版本了。

## 0x04 基本命令 - old fashion

```bash
文件 / 目录 mkdir / rm
查找文件 cd / cp / pwd / find
查看文件 more / less / tail / diff / cat / grep
用户权限 chown / chmod
复制 / 粘贴 / 同步 / 链接 cp / rsync / ln
挂载 mount
```

```bash
# 创建和删除
mkdir
mkdir -p a/b/c

rm
rm -rf dir/file/regex
rm *log# 等价 find ./ -name "*log" -exec rm {};

mv
cp
cp -r source_dir dest_dir
rsync --progress -a source_dir dest_dir
rsync -vr --progress you_folder_here twocucao@192.168.2.151:/Users/twocucao/Codes/# 目录切换 ls -lrt
find ./ -name "*.o" -exec rm {} \;
more
head
tail
tail -f filename
diff
chown
chmod
chown -R tuxapp source/chmod a+x myscript
ln cc ccA
ln -s cc ccTo
cat -v record.log | grep AAA | grep -v BBB | wc -l
find ./ | wc -l
```

```bash
查找文件之 find (gfind)
## Find
find . \( -name "*.txt" -o -name "*.pdf" \) -print
# 正则方式查找 txt 和 pdf
find . -regex  ".*\(\.txt|\.pdf\)$"
find . ! -name "*.txt" -print
find . -maxdepth 1 -type f
# 定制搜索
## 按照类型搜索
find . -type f -print  #只列出所有文件
find . -type d -print  #只列出所有目录
find . -type l -print  #只列出所有符号链接
## 按照时间搜索
find . -atime 7 -type f -print
# 最近第 7 天被访问过的所有文件：
find . -atime -7 -type f -print
# 最近 7 天内被访问过的所有文件：
find . -atime +7 type f -print
# 查询 7 天前被访问过的所有文件：
# w,k,M,Gfind . -type f -size +2kfind . -type f -perm 644 -print 
# 找具有可执行权限的所有文件 find . -type f -user weber -print
# 找用户 weber 所拥有的文件
# 后续动作
## 删除 find . -type f -name "*.swp" -delete
## 执行动作
find . -type f -name "*.swp" | xargs rmfind . -type f -user root -exec chown weber {} \;
## eg: copy 到另一个目录
find . -type f -mtime +10 -name "*.txt" -exec cp {} OLD \;
##  -exec ./commands.sh {} \;
# 2. 删除内部为空的文件夹# 递归删除 a/b/c
find . -type d -empty -delete
# 使用.gitkeep 进行填充
find . -type d -empty -exec touch {}/.gitkeep \;
find . -type d -empty -not -path '*/\.*' -exec touch {}/.gitkeep \;
# 不初始化.git/
# 3. 寻找 TOP 10
find . -type f -printf '%s %p\n'| sort -nr | head -10 | awk '{$1/=1024*1024;printf "%.2fMB - %s\n",$1,$2}'
# 4. 寻找文件夹 TOP 10
{}: A placeholder token that will be replaced with the path of the search result (documents/images/party.jpg).{.}: Like {}, but without the file extension (documents/images/party).{/}: A placeholder that will be replaced by the basename of the search result (party.jpg).{//}: Uses the parent of the discovered path (documents/images).{/.}: Uses the basename, with the extension removed (party).# Convert all jpg files to png files:fd -e jpg -x convert {} {.}.png# Unpack all zip files (if no placeholder is given, the path is appended):fd -e zip -x unzip# Convert all flac files into opus files:fd -e flac -x ffmpeg -i {} -c:a libopus {.}.opus# Count the number of lines in Rust files (the command template can be terminated with ';'):fd -x wc -l \; -e rs
```

```bash
# 压缩 / 解压缩
7z / 7za /7zr
tar / gzip / unzip/ unrar
# 打包
tar -cvf
# 解包
tar -xvf
# 压缩
gzip
# 解压缩
gunzip
bzip
tar 是将多个文件放在一起变成一个 tar 文件 (Tape Archiver)
gzip 是讲一个文件变成一个压缩文件
则 foo.tar.gz 指的是 先把文件转为 tar 文件，然后 gzip 之
```

### 文本篇

```bash
grep match_pattern file

	-o 只输出匹配的文本行
	-v 只输出没有匹配的文本行
	-c 统计文件中包含文本的次数
	-n 打印匹配行号
	-i 搜索时符合大小写
	-l 之打印文件名

grep "class" . -R -n # 多级目录中对文本递归搜索
grep -e "class" -e "vitural" file # 匹配多个模式
grep "test" file* -lZ| xargs -0 rm # grep 输出以、0 作为结尾符的文件名：（-z）-d 定义定界符-n 输出为多行-l {} 指定替换字符串 cat file.txt | xargs # 打印多行 cat file.txt | xargs -n 3 # 分割多行
cat file.txt | xargs -I {} ./command.sh -p {} -1-0 指定、0 为输入定界符
find source_dir/ -type f -name "*.cpp" -print0 |xargs -0 wc -l

# sort 排序

-n 按数字进行排序
-d 按字典序进行排序
-r 逆序排序
-k N 指定按照第 N 列排序
sort -nrk 1 data.txtsort -bd data // 忽略像空格之类的前导空白字符
sort unsort.txt | uniq > sorted.txt # 消除重复行
sort unsort.txt | uniq -c # 统计各行在文件中出现的次数
sort unsort.txt | uniq -d # 找出重复行# 用 tr 进行转换

# cut 按列切分文本 cut -f2,4 filename
# 截取文件的第 2 列和第 4 列 cut -f3 --complement filename #去文件除第 3 列的所有列 cut -f2 -d";" filename -d #指定定界符 cut -c1-5 file
# 打印第一到 5 个字符 cut -c-2 file 
# 打印前 2 个字符# paste 按列拼接文本 paste file1 file2 -d ","

# wc 统计行和字符的工具 wc -l file # 统计行数 wc -w file # 统计单词数 wc -c file # 统计字符数

# sed 文本替换利器 sed 's/text/replace_text/' file 
# 首处替换 sed 's/text/replace_text/g' file 
# 全局替换 sed -i 's/text/repalce_text/g' file # 替换文件 sed '/^$/d' file 
# 移除空白行 sed -i 's/twocucao/micheal/g' xx.dump.sqlsed -n 634428,887831p insert_doc_ids_new.sql > uninserted_sql.sql
```

### 用户篇

```bash
# 添加 yaweb 为 sudo 用户
usermod -aG sudo yaweb
所有用户和用户组信息保存在：/etc/passwd , /etc/group
用户
useradd -m yaweb # 创建相关账号，和用户目录 /home/yawebpasswd yawebuserdel -r yaweb # 删除
用户组
usermod -g groupName username # 变更组 usermod -G groupName username # 添加到组 usermod -aG sudo yaweb # 添加 yaweb 到 sudo 组
用户权限
chown userMark(+|-)PermissionsMark
userMark 取值： - u：用户 - g：组 - o：其它用户 - a：所有用户
PermissionsMark 取值： - r: 读 - w：写 - x：执行
chmod a+x main         对所有用户给文件 main 增加可执行权限 chmod g+w blogs        对组用户给文件 blogs 增加可写权限 chown -R weber server/
远程登录
ssh -l root 192.168.2.253
ssh-copy-id root@192.168.2.253
```

### 网络篇

```bash
/etc/hostname
/etc/hosts
netstat -a
```

### 磁盘篇

```bash
# 查看当前目录大小
du -sh
du -sh `ls` | sort
# 查看当前目录的下一级文件和子目录的磁盘容量
du -lh --max-depth=1
```

### 进程管理

```bash
ps -ef | grep twocucao ps -lu twocucao # 完整显示
ps -ajx
ps au | grep phantomjs | awk '{ print $2 }' | xargs kill -9 
top htop 
lsof -i:3306
lsof -u twocucao 
kill -9 pidnum 
# 将用户 colin115 下的所有进程名以 av_开头的进程终止
ps -u colin115 |  awk '/av_/ {print "kill -9 " $1}' | sh
# 将用户 colin115 下所有进程名中包含 HOST 的进程终止：
ps -fe | grep colin115 | grep HOST | awk '{print $2}' | xargs kill -9;
```

Systemd
创建一个 Systemd 服务

```
# /etc/systemd/system/gunicorn.service: 

[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target 

[Service]
PIDFile=/run/gunicorn/pid
User=someuser
Group=someuser
RuntimeDirectory=gunicorn
WorkingDirectory=/home/someuser/applicationroot
ExecStart=/usr/bin/gunicorn --pid /run/gunicorn/pid  
\\           --bind unix:/run/gunicorn/socket applicationname.wsgi
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID PrivateTmp=true 

[Install]
WantedBy=multi-user.target
```

### 性能监控

### 内存瓶颈

```
htop
free # 从 /proc/meminfo 读取数据
IO 瓶颈
# ubuntu 下 可以 mac 下不可以 iostat -d -x -k 1 1
如果 %iowait 的值过高，表示硬盘存在 I/O 瓶颈。
如果 %util 接近 100%，说明产生的 I/O 请求太多，I/O 系统已经满负荷，该磁盘可能存在瓶颈。
如果 svctm 比较接近 await，说明 I/O 几乎没有等待时间；
如果 await 远大于 svctm，说明 I/O 队列太长，io 响应太慢，则需要进行必要优化。
如果 avgqu-sz 比较大，也表示有大量 io 在等待。
```

## 0x06 资料推荐

- 一个关于 Linux 命令的各种奇技的网站 http://www.commandlinefu.com/commands/browse
- Linux 工具快速教程 http://linuxtools-rst.readthedocs.org/zh_CN/latest/index.html
- 一个 Awesome List, https://github.com/jaywcjlove/linux-command
- 命令行的艺术 https://github.com/jlevy/the-art-of-command-line
- man command 需要好好研读，特别是 man bash 至少要研读几遍

---
ChangeLog:
- **2015-04-18** 初始化本文
- **2018-08-28** 重修文字
- **2020-10-24** 重修文字
