# shell编程
## 变量
```C
数据类型:shell中只有string字符串类型
变量：全局变量 -- 环境变量
     局部变量 -- 本地变量
     示例：VAR=10(注意：没有空格)
env用法：
env  (列出shell中所有环境变量)

export用法：
VAR=10
export VAR (将VAR设置为环境变量即全局变量)

unset VAR （删除变量）
```
# 代换
```C
ls *.txt （查找所有以.txt结尾的文件）
ls ????.txt  (查找文件名为四个字母且以.txt结尾的文件)
ls [a-z].txt （查找a到z任意一个字母且以.txt结尾的文件。了注意：只匹配一个）
ls [xyaz].txt (查找xyaz任意一个字母命名且以.txt结尾的文件)
ls [a-z][0-9].txt (查找第一个为任意一个字母，第二个为0到9任意一个数字且以.txt结尾的文件)

命令代换
${变量名}    取变量的值（更安全）
$(命令)     取命令执行结果
$变量名     取变量的值

算术代换
$((变量名))或$[变量名]  对变量执行算术运算
例如：
VAR=10
echo $((VAR+5))或echo $[VAR+5]   (结果为15)
echo $[8#10+3]  (结果为11,表示8进制的10+3即11)

转义：
VAR=10
echo \$VAR   (不再是打印变量的值，而是将$认为是字符，打印结果为$VAR)

单引号：括字符串 （不能展开变量）
双引号：括字符串（可以将变量展开）
echo "$VAR"   (打印10)
echo '$VAR'   (打印$VAR)
```
## 条件测试
```C
条件判别表达式：真0  假1
整数判别符：-eq 等于
       -gt 大于
       -lt 小于
       -ge 大于等于
       -le 小于等于
文件类型判别符：
          -d 目录文件
          -f 普通文件
          -p 管道
          -l 软连接
          -c 字符设备vei
          -b 块设备
          -s socket
字符长度判断：
          -z 空字符串（长度为0）
          -n 非空字符串（长度非0）
字符相等判断：
          = 相等
          ！= 不相等
逻辑运算：
          a 逻辑与
          o 逻辑或
          ！逻辑非
例：
var=10
test $var -gt 8 或者[ $var -gt 8 ](判断var是否大于8,真返回0,假返回1)
echo $?  (打印上一条命令执行的结果)

[ -d user/src ] (判断user/src是否是目录，如果是则返回0,不是返回1)
[ "abc" = "acb" ] (判断abc是否与acb相等，如果相等返回0,反之)

var=abc
[ -z $var ] (判断var是否是非空字符串，如果是返回0,反之)


[ -d desk -a “$var” = 'abc' ] (相当于 desk是否是目录 && var是否等于abc，只有当两个都为真时，才返回0,反之)
```
## if分支语句
```C
if语句：
     if [判别条件];then
          执行内容
     elif [判别条件]
          执行内容
     else
          执行内容
     fi
例如：
if [ -f test.txt ];then
     echo 'it is a file'
elif [-d test.txt ];then
     echo 'it is a directory'
else
     echo 'it is no file and dir'
(判断test.txt是否是文件，是否是目录，执行响应内容)
```
## case分支
```C
#! /bin/bash
echo '请输入你要判决的文件名：'
read file_name
case "$file_name" in
testfile|file)
     echo 'it is a file';;
testdir|dir)
     echo 'it is a dir';;
*）
     echo 'it is not dir and file';;
esac
     
```
## 循环
```C
for循环：
for file_name in `ls`;do
	echo ${file_name}     这里会打印ls命令执行的结果（test test.cpp dir）
done


while循环：
echo '请输入密码：'
read mypwd
while [ "${mypwd}" !=  "123" ];do
     echo '密码错误,请重新输入'
     read mypwd

     if [ ${mypwd} = "break" ];then
		break
	fi
done
```
## 命令行参数
```C
$0 （相当于argv[0]）
$1 （相当于argv[1],其余同理）
$#  (相当于argc-1，表示减去第一个参数该参数为文件名称)
$@或$*  (表示参数列表$1 $2 $3 ....)
$?  (上一条命令的执行状态)
$$  (当前进程号)


echo "hello\n\n\n"     (不会打印3个换行符)
echo -e "hello\n\n\n"  (会打印3个换行符)

echo "hello"		   (会自动添加回车)
echo -n "hello"        (不添加回车)
```
## 重定向
```C
date > file (将date命令执行的结果以覆盖的方式放入file中)
date >> file (将date命令执行的结果以追加的方式放入file中)

0:标准输入
1:标准输出
2：标准出错
rm ttt.txt > file 2 > &1 (将rm ttt.txt执行的结果放入file，如果命令出错了也把出错信息放入file中)
```
## 函数
```C
定义：
函数名(){
	函数内容;
}

调用：
函数名 参数1 参数2 参数3


函数外：
$0:表示命令行参数argv[0]
$1-$N:表示命令行参数argv[1]-argv[N]
函数内：
$0:表示命令行参数argv[0];
$0-$N:表示参数1-参数N

例子：
my_function(){
	echo "$0";
	echo "$1";
	echo "$2";
	return 1；#注意：return后面只能跟数字
}

my_function aa bb
echo "函数的返回值：$?"

```
## shell调试
```C
sh -x ./test.sh
使用-x调试shell文件，以后每执行一个指令都会打印出来。

set -x启用调试，set +x禁用调试
例如文件：
#！ /bin/bash 

my_func(){
	set -x#启用调试
	echo "$1"
	set +x#禁用调试
	echo "$2"
	echo "$3"
}

my_func aa bb cc

```
