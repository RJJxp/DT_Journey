# SCP 命令
```bash
# 复制文件, 服务器到本地
scp <server_name>@<server_ip>:<server_path> <local_dir>
# 复制文件夹, 服务器到本地
scp -r <server_name>@<server_ip>:<server_dir> <local_dir>
# 复制文件, 本地到服务器
scp <local_path> <server_name>@<server_ip>:<server_dir> 
# 复制文件夹, 本地到服务器
scp -r <local_dir> <server_name>@<server_ip>:<server_dir>
# 指定服务器端口
scp -P <port_id> <local_path> <server_name>@<server_ip>:<server_dir>
```

# ls 命令

```bash
# 查看当前目录下文件数量, 不包含子目录中文件
ls -l | grep "^-"| wc -l
# 查看当前目录下文件数量, 包含子目录文件
ls -lR | grep "^-"| wc -l
# 查看当前目录下文件夹数量, 不包含子目录中文件
ls -l | grep "^d"| wc -l
# 查看当前目录下文件夹数量, 包含子目录中文件
ls -lR | grep "^d"| wc -l
```

`^d`是正则表达式的写法, `^`表示开始位置的匹配; `$`表示结束位置的匹配; 在grep命令中, 使用`-F`或`--fixed-strings`选项可以强制grep将模式作为固定字符串处理, 而不是正则表达式



# 文件夹大小
```bash
cd <dst_dir>
du -h --max-depth=0
du -h --max-depth=1
```

# 查看进程
```bash
# 查询所有进程
ps -ef 
# 查询某进程是否运行中
ps -ef | grep <process_name>
```

# chmod
```bash
# -R 递归将dir目录下所有chmod
# 指定新的所有者和所有组
sudo chown -R <USER_NAME>:<GROUP_NAME> <dir>
```

# 重新挂载分区
```bash
sudo mount -o remount rw /path/to/mount_point
```

# ssh相关
`-f "/home/xp/.ssh/known_hosts"` 指定了 known_hosts 文件的路径, 这个文件存储了你之前连接过的所有远程主机的 SSH 密钥指纹, 用于验证主机身份. `-R "172.20.1.22"` 指示 ssh-keygen 删除所有与 IP 地址 172.20.1.22 相关的条目. 这样做的目的是在下次尝试连接到该 IP 地址时, SSH 客户端不会因为密钥不匹配而拒绝连接, 而是会提示你接受新的密钥

```bash
ssh-keygen -f "/home/xp/.ssh/known_hosts" -R "172.20.1.22"
```

`sudo -S` 选项用于指示 sudo 命令从标准输入(stdin)而不是终端设备读取密码. 这允许脚本或其他自动化工具通过管道传递密码给 sudo, 而不需要用户交互式地输入密码. 
```bash
echo "pins" | sudo -S CMD
```

`killall -9` 强制关闭进程
```bash
echo 'nvidia' | sudo -S killall -9 /xpilot/timesync/phc2sys
```

设置系统时间
```bash
sudo -S date -s "{struct_time.tm_year}-{struct_time.tm_mon}-{struct_time.tm_mday} {struct_time.tm_hour}:{struct_time.tm_min}:{struct_time.tm_sec}"
```