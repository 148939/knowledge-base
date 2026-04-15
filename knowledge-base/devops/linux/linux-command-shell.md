# 检索索引：本文档收录【Linux】【常用命令】知识点，内容100%来自Linux官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Linux 是一个自由和开放源码的类 Unix 操作系统，它基于 POSIX 和 UNIX。Linux 最初由 Linus Torvalds 在 1991 年发布。Linux 以其稳定性、安全性、高性能和开源特性而著称，广泛应用于服务器、桌面、嵌入式系统、移动设备和云计算等领域。

Linux 命令行是与 Linux 系统交互的强大工具，掌握 Linux 常用命令是高效使用 Linux 的基础。

Linux 常用命令的特点：
1. **简洁高效**：命令设计简洁，功能强大
2. **灵活组合**：多个命令可以通过管道组合使用
3. **可脚本化**：命令可以编写成脚本自动化执行
4. **丰富的选项**：每个命令有丰富的选项和参数
5. **在线帮助**：man 和 help 提供详细帮助

## 2. 核心知识点（结构化层级）

### 2.1 文件系统与目录操作
1. 目录结构（/、/root、/home、/etc、/var、/usr、/tmp 等）
2. 路径表示（绝对路径、相对路径、~、.、..）
3. 目录操作命令（pwd、cd、ls、mkdir、rmdir）
4. 文件操作命令（touch、cp、mv、rm、ln）
5. 文件查看命令（cat、more、less、head、tail）
6. 文件查找命令（find、locate、which、whereis）
7. 文件权限与所有权（chmod、chown、chgrp）
8. 文件压缩与解压（tar、gzip、bzip2、zip、unzip）

### 2.2 文本处理与编辑
1. 文本编辑器（vi、vim、nano）
2. 文本查看（cat、tac、rev）
3. 文本分页（more、less）
4. 文本截取（head、tail、cut）
5. 文本排序（sort、uniq）
6. 文本统计（wc、wc -l、wc -w）
7. 文本搜索（grep、egrep、fgrep）
8. 文本替换（sed、awk）
9. 文本比较（diff、cmp）
10. 文本处理管道

### 2.3 系统信息与状态
1. 系统信息（uname、hostname、uptime、dmesg）
2. 系统监控（top、htop、ps、pstree）
3. 内存信息（free、vmstat）
4. 磁盘信息（df、du、iostat）
5. CPU 信息（lscpu、cat /proc/cpuinfo）
6. 系统时间（date、cal、timedatectl）
7. 系统负载（w、uptime、load average）
8. 系统日志（journalctl、tail -f /var/log/syslog）

### 2.4 进程管理
1. 查看进程（ps、pstree、top、htop）
2. 进程控制（kill、killall、pkill）
3. 进程优先级（nice、renice）
4. 后台进程（&、jobs、fg、bg）
5. 进程信号（SIGTERM、SIGKILL、SIGHUP）
6. 守护进程（systemd、service）
7. 进程监控（pidof、pgrep、lsof）

### 2.5 网络操作
1. 网络配置（ifconfig、ip addr、ip link）
2. 网络连接（ping、traceroute、mtr）
3. 网络服务（netstat、ss、lsof）
4. 网络下载（wget、curl）
5. 网络传输（scp、sftp、rsync）
6. 域名查询（nslookup、dig、host）
7. 网络防火墙（iptables、ufw、firewalld）
8. SSH 远程登录（ssh、ssh-keygen、ssh-copy-id）

### 2.6 用户与权限管理
1. 用户管理（useradd、userdel、usermod、passwd）
2. 用户组管理（groupadd、groupdel、groupmod）
3. 用户切换（su、sudo、visudo）
4. 用户信息（id、who、w、whoami）
5. 文件权限（chmod、umask）
6. 文件所有权（chown、chgrp）
7. 特殊权限（SUID、SGID、Sticky Bit）
8. ACL 权限（getfacl、setfacl）

### 2.7 Shell 脚本基础
1. Shell 类型（bash、sh、zsh、fish）
2. 脚本创建与执行（#!/bin/bash、chmod +x、./script.sh）
3. 变量（定义、使用、环境变量、参数）
4. 条件判断（if、case、test、[ ]、[[ ]]）
5. 循环（for、while、until）
6. 函数（定义、调用、参数、返回值）
7. 输入输出（read、echo、printf、重定向）
8. 管道与重定向（|、>、>>、2>&1）

### 2.8 软件包管理
1. Debian/Ubuntu（apt、apt-get、dpkg）
2. RHEL/CentOS（yum、dnf、rpm）
3. Arch Linux（pacman）
4. 源码编译（./configure、make、make install）
5. 软件仓库管理
6. 依赖关系处理

### 2.9 磁盘与文件系统
1. 磁盘信息（fdisk、parted、lsblk）
2. 挂载与卸载（mount、umount、/etc/fstab）
3. 文件系统（ext4、xfs、btrfs、ntfs）
4. 磁盘检查（fsck、e2fsck）
5. 磁盘配额（quota）
6. LVM 逻辑卷管理
7. RAID 磁盘阵列

### 2.10 系统服务与启动
1. Systemd（systemctl、journalctl）
2. SysVinit（service、chkconfig）
3. 系统启动（/etc/init.d、/etc/rc.local）
4. 定时任务（crontab、at、anacron）
5. 系统日志（/var/log/、rsyslog、logrotate）

### 2.11 常用命令速查表
1. 文件与目录操作速查
2. 文本处理速查
3. 系统监控速查
4. 网络操作速查
5. 进程管理速查

## 3. 官方标准语法 / API

### 文件系统与目录操作
```bash
# 查看当前目录
pwd

# 切换目录
cd /path/to/directory
cd ~  # 切换到家目录
cd -  # 切换到上次目录
cd ..  # 切换到上级目录

# 列出目录内容
ls
ls -l  # 长格式
ls -a  # 显示隐藏文件
ls -la  # 长格式显示所有文件
ls -lh  # 人类可读格式
ls -lt  # 按时间排序
ls -R  # 递归显示

# 创建目录
mkdir directory
mkdir -p dir1/dir2/dir3  # 递归创建
mkdir -m 755 directory  # 指定权限

# 删除空目录
rmdir directory
rmdir -p dir1/dir2/dir3  # 递归删除空目录

# 创建空文件
touch filename
touch -t 202401011200 filename  # 指定时间

# 复制文件
cp source.txt dest.txt
cp -r source_dir dest_dir  # 递归复制目录
cp -i source.txt dest.txt  # 交互式提示
cp -v source.txt dest.txt  # 显示详细信息
cp -a source dest  # 保留属性复制

# 移动/重命名文件
mv oldname newname
mv file.txt /path/to/directory/
mv -i oldname newname  # 交互式提示
mv -v oldname newname  # 显示详细信息

# 删除文件
rm filename
rm -i filename  # 交互式确认
rm -r directory  # 递归删除目录
rm -rf directory  # 强制递归删除（慎用）
rm -v filename  # 显示详细信息

# 创建链接
ln -s target linkname  # 软链接
ln target linkname  # 硬链接

# 查看文件内容
cat filename
cat -n filename  # 显示行号
cat file1 file2 > combined  # 合并文件

# 分页查看
more filename
less filename  # 更强大的分页查看

# 查看文件开头/结尾
head filename
head -n 20 filename  # 前20行
tail filename
tail -n 20 filename  # 后20行
tail -f filename  # 实时跟踪

# 查找文件
find /path -name "*.txt"
find /path -type f -size +10M
find /path -mtime -7  # 7天内修改
find /path -exec command {} \;

# 快速查找（需要updatedb）
locate filename

# 查找命令位置
which command
whereis command

# 文件权限
chmod 755 filename
chmod u+x filename
chmod g-w filename
chmod o=r filename
chmod -R 755 directory

# 文件所有者
chown user:group filename
chown -R user:group directory
chgrp group filename

# 文件压缩
tar -cvf archive.tar files/
tar -xvf archive.tar
tar -czvf archive.tar.gz files/
tar -xzvf archive.tar.gz
tar -cjvf archive.tar.bz2 files/
tar -xjvf archive.tar.bz2

gzip filename
gzip -d filename.gz

zip archive.zip files/
unzip archive.zip
```

### 文本处理命令
```bash
# 查看文件
cat filename
cat -n filename  # 显示行号

# 分页查看
more filename
less filename
# less 操作：
# 空格 - 向下翻页
# b - 向上翻页
# /pattern - 搜索
# n - 下一个匹配
# N - 上一个匹配
# q - 退出

# 查看开头/结尾
head filename
head -n 10 filename
tail filename
tail -n 10 filename
tail -f filename  # 实时跟踪

# 截取列
cut -d: -f1 /etc/passwd
cut -c1-10 filename

# 排序
sort filename
sort -r filename  # 逆序
sort -n filename  # 数字排序
sort -k2 filename  # 按第2列排序
sort -u filename  # 去重排序

# 去重
uniq filename
uniq -c filename  # 统计出现次数
uniq -d filename  # 只显示重复行

# 统计
wc filename
wc -l filename  # 行数
wc -w filename  # 词数
wc -c filename  # 字节数

# 搜索
grep pattern filename
grep -i pattern filename  # 忽略大小写
grep -v pattern filename  # 反向匹配
grep -n pattern filename  # 显示行号
grep -c pattern filename  # 统计匹配数
grep -r pattern /path  # 递归搜索
grep -A 5 pattern filename  # 显示匹配后5行
grep -B 5 pattern filename  # 显示匹配前5行
grep -C 2 pattern filename  # 显示匹配前后2行

# 正则表达式搜索
egrep "pattern1|pattern2" filename

# 流编辑器
sed 's/old/new/g' filename
sed -i 's/old/new/g' filename  # 原地修改
sed -n '1,10p' filename  # 打印1-10行
sed '/pattern/d' filename  # 删除匹配行

# 文本处理
awk '{print $1}' filename
awk -F: '{print $1}' /etc/passwd
awk '{sum += $1} END {print sum}' filename
awk '/pattern/ {print $0}' filename

# 比较文件
diff file1 file2
diff -u file1 file2  # 统一格式
cmp file1 file2

# 文本统计
wc filename  # 行数、词数、字节数
```

### 系统信息命令
```bash
# 系统信息
uname -a
uname -s  # 内核名称
uname -r  # 内核版本
uname -m  # 机器架构

# 主机名
hostname
hostnamectl
hostname newname

# 运行时间
uptime
w

# 系统启动信息
dmesg
dmesg | less

# 系统监控
top
# top 交互命令：
# P - 按CPU排序
# M - 按内存排序
# T - 按时间排序
# k - 杀死进程
# q - 退出

# 更好的系统监控
htop

# 查看进程
ps
ps aux
ps -ef
ps -u username

# 进程树
pstree
pstree -p  # 显示PID

# 内存信息
free
free -h  # 人类可读
free -m  # 以MB显示
free -g  # 以GB显示

# 内存统计
vmstat
vmstat 1  # 每秒刷新

# 磁盘信息
df
df -h  # 人类可读
df -T  # 显示文件系统类型

# 目录大小
du
du -h  # 人类可读
du -sh directory  # 总计
du -d 1 directory  # 深度1

# IO 统计
iostat
iostat -x  # 扩展统计
iostat 1  # 每秒刷新

# CPU 信息
lscpu
cat /proc/cpuinfo

# 系统时间
date
date +"%Y-%m-%d %H:%M:%S"
cal
cal 2024
cal -3  # 显示前后月

# 时间同步
timedatectl
timedatectl list-timezones
timedatectl set-timezone Asia/Shanghai

# 系统负载
uptime
w
cat /proc/loadavg

# 系统日志
journalctl
journalctl -u service
journalctl -f  # 实时跟踪
journalctl --since "2024-01-01"

# 查看系统日志
tail -f /var/log/syslog
tail -f /var/log/messages
tail -f /var/log/auth.log
```

### 进程管理命令
```bash
# 查看进程
ps
ps aux  # 查看所有进程
ps -ef  # 查看所有进程（System V风格）
ps -u username  # 查看指定用户进程

# 查看进程树
pstree
pstree -p  # 显示PID
pstree -a  # 显示命令行参数

# 实时监控进程
top
htop

# 查找进程
pgrep pattern
pgrep -u username pattern
pidof processname

# 查看进程打开的文件
lsof
lsof -p PID
lsof -u username
lsof -i :port

# 结束进程
kill PID
kill -l  # 列出信号
kill -9 PID  # 强制结束
kill -15 PID  # 优雅结束（默认）
kill -HUP PID  # 重新加载配置

# 结束同名进程
killall processname
killall -9 processname

# 按模式结束进程
pkill pattern
pkill -u username pattern

# 进程优先级
nice -n 10 command  # 以优先级10运行
renice -n 5 -p PID  # 调整已有进程优先级
renice -n -5 -u username  # 调整用户所有进程优先级

# 后台运行
command &

# 查看后台任务
jobs

# 后台任务放到前台
fg %jobnumber
fg  # 最近的任务

# 后台任务放到后台运行
bg %jobnumber

# 中止任务
Ctrl+C  # 中止当前任务
Ctrl+Z  # 暂停当前任务

# 查看进程详细信息
cat /proc/PID/status
cat /proc/PID/cmdline
cat /proc/PID/environ
```

### 网络操作命令
```bash
# 网络接口配置
ifconfig
ifconfig -a

# 新的 ip 命令
ip addr show
ip link show
ip route show

# 配置网络接口
ip addr add 192.168.1.100/24 dev eth0
ip link set eth0 up
ip route add default via 192.168.1.1

# 测试网络连接
ping hostname
ping -c 4 hostname  # 发送4次
ping -i 2 hostname  # 间隔2秒

# 路由追踪
traceroute hostname
mtr hostname  # 更好的路由追踪

# 查看网络连接
netstat
netstat -tuln  # 查看监听端口
netstat -tulnp  # 查看监听端口和进程
netstat -an  # 查看所有连接
netstat -rn  # 查看路由表

# 新的 ss 命令（替代 netstat）
ss
ss -tuln
ss -tulnp
ss -an

# 查看打开的网络文件
lsof -i
lsof -i :port
lsof -i @hostname
lsof -i TCP:port

# 下载文件
wget url
wget -O filename url
wget -c url  # 断点续传
wget -r url  # 递归下载

# 下载和上传
curl url
curl -o filename url
curl -O url
curl -L url  # 跟随重定向
curl -d "data" url  # POST数据
curl -X POST url  # 指定方法

# 安全复制
scp file.txt user@host:/path/
scp user@host:/path/file.txt ./
scp -r directory user@host:/path/

# SFTP
sftp user@host

# 同步文件
rsync -av source/ user@host:/path/
rsync -avz source/ user@host:/path/  # 压缩传输
rsync -av --delete source/ user@host:/path/  # 删除目标多余文件

# DNS 查询
nslookup hostname
nslookup -type=A hostname
dig hostname
dig +short hostname
host hostname

# SSH 远程登录
ssh user@host
ssh -p port user@host
ssh -i keyfile user@host

# SSH 密钥生成
ssh-keygen
ssh-keygen -t rsa -b 4096

# 复制公钥到服务器
ssh-copy-id user@host

# 查看防火墙
iptables -L
iptables -L -n -v

# UFW 防火墙（Ubuntu）
ufw status
ufw enable
ufw disable
ufw allow 22/tcp
ufw allow from 192.168.1.0/24

# Firewalld（CentOS/RHEL）
firewall-cmd --list-all
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --reload
```

### 用户与权限管理命令
```bash
# 添加用户
useradd username
useradd -m username  # 创建家目录
useradd -s /bin/bash username  # 指定shell
useradd -g groupname username  # 指定主组
useradd -G group1,group2 username  # 指定附加组

# 删除用户
userdel username
userdel -r username  # 删除家目录

# 修改用户
usermod -l newname oldname  # 修改用户名
usermod -d /new/home username  # 修改家目录
usermod -s /bin/zsh username  # 修改shell
usermod -aG groupname username  # 添加到组

# 修改密码
passwd
passwd username

# 用户信息
id
id username
whoami
who
w
users

# 添加用户组
groupadd groupname
groupadd -g 1000 groupname  # 指定GID

# 删除用户组
groupdel groupname

# 修改用户组
groupmod -n newname oldname
groupmod -g 1001 groupname

# 切换用户
su username
su - username  # 切换到用户环境
su -c "command" username  # 执行命令后返回

# sudo 执行
sudo command
sudo -u username command
sudo -i  # 切换到root

# 编辑 sudoers
visudo

# 文件权限
ls -l

# 修改权限
chmod 755 filename
chmod u=rwx,g=rx,o=rx filename
chmod u+x filename
chmod g-w filename
chmod o-r filename
chmod -R 755 directory

# 权限数字说明
# 4 - r (读)
# 2 - w (写)
# 1 - x (执行)
# 755 = rwxr-xr-x
# 644 = rw-r--r--

# 修改所有者
chown user:group filename
chown user filename
chown :group filename
chown -R user:group directory

# 修改所属组
chgrp group filename
chgrp -R group directory

# 特殊权限
chmod u+s filename  # SUID
chmod g+s filename  # SGID
chmod +t directory  # Sticky Bit

# umask
umask
umask 0022

# ACL 权限
getfacl filename
setfacl -m u:username:rwx filename
setfacl -m g:groupname:rwx filename
setfacl -x u:username filename
```

### Shell 脚本基础
```bash
#!/bin/bash

# 注释
# 这是一行注释

# 变量
VAR="Hello"
echo $VAR
echo ${VAR}

# 只读变量
readonly PI=3.14

# 环境变量
export MYVAR="value"
echo $MYVAR
echo $PATH
echo $HOME
echo $USER

# 位置参数
echo $0  # 脚本名
echo $1  # 第1个参数
echo $2  # 第2个参数
echo $#  # 参数数量
echo $*  # 所有参数
echo $@  # 所有参数（保留空格）

# 条件判断
if [ condition ]; then
    # do something
elif [ condition ]; then
    # do something else
else
    # do default
fi

# 字符串判断
[ -z "$VAR" ]  # 空字符串
[ -n "$VAR" ]  # 非空字符串
[ "$VAR1" = "$VAR2" ]  # 相等
[ "$VAR1" != "$VAR2" ]  # 不相等

# 数字判断
[ $num1 -eq $num2 ]  # 相等
[ $num1 -ne $num2 ]  # 不相等
[ $num1 -gt $num2 ]  # 大于
[ $num1 -lt $num2 ]  # 小于
[ $num1 -ge $num2 ]  # 大于等于
[ $num1 -le $num2 ]  # 小于等于

# 文件判断
[ -f filename ]  # 文件存在且是普通文件
[ -d directory ]  # 目录存在
[ -e filename ]  # 文件存在
[ -r filename ]  # 可读
[ -w filename ]  # 可写
[ -x filename ]  # 可执行
[ -s filename ]  # 文件存在且非空

# 更强大的条件判断（bash）
if [[ condition ]]; then
    # do something
fi

# case 语句
case $VAR in
    pattern1)
        # do something
        ;;
    pattern2|pattern3)
        # do something
        ;;
    *)
        # default
        ;;
esac

# for 循环
for var in list; do
    # do something
done

for ((i=0; i<10; i++)); do
    echo $i
done

# while 循环
while [ condition ]; do
    # do something
done

# until 循环
until [ condition ]; do
    # do something
done

# 函数
function myfunc {
    # do something
}

myfunc() {
    local var1=$1  # 局部变量
    local var2=$2
    echo $var1 $var2
}

myfunc arg1 arg2

# 函数返回值
myfunc() {
    return 0  # 成功
    return 1  # 失败
}

myfunc
echo $?  # 获取返回值

# 输入
read var
read -p "Enter something: " var
read -t 10 var  # 超时10秒

# 输出
echo "Hello World"
echo -n "Hello"  # 不换行
echo -e "Line1\nLine2"  # 转义字符

printf "Hello %s\n" "World"
printf "Number: %d\n" 100

# 重定向
command > file  # 标准输出重定向（覆盖）
command >> file  # 标准输出重定向（追加）
command 2> file  # 标准错误重定向
command &> file  # 标准输出和错误都重定向

# 管道
command1 | command2
command1 | command2 | command3

# 命令替换
var=$(command)
var=`command`

# 算术运算
result=$((1 + 2))
result=$((a + b))
let result=a+b

# 退出脚本
exit 0  # 成功
exit 1  # 失败

# 信号处理
trap cleanup SIGINT SIGTERM

cleanup() {
    echo "Cleaning up..."
    exit 0
}
```

## 4. 官方示例代码（可运行）

### 常用组合命令示例
```bash
# ========== 查找大文件 ==========
# 查找当前目录下大于100MB的文件
find . -type f -size +100M -exec ls -lh {} \;

# ========== 查找并删除临时文件 ==========
# 查找并删除7天前的临时文件
find /tmp -type f -mtime +7 -delete

# ========== 磁盘空间分析 ==========
# 查看当前目录各文件夹大小
du -sh * | sort -h

# ========== 查看网络连接 ==========
# 查看所有TCP连接
netstat -tuln
# 或使用 ss
ss -tuln

# ========== 进程监控 ==========
# 查看内存占用最多的10个进程
ps aux --sort=-%mem | head -10

# 查看CPU占用最多的10个进程
ps aux --sort=-%cpu | head -10

# ========== 日志分析 ==========
# 统计访问日志中IP出现次数
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10

# ========== 批量操作 ==========
# 批量重命名文件
for file in *.txt; do
    mv "$file" "${file%.txt}.bak"
done

# ========== 系统备份 ==========
# 备份目录并压缩
tar -czvf backup_$(date +%Y%m%d).tar.gz /path/to/directory

# ========== 系统监控 ==========
# 监控CPU和内存使用
watch -n 1 'ps aux --sort=-%cpu | head -11'

# ========== 搜索替换 ==========
# 批量替换文件中的文本
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# ========== 端口检查 ==========
# 检查端口是否被占用
lsof -i :8080
netstat -tuln | grep 8080

# ========== 文件比较 ==========
# 比较两个目录差异
diff -r dir1 dir2

# ========== SSH 批量执行 ==========
# 批量在多个服务器上执行命令
for host in host1 host2 host3; do
    ssh $host "uptime"
done
```

### Shell 脚本示例
```bash
#!/bin/bash
# 脚本名称: backup.sh
# 脚本功能: 自动备份重要目录

# 配置
BACKUP_DIR="/backup"
SOURCE_DIRS=(
    "/home/user/data"
    "/etc"
    "/var/www"
)
DATE=$(date +%Y%m%d_%H%M%S)
RETENTION_DAYS=7

# 创建备份目录
mkdir -p "$BACKUP_DIR"

# 循环备份
for dir in "${SOURCE_DIRS[@]}"; do
    if [ -d "$dir" ]; then
        echo "Backing up $dir..."
        filename=$(basename "$dir")
        tar -czvf "$BACKUP_DIR/${filename}_${DATE}.tar.gz" "$dir"
    else
        echo "Warning: $dir does not exist"
    fi
done

# 删除7天前的备份
echo "Cleaning up old backups..."
find "$BACKUP_DIR" -name "*.tar.gz" -mtime +$RETENTION_DAYS -delete

echo "Backup completed!"
```

### 系统监控脚本
```bash
#!/bin/bash
# 脚本名称: monitor.sh
# 脚本功能: 监控系统资源并告警

# 阈值
CPU_THRESHOLD=80
MEM_THRESHOLD=80
DISK_THRESHOLD=90

# 获取CPU使用率
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d. -f1)

# 获取内存使用率
MEM_USAGE=$(free | grep Mem | awk '{print $3/$2 * 100.0}' | cut -d. -f1)

# 获取磁盘使用率
DISK_USAGE=$(df -h / | grep / | awk '{print $5}' | cut -d% -f1)

echo "=== System Monitor ==="
echo "CPU Usage: ${CPU_USAGE}%"
echo "Memory Usage: ${MEM_USAGE}%"
echo "Disk Usage: ${DISK_USAGE}%"

# 检查CPU
if [ "$CPU_USAGE" -gt "$CPU_THRESHOLD" ]; then
    echo "ALERT: CPU usage is high: ${CPU_USAGE}%"
fi

# 检查内存
if [ "$MEM_USAGE" -gt "$MEM_THRESHOLD" ]; then
    echo "ALERT: Memory usage is high: ${MEM_USAGE}%"
fi

# 检查磁盘
if [ "$DISK_USAGE" -gt "$DISK_THRESHOLD" ]; then
    echo "ALERT: Disk usage is high: ${DISK_USAGE}%"
fi

echo "=== Monitor Complete ==="
```

## 5. 官方注意事项 / 最佳实践

### 文件操作最佳实践
1. **使用绝对路径**：脚本中使用绝对路径避免问题
2. **备份重要文件**：修改配置文件前先备份
3. **谨慎使用 rm -rf**：确认路径正确，避免误删
4. **使用 -i 确认**：删除文件时使用 -i 交互确认
5. **合理使用通配符**：理解 *、?、[] 等通配符的作用
6. **检查文件是否存在**：操作前先检查文件是否存在
7. **使用引号包裹变量**：`"$VAR"` 避免空格问题

### 文本处理最佳实践
1. **选择合适的工具**：grep 搜索、sed 替换、awk 处理表格
2. **正则表达式**：掌握基本的正则表达式语法
3. **备份原文件**：sed -i 修改前先备份
4. **管道组合使用**：多个命令通过管道组合更强大
5. **使用 less 查看大文件**：大文件用 less 不用 cat
6. **tail -f 实时跟踪**：日志文件用 tail -f 实时查看

### 系统监控最佳实践
1. **定期监控**：定期检查系统资源使用情况
2. **设置告警**：资源使用率超过阈值时告警
3. **日志轮转**：配置 logrotate 防止日志文件过大
4. **性能基准**：建立系统性能基准线
5. **监控进程**：关注异常进程和僵尸进程
6. **磁盘空间**：定期检查磁盘空间，及时清理

### 网络操作最佳实践
1. **使用 SSH 密钥**：使用密钥认证代替密码
2. **保护 SSH 配置**：修改默认端口，禁用 root 登录
3. **防火墙配置**：只开放必要的端口
4. **安全传输**：使用 scp、sftp、rsync 加密传输
5. **网络诊断**：网络问题时按顺序诊断（ping → traceroute → telnet）
6. **监控网络连接**：定期检查网络连接和端口监听

### 用户权限最佳实践
1. **最小权限原则**：用户只拥有必要的权限
2. **使用 sudo**：日常使用普通用户，需要时用 sudo
3. **强密码策略**：设置复杂密码，定期更换
4. **权限检查**：定期检查重要文件的权限
5. **避免 root 登录**：不要直接用 root 登录，用 su 或 sudo
6. **审计日志**：记录 sudo 使用和登录日志

### Shell 脚本最佳实践
1. **添加 Shebang**：脚本第一行加 `#!/bin/bash`
2. **添加注释**：脚本和关键逻辑添加注释
3. **检查命令是否成功**：使用 `$?` 检查命令执行结果
4. **使用 set -euo pipefail**：增强脚本健壮性
5. **使用函数**：重复代码封装成函数
6. **引用变量**：`"$VAR"` 避免空格问题
7. **脚本可执行**：`chmod +x script.sh`
8. **测试脚本**：先在测试环境验证脚本
9. **日志记录**：重要操作记录日志
10. **错误处理**：添加错误处理和回滚逻辑

### 常用命令速查表

#### 文件与目录
| 命令 | 说明 |
|------|------|
| `pwd` | 显示当前目录 |
| `cd` | 切换目录 |
| `ls` | 列出目录内容 |
| `mkdir` | 创建目录 |
| `touch` | 创建文件 |
| `cp` | 复制文件 |
| `mv` | 移动/重命名文件 |
| `rm` | 删除文件 |
| `ln` | 创建链接 |

#### 查看文件
| 命令 | 说明 |
|------|------|
| `cat` | 查看文件内容 |
| `more` | 分页查看 |
| `less` | 更强大的分页查看 |
| `head` | 查看开头 |
| `tail` | 查看结尾 |

#### 文本搜索
| 命令 | 说明 |
|------|------|
| `grep` | 搜索文本 |
| `find` | 查找文件 |
| `locate` | 快速查找文件 |

#### 系统监控
| 命令 | 说明 |
|------|------|
| `top` | 实时监控 |
| `htop` | 更好的实时监控 |
| `ps` | 查看进程 |
| `free` | 查看内存 |
| `df` | 查看磁盘 |
| `du` | 查看目录大小 |

#### 网络操作
| 命令 | 说明 |
|------|------|
| `ping` | 测试连接 |
| `traceroute` | 路由追踪 |
| `netstat` | 网络连接 |
| `ss` | 网络连接（新） |
| `wget` | 下载文件 |
| `curl` | 下载/上传 |
| `ssh` | 远程登录 |
| `scp` | 安全复制 |

## 6. 官方文档参考链接

- Linux 官方文档：https://www.kernel.org/doc/html/latest/
- Linux man pages：https://man7.org/linux/man-pages/
- Linux 命令速查：https://linuxcommand.org/
- Bash 参考手册：https://www.gnu.org/software/bash/manual/
- Ubuntu 文档：https://help.ubuntu.com/
- CentOS 文档：https://docs.centos.org/
- Arch Linux Wiki：https://wiki.archlinux.org/
- Linux From Scratch：https://www.linuxfromscratch.org/
- Linux GitHub：https://github.com/torvalds/linux
