---
title: shell编程小结
category: [toolswiki, linux]
sidebar:
    nav: TOOL
---

(更新中)

## 一、变量

### 1. 基础命令

```shell
#!/bin/sh

NAME="Zara Ali" # 定义变量
echo $NAME # 打印变量
echo "Introduce variables ${NAME} in the string." #在字符串中引入变量
unset NAME # 删除变量
```

### 2. 特殊变量

| 变量 | 描述                                                         |
| ---- | :----------------------------------------------------------- |
| $0   | 该脚本的文件名。                                             |
| $n   | （n不等于0）传递给脚本或函数的参数。例如，\$1 和 \$2分别表示第一个参数和第二个参数。 |
| $#   | 传递给脚本或函数的参数数量。                                 |
| $*   | 传递给脚本或函数的所有参数。以"\$1" "\$2" … "\$n" 的形式输出所有参数。 |
| $@   | 同\$*。<br />当通过双引号使用变量时（即"\$@"或"\$\*"），"\$\*" 会将所有的参数作为一个整体，以"\$1 \$2 … \$n"的形式输出所有参数；"\$@" 会将各个参数分开，以"\$1" "\$2" … "\$n" 的形式输出所有参数。 |
| $$   | 当前Shell进程ID。对于 Shell 脚本，就是这些脚本所在的进程ID。 |

#### （1）脚本传参示例

现有如下`test.sh`脚本：

```shell
#!/bin/sh

for TOKEN in $*
do
   echo $TOKEN
done
```

执行以下命令，外部参数以空格进行区分：

```shell
$./test.sh Zara Ali 10 Years Old
```

打印以下内容：

```shell
Zara
Ali
10
Years
Old
```

## 二、条件语句

#### （1）语句格式：

```shell
if [ bool表达式1 ]
then
		命令1
elif [ bool表达式2 ]
		命令2
else
		命令3
fi
```

注意：a）方括号和bool表达式之间必须有空格；b）方括号实际上等价于test命令，即`if [ bool表达式 ]`等价于 `if test 布尔表达式`。

#### （2）布尔表达式类型

| 表达式                | 描述                                          |
| --------------------- | --------------------------------------------- |
| ! EXPRESSION          | The EXPRESSION is false.                      |
| -n STRING             | The length of STRING is greater than zero.    |
| -z STRING             | The lengh of STRING is zero (ie it is empty). |
| STRING1 = STRING2     | STRING1 is equal to STRING2                   |
| STRING1 != STRING2    | STRING1 is not equal to STRING2               |
| INTEGER1 -eq INTEGER2 | INTEGER1 is numerically equal to INTEGER2     |
| INTEGER1 -gt INTEGER2 | INTEGER1 is numerically greater than INTEGER2 |
| INTEGER1 -lt INTEGER2 | INTEGER1 is numerically less than INTEGER2    |
| -d FILE               | FILE exists and is a directory.               |
| -e FILE | FILE exists. |
| -r FILE | FILE exists and the read permission is granted. |
| -s FILE | FILE exists and it's size is greater than zero (ie. it is not empty). |
| -w FILE | FILE exists and the write permission is granted. |
| -x FILE | FILE exists and the execute permission is granted. |

示例：

```shell
#! /bin/bash

if [ -s testShell.sh ]
then
	echo "file exists"
else
	echo "file not exists"
fi
```



## 三、循环

### 1. for循环

#### （1）按编号循环：

方法一：使用大括号+两点来获取一个迭代器，可以理解为python中的range函数。如：

```shell
#! /bin/bash

for i in {1..10}
do
	echo $i
done
```

注意10是会输出的，不同于Python。

方法二：`seq`关键字（与Python中range函数用法几乎一致）

```shell
#! /bin/bash

for i in $(seq 10)
do
	echo $i
done
```

输出与方法一相同，即从1打印到10。

注意，`$(seq 10)`与`$(seq 1 10)`以及`$(seq 1 10 1)`完全等价。有三个参数时，第一个参数表示起始值，第二个参数表示终止值，第三个参数表示间隔。

方法三：c语言式

```shell
#! /bin/bash

for ((i=1;i<=10;i++))
do
	echo $i
done
```

输出与方法一相同。

在Shell命令中，用双括号(( statement ))来引入c语言语句，其中包括整数比较、四则运算以及这里使用的c语言中的循环语句。

方法四：将遍历的值写死

```shell
#! /bin/bash

for i in 1 2 3 4 5 6 7 8 9 10
do
	echo $i
done
```

#### （2）遍历命令的输出

```shell
#! /bin/bash

for s in $(ls)
do
	echo $s
done
```

上述脚本通过单圆括号`(command)`来获取命令`ls`的输出，形成列表作为迭代器。

#### （3）遍历传入脚本的参数

脚本`testShell.sh`内容如下：

```shell
#! /bin/bash

for p in $@
do
	echo $p
done
```

如三-1-（1）中所示的方式传入对应的参数，即执行以下命令，则会打印所传入的参数：

```shell
$ sh testShell.sh 1 hello 3
1
hello
3
```

#### （4）遍历文件中的内容

遍历文件中的内容可以采用**输入重定向**的方式来获取文件文件内容（输入输出重定向可以参考[此文](https://codingcm.github.io/toolswiki/linux/2022/01/11/shellRedirection.html)），如：

```shell
#!/bin/sh

for s in $(<log.txt) 
do
echo $s
done
```

注意，`log.txt`中的**空格**和**换行符**都将成为切分文本的标记符号。

### 2. while循环

#### （1）按编号循环

```shell
#! /bin/bash

i=1
while ((i<=10))
do
	echo $i
	((i++))
done
```

while循环控制不同实现方式的主要区别在于**条件语句**和**变量自增**的不同写法。下面再给出一种实现方式：

```shell
#! /bin/bash

i=1
while [ $i -lt 11 ]
do
	echo $i
	let i=$i+1
done
```

这里的条件语句由c语言式改为了内部条件测试的方式（方括号），自增改用了let命令来实现。条件控制和变量自增都还有一些其他的方式，这里略。

