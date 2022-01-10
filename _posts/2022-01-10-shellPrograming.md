---
title: shell编程小结
category: [toolswiki, linux]
sidebar:
    nav: TOOL
---

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

（1）脚本传参示例

现有如下`test.sh`脚本：

```shell
#!/bin/sh

for TOKEN in $*
do
   echo $TOKEN
done
```

执行以下命令：

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

（1）语句格式：

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

（2）布尔表达式类型

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

（1）按编号循环：

方法一：使用大括号+两点，等价于python中的range函数。如：

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

方法三：c++式

```shell
#! /bin/bash

for ((i=1;i<=10;i++))
do
	echo $i
done
```

输出与方法一相同。

方法四：将遍历的值写死

```shell
#! /bin/bash

for i in 1 2 3 4 5 6 7 8 9 10
do
	echo $i
done
```



