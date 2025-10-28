# Shell命令

## 命令基础












## 常用命令
### 文件与目录操作
- **ls**
	- 功能：显示指定目录的文件和子目录Shell。
	- 选项：
		- `-l`：长格式显示（包括权限、所有者、大小、修改时间）。
		- `-a`：显示隐藏文件（以`.`开头）。
		- `-h`：人性化显示文件大小（如KB、MB，需配合`-l`）。
		- `-t`：按修改时间排序。
		- `-r`：逆序排列。
	- 示例：
		- `ls -lah /home`：显示`/home`下所有文件的详细列表。
		- `ls -lt /var/log`：按时间倒序显示日志文件。
- **cd**
	- 功能：更改当前工作目录。
	- 用法：
		- `cd /path/to/dir`：进入指定目录。
		- `cd ..`：返回上一级目录。
		- `cd -`：返回上一次所在目录。
		- `cd ~`：进入用户家目录。
		- `cd`：不带参数，返回家目录。
	- 示例：
		- `cd /etc`：进入`/etc`目录。
		- `cd ../logs`：进入上一级的`logs`目录。
- **pwd**
	- 功能：输出当前所在目录的绝对路径。
- **cp**
	- 功能：复制文件或目录到指定位置。
	- 选项：
		- `-r`：递归复制目录及其内容。
		- `-p`：保留文件属性（如权限、时间戳）。
		- `-i`：交互式，覆盖前提示。
		- `-v`：显示复制过程。
	- 示例：
		- `cp file.txt /backup/`：复制`file.txt`到`/backup`。
		- `cp -r /src /dst`：递归复制`src`目录到`dst`。
- **mv**
	- 功能：移动文件/目录或重命名。
	- 选项：
		- `-i`：交互式，覆盖前提示。
		- `-f`：强制覆盖。
		- `-v`：显示移动过程。
	- 示例：
		- `mv file.txt /new/path/`：移动文件到新路径。
		- `mv oldname.txt newname.txt`：重命名文件。
- **rm**
	- 功能：删除指定文件或目录。
	- 选项：
		- `-r`：递归删除目录及其内容。
		- `-f`：强制删除，不提示。
		- `-i`：交互式，逐个确认。
	- 示例：
		- `rm file.txt`：删除单个文件。
		- `rm -rf /tmp/test`：删除`test`目录及内容（谨慎使用）。
	- 注意：`rm -rf`具破坏性，无恢复可能，建议先用`echo rm -rf *`预览。
- **mkdir**
	- 功能：创建新目录。
	- 选项：
		- `-p`：创建多级目录，存在时不报错。
	- 示例：
		- `mkdir project`：创建`project`目录。
		- `mkdir -p /data/logs/2025`：创建多级目录。
- **rmdir**
	- 功能：删除空的目录。
- **touch**
	- 功能：创建空文件或更新文件的访问/修改时间。
	- 示例：
		- `touch newfile.txt`：创建空文件。
		- `touch -t 202501011200 file.txt`：将文件时间设为2025年1月1日12:00。
- **ln**
	- 功能：创建硬链接或软链接。
	- 选项：
		- `-s`：创建软链接（符号链接，类似快捷方式）。
	- 示例：
		- `ln -s /etc/config.conf config.ln`：创建软链接。
		- `ln file.txt file.hard`：创建硬链接。
### 文件查看与编辑
- **cat**
	- 功能：输出文件内容到终端或连接多个文件。
	- 选项：
		- `-n`：显示行号。
		- `-E`：显示行尾$符号。
	- 示例：
		- `cat /etc/passwd`：显示用户账户信息。
		- `cat file1.txt file2.txt > combined.txt`：合并文件。
- **less**
	- 功能：分页显示文件内容，适合查看大文件。
	- 操作：
		- 上下箭头：滚动。
		- `/pattern`：搜索。
		- `q`：退出。
	- 示例：
		- `less /var/log/syslog`：分页查看系统日志。
- **more**
	- 功能：类似`less`，逐页显示文件内容。
	- 操作：空格翻页，`q`退出。
	- 示例：
		- `more largefile.txt`。
- **head**
	- 功能：显示文件前几行（默认10行）。
	- 选项：
		- `-n N`：显示前N行。
	- 示例：
		- `head -n 5 access.log`：显示日志前5行。
- **tail**
	- 功能：显示文件末尾几行（默认10行）。
	- 选项：
		- `-n N`：显示末尾N行。
		- `-f`：实时监控文件变化。
	- 示例：
		- `tail -f /var/log/syslog`：实时查看系统日志。
		- `tail -n 10 error.log`：显示最后10行。
- **nano**
	- 功能：编辑文本文件，适合初学者。
	- 操作：
		- `Ctrl+O`：保存。
		- `Ctrl+X`：退出。
		- `Ctrl+G`：帮助。
	- 示例：
		- `nano config.txt`：编辑配置文件。
- **vim/vi**
	- 功能：支持多种模式，适合高级用户。
	- 模式：
		- 命令模式：移动光标（`h`左、`j`下、`k`上、`l`右）、删除（`dd`）、复制（`yy`）。
		- 插入模式：按`i`进入，编辑文本。
		- 底行模式：输入`:`执行命令（如`:wq`保存退出，`:q!`强制退出）。
	- 示例：
		- `vim script.sh`：编辑Shell脚本。
### 文本处理
- **grep**
	- 功能：在文件中搜索匹配指定模式的行。
	- 选项：
		- `-i`：忽略大小写。
		- `-r`：递归搜索目录。
		- `-n`：显示行号。
		- `-v`：显示不匹配的行。
		- `-w`：匹配整个单词。
	- 示例：
		- `grep -rin "error" /var/log`：递归查找日志中包含“error”的行。
		- `grep -v "test" file.txt`：显示不含“test”的行。
- **awk**
	- 功能：按字段解析和处理文本，适合结构化数据。
	- 选项：
		- `-F`：指定分隔符。
		- `$1`、`$2`：表示第几列。
	- 示例：
		- `awk -F: '{print $1}' /etc/passwd`：打印用户名字段。
		- `awk '{sum+=$1} END {print sum}' numbers.txt`：计算第一列总和。
- **sed**
	- 功能：修改、替换或删除文本流。
	- 选项：
		- `-i`：直接修改文件。
		- `-e`：执行多个编辑命令。
	- 示例：
		- `sed 's/error/warning/g' log.txt`：全局替换“error”为“warning”。
		- `sed -i '/^#/d' config.conf`：删除配置文件中以`#`开头的行。
- **cut**
	- 功能：按分隔符提取文本的特定字段。
	- 选项：
		- `-d`：指定分隔符。
		- `-f`：选择字段。
	- 示例：
		- `cut -d: -f1 /etc/passwd`：提取用户名字段。
- **sort**
	- 功能：对文件内容按行排序。
	- 选项：
		- `-n`：按数字排序。
		- `-r`：逆序。
		- `-k`：按指定列排序。
	- 示例：
		- `sort -n numbers.txt`：按数字升序排序。
		- `sort -k2 -t: file.txt`：按第二列排序（分隔符为:）。
- **uniq**
	- 功能：删除重复行（需先排序）。
	- 选项：
		- `-c`：显示重复次数。
		- `-d`：仅显示重复行。
	- 示例：
		- `sort file.txt | uniq -c`：统计每行出现次数。
- **wc**
	- 功能：统计行数、字数、字符数。
	- 选项：
		- `-l`：行数。
		- `-w`：字数。
		- `-c`：字符数。
	- 示例：
		- `wc -l script.sh`：统计脚本行数。
### 系统监控
- **top**
	- 功能：动态显示系统进程、CPU、内存使用情况。
	- 操作：
		- `q`：退出。
		- `k`：杀死进程（输入PID）。
		- `1`：显示各CPU核心使用率。
	- 示例：
		- `top`：启动监控。
- **htop**
	- 功能：更友好的进程查看工具（需安装）。
	- 操作：支持鼠标操作，上下箭头选择，`F9`杀死进程。
	- 示例：
		- `htop`。
- **ps**
	- 功能：显示当前进程状态。
	- 选项：
		- `aux`：显示所有用户进程。
		- `-ef`：显示详细进程信息。
	- 示例：
		- `ps aux | grep nginx`：查找nginx进程。
- **free**
	- 功能：显示系统内存和交换空间使用情况。
	- 选项：
		- `-h`：人性化显示。
		- `-s N`：每N秒刷新。
	- 示例：
		- `free -h`：显示内存使用。
- **df**
	- 功能：显示文件系统磁盘使用情况。
	- 选项：
		- `-h`：人性化显示。
		- `-T`：显示文件系统类型。
	- 示例：
		- `df -h /`：查看根分区使用情况。
- **du**
	- 功能：统计文件或目录的磁盘占用。
	- 选项：
		- `-h`：人性化显示。
		- `-s`：汇总大小。
	- 示例：
		- `du -sh /home/user`：显示用户目录总大小。
- **iostat**
	- 功能：显示CPU和磁盘I/O统计。
	- 选项：
		- `-x`：详细统计。
		- `N`：每N秒刷新。
	- 示例：
		- `iostat -x 1`：每秒更新I/O统计。
- **vmstat**
	- 功能：显示内存、CPU、I/O等统计。
	- 示例：
		- `vmstat 1`：每秒更新。
### 进程管理
- **kill**
	- 功能：向进程发送信号（默认SIGTERM）。
	- 信号：
		- `-9`（SIGKILL）：强制终止。
		- `-15（`SIGTERM）：优雅终止。
	- 示例：
		- `kill -9 1234`：强制终止PID为1234的进程。
- **killall**
	- 功能：按进程名终止所有匹配进程。
	- 示例：
		- `killall nginx`：终止所有nginx进程。
- **pkill**
	- 功能：按进程名或模式终止进程。
	- 示例：
		- `pkill -u user`：终止用户user的进程。
- **nohup**
	- 功能：忽略挂起信号运行命令。
	- 示例：
		- `nohup python script.py &`：后台运行Python脚本。
- **jobs**
	- 功能：列出当前Shell的后台任务。
	- 示例：
		- `jobs`：显示后台任务列表。
- **fg**
	- 功能：将后台任务切换到前台。
	- 示例：
		- `fg %1`：切换任务编号1到前台。
- **bg**
	- 功能：继续运行暂停的后台任务。
	- 示例：
		- `bg %1`。
### 网络管理
- **ping**
	- 功能：发送ICMP包测试网络连通性。
	- 选项：
		- `-c N`：发送N次ping。
		- `-i N`：设置发送间隔（秒）。
	- 示例：
		- `ping -c 4 google.com`：ping 4次。
- **curl**
	- 功能：获取网页内容或发送HTTP请求。
	- 选项：
		- `-o`：保存输出到文件。
		- `-I`：仅显示头信息。
	- 示例：
		- `curl -o page.html https://example.com`：下载网页。
- **wget**
	- 功能：从网络下载文件。
	- 选项：
		- `-O`：指定输出文件名。
		- `-c`：断点续传。
	- 示例：
		- `wget https://example.com/file.zip`。
- **netstat**
	- 功能：显示网络连接、路由表、端口等。
	- 选项：
		- `-tuln`：显示TCP/UDP监听端口。
		- `-r`：显示路由表。
	- 示例：
		- `netstat -tuln`：列出监听端口。
- **ss**
	- 功能：显示网络连接信息，替代`netstat`。
	- 示例：
		- `ss -tuln`：显示监听端口。
- **tcpdump**
	- 功能：捕获网络流量。
	- 选项：
		- `-i`：指定网卡。
		- `-w`：保存到文件。
	- 示例：
		- `tcpdump -i eth0 port 80`：捕获80端口流量。
- **ip**
	- 功能：配置和查看网络接口、路由。
	- 示例：
		- `ip addr`：查看网络接口。
		- `ip route`：查看路由表。
### 包管理
### 文件权限与查找
- **chmod**
	- 功能：更改文件或目录的权限。
	- 权限：
		- 数字：`r=4`、`w=2`、`x=1`（如`755`表示`rwxr-xr-x`）。
		- 符号：`u`（用户）、`g`（组）、`o`（其他）。
	- 示例：
		- `chmod 644 file.txt`：设置文件为`rw-r--r--`。
		- `chmod u+x script.sh`：为用户添加执行权限。
- **chown**
	- 功能：更改文件或目录的所有者和所属组。
	- 选项：
		- `-R`：递归更改。
	- 示例：
		- `chown user:group file.txt`：更改所有者和组。
		- `chown -R user /data`：递归更改目录。
- **find**
	- 功能：根据条件在目标目录及后代目录中查找文件。
	- 选项：
		- `-name`：按文件名查找。
		- `-type`：文件类型（`f`文件、`d`目录）。
		- `-size`：按大小查找。
		- `-exec`：对找到的文件执行命令。
	- 示例：
		- `find / -name "*.log"`：查找所有`.log`文件。
		- `find /home -type f -size +100M`：查找大于100MB的文件。
- **locate**
	- 功能：基于数据库快速查找文件。
	- 示例：
		- `locate config.conf`：查找包含`“config.conf”`的文件。
		- `sudo updatedb`：更新数据库。






# Shell编程
## 概述
### 简介
- Shell 脚本是一种使用 Shell 解释器（如 Bash）编写的程序，用于自动化执行一系列命令。
- Shell 脚本可以将多个命令组合成一个可执行文件，实现复杂的任务自动化、系统管理、数据处理等功能。
- Shell 脚本的本质是文本文件，其中包含 Shell 命令、控制结构和逻辑判断。
### 需求
- **自动化**：简化重复性任务，如备份文件、监控系统。
- **便携性**：在 Linux/Unix 系统上广泛兼容。
- **效率**：快速开发，无需编译，直接执行。
- **集成**：轻松调用系统命令和工具（如 grep、sed、awk）。
- **适用场景**：系统运维、DevOps、数据 ETL 处理、批量文件操作等。
### 类型
- **Bash 脚本**：最常见，使用 `#!/bin/bash`。
- **其他**：如 sh、ksh、zsh 脚本，根据需求选择。
## 基础
1. 创建 Shell 脚本
	1. 使用文本编辑器（如 vim、nano）创建文件，例如 script.sh。
	2. 第一行添加 Shebang：`#!/bin/bash` 或者 `#!/usr/bin/env bash`（更具通用性），指定解释器路径（可以通过 which bash 查看）。
	3. 添加脚本内容。
	4. 保存文件。
	- 示例
		```shell
		#!/bin/bash
		echo "Hello, World!"
		```
2. 赋予执行权限
	- 使用 `chmod +x script.sh` 使脚本可执行。
	- 权限检查：`ls -l script.sh` 应显示 `-rwxr-xr-x`。
3. 执行脚本
	- **直接执行**：`./script.sh`（需在当前目录）。
	- **通过解释器**：`bash script.sh`（无需执行权限）。
	- **源执行**：`source script.sh` 或 `. script.sh`（在当前 Shell 中执行，变量会保留）。
	- **传递参数**：`./script.sh arg1 arg2`。
4. 脚本注释
	- 以 `#` 开头忽略该行内容。
	- 示例：`# 这是一行注释`。
## 变量
### 变量定义
- **语法**：`variable_name=value`（等号两边无空格）。
- **规则**：
	- 变量名由字母、数字、下划线组成，首字符不能是数字。
	- 区分大小写：`Var` 和 `var` 是不同变量。
	- 避免使用保留关键字（如 `if`, `while`）。
- **示例**：
	```shell
	name="Alice"
	count=42
	```
### 变量使用
- **访问变量**：`$variable_name` 或 `${variable_name}`。
- **使用** `${}` **的场景**：
	- 避免歧义：`echo ${name}_suffix`（防止 `name_suffix` 被误解）。
	- 复杂操作：如 `${variable:-default}`。
- **示例**：
	```shell
	echo $name  # 输出：Alice
	echo ${name}_test  # 输出：Alice_test
	```
### 变量类型
- **本地变量**：
	- 仅在当前 Shell 或脚本有效。
	- 示例：`temp="temporary`"。
- **环境变量**：
	- 通过 `export` 导出，子进程可访问。
	- 示例：`export PATH="$PATH:/new/path`"。
- **只读变量**：
	- 使用 `readonly` 或 `declare -r` 定义，不可修改。
	- 示例：`readonly version="1.0"`。
- **Shell 内置变量**：
	- `HOME`：用户主目录。
	- `PATH`：命令搜索路径。
	- `USER`：当前用户名。
	- 查看：`env` 或 `set`。
### 变量作用域
- **本地作用域**：在函数内定义，使用 `local` 限制。
- **全局作用域**：脚本中直接定义，影响整个脚本。
- **环境作用域**：导出后，子 Shell/进程可访问。
- **示例**：
	```shell
	global_var="I'm global"
	my_function() {
	  local local_var="I'm local"
	  echo $local_var
	  echo $global_var
	}
	my_function
	echo $global_var  # 输出：I'm global
	echo $local_var   # 无输出
	```
### 变量默认值
- **默认值**：`${variable:-default}`，变量未定义时使用默认值。
- **赋值默认值**：`${variable:=default}`，未定义时赋值并使用。
- **检查定义**：`${variable:+value}`，定义时返回 value。
- **错误提示**：`${variable:?"Error message"}`，未定义时报错。
- **示例**：
	```shell
	unset var
	echo ${var:-default}  # 输出：default
	echo ${var:=value}    # 输出：value，var 被赋值为 value
	echo ${var:+set}      # 输出：set
	```
### 特殊变量
- `$0`：脚本文件名。
- `$1`, `$2`, ...：位置参数（脚本传入的参数）。
- `$#`：参数个数。
- `$@`：所有参数（每个参数独立）。
- `$*`：所有参数（作为一个字符串）。
- `$?`：上一命令的退出状态（0 表示成功）。
- `$$`：当前脚本的进程 ID。
- 示例：
	```shell
	echo "Script name: $0"
	echo "First argument: $1"
	echo "Number of arguments: $#"
	```
## 参数
### 位置参数
- **位置参数**：脚本调用时传入，如 `./script.sh param1 param2`。
- **shift**：移动参数位置（shift 使 `$2` 变为 `$1`）。
- **getopts**：处理选项参数（如 `-a -b value`）。 
- 示例：
	```shell
	while getopts "a:b:" opt; do
	  case $opt in
	    a) echo "Option a: $OPTARG" ;;
	    b) echo "Option b: $OPTARG" ;;
	  esac
	done
	```
## 运算符
### 算术运算符
- **语法**
	- **双括号结构**：`((expression))` 用于算术计算，支持 C 风格操作符，无需 `$` 调用变量，会自动将字面两识别为变量。
	- **let 命令**：`let expression`，等价于 `(())`。
	- **expr 命令**：`expr expression`，较旧，但兼容性好（需注意空格）。
	- **bc 命令**：处理浮点和高级数学。
	- `$[]`：`$[expression]`，已过时，推荐用 `$(())`，用于计算表达式并将结果作为字符串赋值给变量，常用于赋值操作。
- **操作符**
	- 基本运算
		- `+`：加法
		- `-`：减法
		- `*`：乘法
		- `/`：除法（整数除法，向下取整）
		- `%`：取模
	- 高级运算
		- `**`：幂运算（Bash 特定）
		- `++`、`--`：自增、自减（前置/后置）
	- 位运算
		- `&`：按位与
		- `|`：按位或
		- `^`：按位异或
		- `~`：按位取反
		- `<<`、`>>`：左移、右移
- **示例**
	```shell
	# 基本运算
	a=10
	b=3
	((result = a + b))
	echo $result  # 输出：13
	
	let sum=a+b
	echo $sum  # 输出：13
	
	product=$(expr $a \* $b)  # 注意转义 *
	echo $product  # 输出：30
	
	# 高级运算
	((a++))
	echo $a  # 输出：11
	
	((power = 2 ** 3))
	echo $power  # 输出：8
	
	# 浮点运算
	result=$(echo "scale=2; 10 / 3" | bc)
	echo $result  # 输出：3.33
	```
### 字符串操作
- **基础**
	- Shell 变量默认字符串，无需类型声明。
	- 引号：单引号（字面量，换行符、`$`等符号不会解析）、双引号（变量扩展，换行符、`$` 等符号会被解析）。
	- 使用 `tr`、`sed` 等外部命令辅助。
- **运算符**
	- **长度**：`${#string}` 获取长度。
	- **连接**：直接拼接 `str1$str2` 或 `str1="$str1 $str2"`。
	- **子串提取**：`${string:position:length}`。
	- **删除子串**：
		- `${string#pattern}`：从头删除最短匹配。
		- `${string##pattern}`：从头删除最长匹配。
		- `${string%pattern}`：从尾删除最短匹配。
		- `${string%%pattern}`：从尾删除最长匹配。
	- **替换**：
		- `${string/old/new}`：替换第一个匹配。
		- `${string//old/new}`：替换所有匹配。
- **示例**
	```shell
	# 长度与连接
	str="Hello World"
	echo ${#str}  # 输出：11
	
	str2="!"
	combined="$str$str2"
	echo $combined  # 输出：Hello World!
	
	# 字串与删除
	echo ${str:6:5}  # 输出：World
	
	file="example.tar.gz"
	echo ${file#*.}  # 输出：tar.gz
	echo ${file##*.}  # 输出：gz
	echo ${file%.*}  # 输出：example.tar
	
	# 替换
	echo ${str/World/Universe}  # 输出：Hello Universe
	echo ${str//l/L}  # 输出：HeLLo WorLd
	```
### 条件表达式
- **语法**
	- **单方括号** `[]`：传统测试，空格与内容之间有空格。
	- **双方括号** `[[]]`：Bash 扩展，支持正则、逻辑运算符。
	- **test 命令**：等价于 `[]`。
- **运算符**
	- 数值比较
		- `-eq`：等于
		- `-ne`：不等于
		- `-gt`：大于
		- `-ge`：大于等于
		- `-lt`：小于
		- `-le`：小于等于
	- 字符串比较
		- `=` 或 `==`：等于
		- `!=`：不等于
		- `-z`：空字符串
		- `-n`：非空
		- `<`、`>`：字典序比较（`[[]]` 中）
	- 文件测试
		- `-e`：存在
		- `-f`：普通文件
		- `-d`：目录
		- `-r`：可读
		- `-w`：可写
		- `-x`：可执行
		- `-s`：非空文件
	- 逻辑运算符
		- `&&`：逻辑与
		- `||`：逻辑或
		- `!`：逻辑非
- **示例**
	```shell
	# 数值比较
	if [ $a -gt $b ]; then
	  echo "$a > $b"
	fi
	
	# 字符串比较
	if [[ $str == "Hello World" ]]; then
	  echo "Match"
	fi
	
	if [ -z "$var" ]; then
	  echo "Empty"
	fi
	
	# 文件测试
	if [ -f "file.txt" ]; then
	  echo "File exists"
	fi
	
	# 逻辑运算符
	if [[ $a -gt 5 && $b -lt 10 ]]; then
	  echo "Condition met"
	fi
	
	# 正则表达式
	if [[ $str =~ ^Hello ]]; then
	  echo "Starts with Hello"
	fi
	```
## 控制流
### 条件语句
- **if语句**
	- **作用**：根据条件执行代码块，支持单分支、多分支和默认分支。
	- **语法**：
		```shell
		if [ 条件 ]; then
		  # 条件为真时执行的命令
		elif [ 条件 ]; then
		  # 可选，另一个条件为真时执行
		else
		  # 可选，所有条件为假时执行
		fi
		```
	- **示例**：
		```shell
		#!/bin/bash
		file="example.txt"
		if [ -f "$file" ]; then
		  echo "文件 $file 存在。"
		elif [ -d "$file" ]; then
		  echo " $file 是一个目录。"
		else
		  echo "文件 $file 不存在。"
		fi
		```
- **case语句** 
	- **作用**：多路分支选择，根据变量值匹配模式执行代码。
	- **语法**：
		```shell
		case $变量 in
		  模式1)
		    # 匹配模式1时执行
		    ;;case语句
		  模式2)
		    # 匹配模式2时执行
		    ;;
		  *)
		    # 默认匹配（可选）
		    ;;
		esac
		```
	- **示例**：
		```shell
		#!/bin/bash
		read -p "输入水果名称: " fruit
		case $fruit in
		  apple|Apple)
		    echo "这是一个苹果。"
		    ;;
		  banana)
		    echo "这是一个香蕉。"
		    ;;
		  *)
		    echo "未知水果。"
		    ;;
		esac
		```
### 循环语句
- **for循环**
	- **作用**：遍历列表、数组或命令输出，重复执行代码。
	- **语法**：
		```shell
		for 变量 in 列表; do
		  # 命令
		done
		```
	- **列表来源**：
		- 静态：`for i in 1 2 3; do ...`
		- 动态：`for file in *.txt; do ...`（通配符）。
		- 命令替换：`for line in $(cat file.txt); do ...`
	- **示例**：
		```shell
		#!/bin/bash
		for file in /path/to/dir/*; do
		  if [ -f "$file" ]; then
		    echo "处理文件: $file"
		  fi
		done
		```
- **while循环**
	- **作用**：条件为真时重复执行。
	- **语法**：
		```shell
		while [ 条件 ]; do
		  # 命令
		done
		```
	- **示例**：
		```shell
		#!/bin/bash
		count=1
		while [ $count -le 5 ]; do
		  echo "计数: $count"
		  ((count++))
		done
		```
- **until循环**
	- **作用**：条件为假时重复执行（与 while 相反）。
	- **语法**：
		```shell
		until [ 条件 ]; do
		  # 命令
		done
		```
	- **示例**：
		```shell
		#!/bin/bash
		until [ -f "done.txt" ]; do
		  echo "等待文件..."
		  sleep 1
		done
		echo "文件已存在。"
		```
### 跳转语句
- **break语句**
	- **作用**：跳出当前循环（或多层循环）。
	- **语法**：break [n]（n 为跳出层数，默认 1）。
	- **示例**：
		```shell
		for i in {1..10}; do
		  if [ $i -eq 5 ]; then
		    break
		  fi
		  echo $i
		done  # 输出 1 2 3 4
		```
- **continue语句**
	- **作用**：跳过当前迭代，继续下一次循环。
	- **语法**：continue [n]（n 为层数）。
	- **示例**：
		```shell
		for i in {1..5}; do
		  if [ $i -eq 3 ]; then
		    continue
		  fi
		  echo $i
		done  # 输出 1 2 4 5
		```
## 函数
### 函数定义
- **方式一**
	```shell
	function 函数名() {
	  # 函数体：命令序列
	}
	```
- **方式二**
	```shell
	函数名() {
	  # 函数体
	}
	```
- **规则**
	- 函数名：字母、数字、下划线组成，首字符非数字。
	- 函数体：任意 Shell 命令，包括变量、循环、条件等。
	- 无参数列表：参数通过位置参数传入。
- **示例**：
	```shell
	#!/bin/bash
	
	function greet() {
	  echo "Hello, World!"
	}
	
	function check_even() {
	  if (( $1 % 2 == 0 )); then
	    echo "$1 是偶数。"
	  else
	    echo "$1 是奇数。"
	  fi
	}
	```
### 函数调用
- 调用：
	- **直接调用**：函数名 [参数1 参数2 ...]
	- **特点**：
		- 参数以空格分隔。
		- 调用时不需括号。
		- 函数执行后，返回到调用点继续。
- 示例：
	```shell
	greet  # 输出：Hello, World!
	
	check_even 4  # 输出：4 是偶数。
	```


### 函数参数




### 函数返回值





- 定义函数
	- 语法：
		```shell
		function_name() {
		  commands
		}
		# 或者
		function function_name {
		  commands
		}
		```
- 调用函数
	- `function_name arg1 arg2`。
	- 函数内使用 `$1`, `$2` 等访问参数。
- 返回值
	- 使用 return value 返回退出码。
	- 或通过全局变量/echo 输出。
	- 示例：
		```shell
		add() {
		  sum=$(( $1 + $2 ))
		  echo $sum
		}
		result=$(add 3 4)
		echo $result  # 输出：7
		```
- 局部变量
	- 使用 `local var=value` 定义函数内变量。
## 输入输出
- 输出
	- echo：打印文本，支持 -n (无换行), -e (解释转义)。
	- printf：格式化输出，如 printf "%s\n" "Hello".
- 输入
	- read：从标准输入读取。 
	- 一般情况下，read 命令作为从键盘获取输入，但在管道 (`|`) 或重定向 (`<`) 的场景下，`read` 的输入源不再是键盘，而是来自标准输入 (stdin)
	- 示例：
		```shell
		read -p "Enter name: " name
		echo "Name: $name"
		```
	- 选项：-s (静默输入密码), -t (超时)。
- 重定向
	- >, >>, <, 2>, 等，如脚本中重定向日志。
## 数组
- 定义数组
	- `array=(item1 item2 item3)` 或 `array[0]=item1`
- 访问数组
	- `${array[index]}`：单个元素。
	- `${array[@]}`：所有元素。
	- `${#array[@]}`：长度。
- 操作数组
	- 添加：`array+=("new")`
	- 删除：`unset array[index]`
	- 示例：
		```shell
		fruits=("apple" "banana" "cherry")
		echo ${fruits[1]}  # 输出：banana
		for fruit in "${fruits[@]}"; do
		  echo $fruit>>>>
		done
		```
## 调试与错误处理
### 调试处理
- bash -x script.sh：显示每条命令执行。
- set -x / set +x：脚本中启用/禁用。
- set -e：命令失败时退出脚本。
- set -u：未定义变量时报错。
### 错误处理
- 检查 $? 判断成功。
- trap：捕获信号，如 trap "echo Exiting" EXIT。
- 示例：错误日志
	```shell
	command || echo "Error occurred" >&2
	```
## 高级主题
- 信号处理
	- 使用 trap 处理 SIGINT (Ctrl+C) 等。
- 子 Shell 与命令替换
	- $(command)：替换命令输出。
	- (commands)：在子 Shell 执行。
- Here Document
	- 用于多行输入：
- 正则表达式与文本处理
	- 集成 grep、sed、awk 在脚本中。

shell 中引号不会变成字符串，只是确保引号中内容为一个整体
