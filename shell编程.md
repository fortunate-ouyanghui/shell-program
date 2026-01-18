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
