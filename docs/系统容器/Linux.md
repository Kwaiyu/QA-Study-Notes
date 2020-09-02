## 环境变量

### 当前会话

```
export PATH=$PATH:/usr/local/webserver/php/bin
```

### 当前登录

```
vi ~/.bash_profile
在PATH=$PATH:$HOME/bin一行之后添加/usr/local/webserver/php/bin
```

### 系统用户永久

```
vi /etc/profile
在文件末尾添加
PATH=$PATH:/usr/local/webserver/php/bin:/usr/local/webserver/mysql/bin
export PATH
source /etc/profile
echo $PATH
```

## 文件

### yum(Centos8弃用yum,改为dnf)

yum软件包管理器阿里镜像：

[](https://mirrors.aliyun.com/repo/Centos-7.repo)

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

虚拟机vm tools共享文件夹：

```
mkdir /mnt/cdrom
mount -t iso9660 /dev/cdrom /mnt/cdrom
cp /mnt/cdrom/VMtools.tar.gz /root/vm.tar.gz
tar -xzf vm.tar.gz
cd vmware-tools-distrib/
./vmware-install.pl
cp mnt/hgfs/file /etc/yum.repos.d/CentOS-Base.repo
chmod 644
```

配置DNS开启网卡清缓存：

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
ONBOOT=YES
DNS1=223.5.5.5
DNS2=223.6.6.6
vi /etc/resolv.conf
nameserver 223.5.5.5
nameserver 223.6.6.6
yum clean all
yum makecache
service network restart
```

yum常用命令：

- 1.列出所有可更新的软件清单命令：yum check-update
- 2.更新所有软件命令：yum update
- 3.仅安装指定的软件命令：yum install <package_name>
- 4.仅更新指定的软件命令：yum update <package_name>
- 5.列出所有可安裝的软件清单命令：yum list
- 6.删除软件包命令：yum remove <package_name>
- 7.查找软件包 命令：yum search <keyword>
- 8.清除缓存命令:
  - yum clean packages: 清除缓存目录下的软件包
  - yum clean headers: 清除缓存目录下的 headers
  - yum clean oldheaders: 清除缓存目录下旧的 headers
  - yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除缓存目录下的软件包及旧的headers
- 9. yum provides /usr/bin/ab 查找包含程序的软件包

### apt

### grep

Linux grep 命令用于查找文件里符合条件的字符串。查找内容包含指定的范本样式的文件，如果发现某文件的内容符合所指定的范本样式，预设 grep 指令会把含有范本样式的那一列显示出来。若不指定任何文件名称，或是所给予的文件名为 **-**，则 grep 指令会从标准输入设备读取数据。

```
grep [-abcEFGhHilLnqrsvVwxy][-A<显示列数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样式>][-f<范本文件>][--help][范本样式][文件或目录...]
```

- **-a 或 --text** : 不要忽略二进制的数据。
- **-A<显示行数> 或 --after-context=<显示行数>** : 除了显示符合范本样式的那一列之外，并显示该行之后的内容。
- **-b 或 --byte-offset** : 在显示符合样式的那一行之前，标示出该行第一个字符的编号。
- **-B<显示行数> 或 --before-context=<显示行数>** : 除了显示符合样式的那一行之外，并显示该行之前的内容。
- **-c 或 --count** : 计算符合样式的列数。
- **-C<显示行数> 或 --context=<显示行数>或-<显示行数>** : 除了显示符合样式的那一行之外，并显示该行之前后的内容。
- **-d <动作> 或 --directories=<动作>** : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
- **-e<范本样式> 或 --regexp=<范本样式>** : 指定字符串做为查找文件内容的样式。
- **-E 或 --extended-regexp** : 将样式为延伸的正则表达式来使用。
- **-f<规则文件> 或 --file=<规则文件>** : 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
- **-F 或 --fixed-regexp** : 将样式视为固定字符串的列表。
- **-G 或 --basic-regexp** : 将样式视为普通的表示法来使用。
- **-h 或 --no-filename** : 在显示符合样式的那一行之前，不标示该行所属的文件名称。
- **-H 或 --with-filename** : 在显示符合样式的那一行之前，表示该行所属的文件名称。
- **-i 或 --ignore-case** : 忽略字符大小写的差别。
- **-l 或 --file-with-matches** : 列出文件内容符合指定的样式的文件名称。
- **-L 或 --files-without-match** : 列出文件内容不符合指定的样式的文件名称。
- **-n 或 --line-number** : 在显示符合样式的那一行之前，标示出该行的列数编号。
- **-o 或 --only-matching** : 只显示匹配PATTERN 部分。
- **-q 或 --quiet或--silent** : 不显示任何信息。
- **-r 或 --recursive** : 此参数的效果和指定"-d recurse"参数相同。
- **-s 或 --no-messages** : 不显示错误信息。
- **-v 或 --revert-match** : 显示不包含匹配文本的所有行。
- **-V 或 --version** : 显示版本信息。
- **-w 或 --word-regexp** : 只显示全字符合的列。
- **-x --line-regexp** : 只显示全列符合的列。
- **-y** : 此参数的效果和指定"-i"参数相同。

### awk

AWK 是一种处理文本文件的语言，是一个强大的文本分析工具。

```
awk [选项参数] 'script' var=value file(s)
或
awk [选项参数] -f scriptfile var=value file(s)
```

- -F fs or --field-separator fs
  指定输入文件折分隔符，fs是一个字符串或者是一个正则表达式，如-F:。
- -v var=value or --asign var=value
  赋值一个用户定义变量。
- -f scripfile or --file scriptfile
  从脚本文件中读取awk命令。
- -mf nnn and -mr nnn
  对nnn值设置内在限制，-mf选项限制分配给nnn的最大块数目；-mr选项限制记录的最大数目。这两个功能是Bell实验室版awk的扩展功能，在标准awk中不适用。
- -W compact or --compat, -W traditional or --traditional
  在兼容模式下运行awk。所以gawk的行为和标准的awk完全一样，所有的awk扩展都被忽略。
- -W copyleft or --copyleft, -W copyright or --copyright
  打印简短的版权信息。
- -W help or --help, -W usage or --usage
  打印全部awk选项和每个选项的简短说明。
- -W lint or --lint
  打印不能向传统unix平台移植的结构的警告。
- -W lint-old or --lint-old
  打印关于不能向传统unix平台移植的结构的警告。
- -W posix
  打开兼容模式。但有以下限制，不识别：/x、函数关键字、func、换码序列以及当fs是一个空格时，将新行作为一个域分隔符；操作符**和**=不能代替^和^=；fflush无效。
- -W re-interval or --re-inerval
  允许间隔正则表达式的使用，参考(grep中的Posix字符类)，如括号表达式[[:alpha:]]。
- -W source program-text or --source program-text
  使用program-text作为源代码，可与-f命令混用。
- -W version or --version
  打印bug报告信息的版本。

### sed

Linux sed 命令是利用脚本来处理文本文件。可依照脚本的指令来处理、编辑文本文件。主要用来自动编辑一个或多个文件、简化对文件的反复操作、编写转换程序等。

```
sed [-hnV][-e<script>][-f<script文件>][文本文件]
```

- -e `<script>`或--expression=`<script> `以选项中指定的script来处理输入的文本文件。
- -f<script文件>或--file=<script文件> 以选项中指定的script文件来处理输入的文本文件。
- -h或--help 显示帮助。
- -n或--quiet或--silent 仅显示script处理后的结果。
- -V或--version 显示版本信息。

**动作说明**：

- a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
- c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
- d ：删除，因为是删除啊，所以 d 后面通常不接任何咚咚；
- i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
- p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
- s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g

### expr

expr命令是一个手工命令行计数器，用于在UNIX/LINUX下求表达式变量的值，一般用于整数值，也可用于字符串

```
expr 表达式
```

- 用空格隔开每个项；
- 用 / (反斜杠) 放在 shell 特定的字符前面；
- 对包含空格和其他特殊字符的字符串要用引号括起来

### let

let 命令是 BASH 中用于计算的工具，用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量。如果表达式中包含了空格或其他特殊字符，则必须引起来。

```
let arg [arg ...]
```

arg：要执行的表达式

### sort

sort命令用于将文本文件内容排序

```
sort [-bcdfimMnr][-o<输出文件>][-t<分隔字符>][+<起始栏位>-<结束栏位>][--help][--verison][文件]
```

- -b 忽略每行前面开始出的空格字符。
- -c 检查文件是否已经按照顺序排序。
- -d 排序时，处理英文字母、数字及空格字符外，忽略其他的字符。
- -f 排序时，将小写字母视为大写字母。
- -i 排序时，除了040至176之间的ASCII字符外，忽略其他的字符。
- -m 将几个排序好的文件进行合并。
- -M 将前面3个字母依照月份的缩写进行排序。
- -n 依照数值的大小排序。
- -u 意味着是唯一的(unique)，输出的结果是去完重了的。
- -o<输出文件> 将排序后的结果存入指定的文件。
- -r 以相反的顺序来排序。
- -t<分隔字符> 指定排序时所用的栏位分隔字符。
- +<起始栏位>-<结束栏位> 以指定的栏位来排序，范围由起始栏位到结束栏位的前一栏位。
- --help 显示帮助。
- --version 显示版本信息。

### uniq

用于检查及删除文本文件中重复出现的行列，一般与sort结合使用

```
uniq [-cdu][-f<栏位>][-s<字符位置>][-w<字符位置>][--help][--version][输入文件][输出文件]
```

- -c或--count 在每列旁边显示该行重复出现的次数。
- -d或--repeated 仅显示重复出现的行列。
- -f<栏位>或--skip-fields=<栏位> 忽略比较指定的栏位。
- -s<字符位置>或--skip-chars=<字符位置> 忽略比较指定的字符。
- -u或--unique 仅显示出一次的行列。
- -w<字符位置>或--check-chars=<字符位置> 指定要比较的字符。
- --help 显示帮助。
- --version 显示版本信息。
- [输入文件] 指定已排序好的文本文件。如果不指定此项，则从标准读取数据；
- [输出文件] 指定输出的文件。如果不指定此选项，则将内容显示到标准输出设备（显示终端）。

```
# 当重复的行不相邻时，通过sort排序后删除
$ sort testfile | uniq
```

### find

用于在指定目录下查找文件

```
find path -option [-print]   [-exec -ok command] {} \;
```

find 根据下列规则判断 path 和 expression，在命令列上第一个 - ( ) , ! 之前的部份为 path，之后的是 expression。如果 path 是空字串则使用目前路径，如果 expression 是空字串则使用 -print 为预设 expression。常用expression：

- -mount, -xdev : 只检查和指定目录在同一个文件系统下的文件，避免列出其它文件系统中的文件
- -amin n : 在过去 n 分钟内被读取过
- -anewer file : 比文件 file 更晚被读取过的文件
- -atime n : 在过去n天内被读取过的文件
- -cmin n : 在过去 n 分钟内被修改过
- -cnewer file :比文件 file 更新的文件
- -ctime n : 在过去n天内被修改过的文件
- -empty : 空的文件-gid n or -group name : gid 是 n 或是 group 名称是 name
- -ipath p, -path p : 路径名称符合 p 的文件，ipath 会忽略大小写
- -name name, -iname name : 文件名称符合 name 的文件。iname 会忽略大小写
- -size n : 文件大小 是 n 单位，b 代表 512 位元组的区块，c 表示字元数，k 表示 kilo bytes，w 是二个位元组。
- -type c : 文件类型是 c 的文件。
- d: 目录
- c: 字型装置文件
- b: 区块装置文件
- p: 具名贮列
- f: 一般文件
- l: 符号连结
- s: socket
- -pid n : process id 是 n 的文件

可以使用 ( ) 将运算式分隔，并使用下列运算。

```
exp1 -and exp2
! expr
-not expr
exp1 -or exp2
exp1, exp2
```

## 进程

### ps

ps命令用于显示当前进程 (process) 的状态。

```
ps [options] [--help]
```

- ps 的参数非常多, 在此仅列出几个常用的参数并大略介绍含义
- -A 列出所有的行程
- -w 显示加宽可以显示较多的资讯
- -au 显示较详细的资讯
- -aux 显示所有包含其他使用者的行程
- au(x) 输出格式 :
- USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
- USER: 行程拥有者
- PID: pid
- %CPU: 占用的 CPU 使用率
- %MEM: 占用的记忆体使用率
- VSZ: 占用的虚拟记忆体大小
- RSS: 占用的记忆体大小
- TTY: 终端的次要装置号码 (minor device number of tty)
- STAT: 该行程的状态:
- D: 无法中断的休眠状态 (通常 IO 的进程)
- R: 正在执行中
- S: 静止状态
- T: 暂停执行
- Z: 不存在但暂时无法消除
- W: 没有足够的记忆体分页可分配
- <: 高优先序的行程
- N: 低优先序的行程
- L: 有记忆体分页分配并锁在记忆体内 (实时系统或捱A I/O)
- START: 行程开始时间
- TIME: 执行的时间
- COMMAND:所执行的指令

### top

top命令用于实时显示 process 的动态

```
top [-] [d delay] [q] [c] [S] [s] [i] [n] [b]
```

- d : 改变显示的更新速度，或是在交谈式指令列( interactive command)按 s
- q : 没有任何延迟的显示速度，如果使用者是有 superuser 的权限，则 top 将会以最高的优先序执行
- c : 切换显示模式，共有两种模式，一是只显示执行档的名称，另一种是显示完整的路径与名称S : 累积模式，会将己完成或消失的子行程 ( dead child process ) 的 CPU time 累积起来
- s : 安全模式，将交谈式指令取消, 避免潜在的危机
- i : 不显示任何闲置 (idle) 或无用 (zombie) 的行程
- n : 更新的次数，完成后将会退出 top
- b : 批次档模式，搭配 "n" 参数一起使用，可以用来将 top 的结果输出到档案内

## 网络

### netstat

命令用于显示网络状态

```
netstat [-acCeFghilMnNoprstuvVwx][-A<网络类型>][--ip]
```

- -a或--all 显示所有连线中的Socket。
- -A<网络类型>或--<网络类型> 列出该网络类型连线中的相关地址。
- -c或--continuous 持续列出网络状态。
- -C或--cache 显示路由器配置的快取信息。
- -e或--extend 显示网络其他相关信息。
- -F或--fib 显示FIB。
- -g或--groups 显示多重广播功能群组组员名单。
- -h或--help 在线帮助。
- -i或--interfaces 显示网络界面信息表单。
- -l或--listening 显示监控中的服务器的Socket。
- -M或--masquerade 显示伪装的网络连线。
- -n或--numeric 直接使用IP地址，而不通过域名服务器。
- -N或--netlink或--symbolic 显示网络硬件外围设备的符号连接名称。
- -o或--timers 显示计时器。
- -p或--programs 显示正在使用Socket的程序识别码和程序名称。
- -r或--route 显示Routing Table。
- -s或--statistics 显示网络工作信息统计表。
- -t或--tcp 显示TCP传输协议的连线状况。
- -u或--udp 显示UDP传输协议的连线状况。
- -v或--verbose 显示指令执行过程。
- -V或--version 显示版本信息。
- -w或--raw 显示RAW传输协议的连线状况。
- -x或--unix 此参数的效果和指定"-A unix"参数相同。
- --ip或--inet 此参数的效果和指定"-A inet"参数相同。

### curl

用于请求web服务器

不带任何参数curl就是发出GET请求，服务器返回内容输出到命令行。

```
curl https://www.example.com
```

- -A 指定客户端用户代理标头`User-Agent`
  
  ```
  curl -A 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36' https://google.com
  ```
  
  移除`User-Agent`标头
  
  ```
  curl -A '' https://google.com
  ```
  
  -H 直接指定标头，更改`User-Agent`
  
  ```
  curl -H 'User-Agent: php/1.0' https://google.com
  ```
- -b 用来向服务器发送Cookie
  
  ```
  curl -b 'foo=bar' https://google.com
  ```
  
  发送多个Cookie
  
  ```
  curl -b 'foo1=bar;foo2=bar2' https://google.com
  ```
  
  读取本地`cookies.txt`
  
  ```
  curl -b cookies.txt https://www.google.com
  ```
- -c 将服务器设置的Cookie写入文件
  
  ```
  curl -c cookies txt https://www.google.com
  ```
- -d 发送POST请求数据体
  
  ```
  curl -d'login=emma＆password=123'-X POST https://google.com/login
  # 或者
  curl -d 'login=emma' -d 'password=123' -X POST  https://google.com/login
  ```
  
  读取本地文件数据，向服务器发送
  
  ```
  curl -d '@data.txt' https://google.com/login
  ```
- --data-urlencode 等同于-d，发送POST请求，区别在于自动将发送的数据进行URL编码，如空格符。
  
  ```
  curl --data-urlencode 'comment=hello world' https://google.com/login
  ```
- -e 用来设置HTTP的标头Referer，表示请求来源
  
  ```
  curl -e 'https://google.com?q=example' https://example.com
  ```
  
  -H 可直接添加标头Referer
  
  ```
  curl -H 'Referer: https://google.com?q=example' https://www.example.com
  ```
- -F 向服务器上传二进制文件
  
  ```
  curl -F 'file=@photo.png' https://google.com/profile
  ```
  
  指定文件类型
  
  ```
  curl -F 'file=@photo.png;type=image/png' https://google.com/profile
  ```
  
  指定文件名
  
  ```
  curl -F 'file=@photo.png;filename=me.png' https://google.com/profile
  ```
- -G 构造URL查询字符串，发出GET请求，实际请求地址为`https://google.com/search?q=kitties&count=20`省略-G会发出POST请求
  
  ```
  curl -G -d 'q=kitties' -d 'count=20' https://www.google.com/search
  ```
- -H 添加HTTP请求标头
  
  ```
  curl -H 'Accept-Language: en-US' https://google.com
  curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' https://google.com
  ```
- -i 打印服务器回应的HTTP标头
- -I 发出HEAD请求，返回HTTP标头，等同于--head
- -k 跳过ssl检测
- -L 让http请求跟随服务器重定向，curl默认不跟随
- --limit-rate 限制HTTP请求和回应的带宽，模拟慢网环境。
- -o 将服务器的响应保存成文件，等同于wget命令
- -O 响应保存成文件，并将URL的最后部分当做文件名。
- -s 不输出错误和进度信息
- -S 只输出错误信息
- -u 设置服务器认证用户名和密码
  
  ```
  curl -u 'bob:12345' https://google.com/login
  curl https://bob:12345@google.com/login
  curl -u 'bob' https://google.com/login
  ```
- -v 输出通信整个过程，用于调试，--trace调试输出二进制数据
- -x 指定http请求的代理
  
  ```
  curl -x socks5://james:cats@myproxy.com:8080 https://www.example.com
  ```
- -X 指定HTTP请求方法