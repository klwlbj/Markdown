#### 文件操作
```shell
 tail #默认查看后10行
    tail -f #动态显示文件内容，如日志
    tail -F #动态显示同名文件内容，即使vim保存时，把文件删了，又生成
    tail -n 20 文件名 #文件的最后20行
    tail -100f 动态显示最后一百行
    tail -f -n 100 前一百行
    tail -r 逆序显示，日志可用
cat #显示文件内容，如配置文件

head #显示前n行或n字节
    head -n 10 filename  查看前10行
    head -n -10 filename 查看除后10行以外的行
less

find [path] [选项] [文件名]...查找文件   
注：文件名完全一致才可找到，不可模糊搜索
    find / -iname php.ini -type f    //找出php.ini,-type f 表示找查普通文件
    -mtime 修改时间
    -user 所有者名称
    -name 按文件名搜索
    -iname 按文件名搜索，不区分大小写
    -size 按大小搜索文件
    -type f 普通文件
    -type d 目录

ls
cd ./path          # 切换到当前目录下的path目录中，“.”表示当前目录 
    cd - #进入上次打开的目录
    cd -2 #进入上上次打开的目录 
    cd ~
    cd ../../
    cd 'a b' # 进入有空格的目录
    
rm -f # 强制删除 
tree 
pwd -P # 显示出实际路径，而非使用连接（link）路径；pwd显示的是连接路径
mkdir
rmdir
```
#### 系统操作
```shell
top [-p]/[-m] #按cpu或按内存排序

iostat #监控IO负载
    iostat -mtx 2 # 每两秒刷新
    yum install sysstat # 安装
    
iftop #监控网络占用
    yum install iftop# 安装

# 详见：https://www.cnblogs.com/redarmy/p/16876345.html
export #查看全局变量
    export ADDPATH=/root/bin #配置环境变量
    # 查看环境变量的几种方式
    vim ~/.bash_profile
    vim ~/.bashrc
    vim /etc/profile
    # 生效方式
    source [相应文件]
    
ps aux #命令显示运行的进程，还会显示进程的一些信息如pid, cpu和内存使用情况等：
kill  #结束进程

crontab #定时任务

free -m  #以MB为单位显示内存使用情况
top #性能分析工具,可设间隔时间，显示次数，指定用户，指定进程号等 
df -h  #磁盘空间
chmod #改变文件或目录权限
chown #改变文件所有者
useradd #添加用户
passwd #设置当前用户密码
diff #命令用于比较两个文件或目录的不同
whereis #搜索文件名的文件
which #会在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果
grep #分析信息，查找信息
grep abc aaa.txt     #在aaa.txt文件中查找包含abc的内容
ssh root@remoteserver -p 2222 #用户名连接到服务器什么端口
```
#### xshell相关
```shell
#xshell上下载文件
yum install lrzsz  #先安装插件
rz  #上传
sz  #下载
```
#### 安装应用相关
```shell
yum install       #centos系统
apt-get install   #ubuntu系统
apk add           #apline系统
```
#### 查看linux版本
```shell
cat /etc/os-release  ## 查看发行版本
# 查看内核版本，编译时间等
cat /proc/version  
uname -a

```
#### 复制文件
```shell
cat 文件1  >  文件3    # 文件1覆盖复制到文件3
cat 文件1  >> 文件3   # 文件1追加复制到文件3
cp 源文件 目标文件（夹） # 覆盖复制到指定目录
```
#### 查看端口占用
```shell
lsof -i:3306 
netstat -anp | grep 3306
```
#### Vim相关
```shell
# 复制粘贴
    dd #剪切当前行
    yy #复制当前行
    p #粘贴
# 查找
    /string #查找
    /string\c # 不区分大小写查找
    /string\C # 区分大小写查找
    n #下一个匹配
    N #上一个匹配
    enter键 #关闭搜索
# 查看时有行号
    :set nu
# 保存和退出
    :wq
    :x # 和:wq类似
    :q # 仅退出
    :q! # 不保存退出
    
u       #撤销
ctrl r  #重做
```
#### 压缩相关
```shell
#压缩
    tar -cvf jpg.tar *.jpg #将目录里所有jpg文件打包成jpg.tar 
    tar -czf #tar.gz文件
    tar -cjf #tar.bz2文件
    tar -cZf #tar.Z文件
    rar #rar格式文件
    zip #zip格式的压缩
#解压
    tar -xvf file.tar #解压 tar包
    tar -xzvf file.tar.gz #解压tar.gz
    tar -xjvf file.tar.bz2   #解压 tar.bz2
    tar -xZvf file.tar.Z   #解压tar.Z
    unrar e file.rar #解压rar
    unzip file.zip #解压zip
```
#### 组合命令
```shell
ls;pwd    #ls报错，仍可继续pwd
ls&& pwd   #ls报错无法继续
ls || pwd #ls报错才继续

换行命令
用\
```
#### 自定义命令
```shell
vim ~/.bashrc  编辑别名，仅使用于bash
source ~/.bashrc 
alias 显示所有命令别名
```

#### **zsh和bash快捷键**及快捷命令

| 表格                           | 功能                                                   |
| ------------------------------ | ------------------------------------------------------ |
| <mark>ctrl+u</mark>            | 删除行头至光标处                                       |
| <mark>ctrl+k</mark>            | 删除光标处至行尾                                       |
| <mark>ctrl+a</mark>            | 光标移动到最前面                                       |
| <mark>ctrl+e</mark>            | 光标移动到最后面                                       |
| ctrl+左/右                     | 移动单词                                               |
| ctrl+r                         | 查询历史命令                                           |
| ctrl+_  (下划线)               | 撤销命令输入                                           |
| ctrl+l                         | 相当于clear                                            |
| <mark>ctrl+s</mark>            | 停止输出，但仍执行，如ping时                           |
| <mark>ctrl+z</mark>            | 暂停执行任务，继续执行，使用fg(前台执行)和bg(后台执行) |
| <mark>ctrl+c</mark>            | <mark>强行中断</mark>任务                              |
| !!                             | bash中执行上一条命令，zsh中仅输出上一条命令，不执行    |
| !hi                            | 找出hi开头的历史命令                                   |
| r                              | zsh中执行上一条命令                                    |
| dirs -v           zsh中简写成d | 显示历史访问目录 ，显示完输入 “ ~1 ”访问第一个目录     |
| 键盘右键                       | zsh一键输入建议命令                                    |
|                                |                                                        |