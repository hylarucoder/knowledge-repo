# Modern Shell

## 0x01 更好的替代品

### find -> fd

```bash
{}: A placeholder token that will be replaced with the path of the search result (documents/images/party.jpg).
{.}: Like {}, but without the file extension (documents/images/party).
{/}: A placeholder that will be replaced by the basename of the search result (party.jpg).
{//}: Uses the parent of the discovered path (documents/images).
{/.}: Uses the basename, with the extension removed (party).
```

```bash
# Convert all jpg files to png files:
fd -e jpg -x convert {} {.}.png

# Unpack all zip files (if no placeholder is given, the path is appended):
fd -e zip -x unzip

# Convert all flac files into opus files:
fd -e flac -x ffmpeg -i {} -c:a libopus {.}.opus

# Count the number of lines in Rust files (the command template can be terminated with ';'):
fd -x wc -l \; -e rs
```

### grep -> ripgrep

### ls -> exa

### cat -> bat

## Dev

### json_pp

### FZF

### Autojump

## Tools

### mycli

### pgcli

### convert

### k9s

### tokei

### git

### FTP

```bash
FTP Client 提交文件
#!/bin/bash 
lftp <<SCRIPT
set ftps:initial-prot ""
set ftp:ssl-force true
set ftp:ssl-protect-data true
set ssl:verify-certificate no
open <ftp://xxx.xxx.xxx.xxx:21> user ftpuser ftppass
lcd /Users/<username>/Ftps/Workspace/libs
put /Users/<username>/Ftps/Workspace/repos/xxx.jar
exit SCRIPT
```

## 0x05 网络

```
dig ns baidu.com

### 如何查看占用 TCP/UDP 端口
lsof -i -P -n | grep LISTEN
netstat -tulpn | grep LISTEN
lsof -i:22 ## see a specific port such as 22 ##
nmap -sTU -O IP-address-Here
```

## 0x07 文件浏览

## 0x08 多媒体处理

### 图片处理

```
convert {{image1.png}} {{image2.png}} {{image3.png}} -delay {{100}} {{animation.gif}}
```

### 视频处理

```bash
# 抽取 mp4 中的音频并保存为 mp3
mkdir outputs
for f in *.mp4;
    do ffmpeg -i "$f" -c:a libmp3lame "outputs/${f%.mp4}.mp3";
done
```

## 0x09 Tmux

```bash
tmux new -s you_tmux_name
tmux ls
tmux a
tmux a -t you_tmux_name
c-b + d
tmux kill-session -t you_tmux_name
# 进阶工具 tmuxp
```

