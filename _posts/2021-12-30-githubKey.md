---
title: Github本地仓库与远程仓库的关联
category: [toolswiki, git]
---

## 关于配置

通过`git clone`指令下载个人远程仓库到本地时，默认是进行过关联的。问题在于push时存在的权限问题，这里的配置是通过ssh密钥的方式来完成授权。具体方法可以参考[官方文档](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)。主要流程如下：
（1）在本地终端通过下述指令生成ssh密钥/公钥对：

```shell
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

在完成上述指令以后，在`~/.ssh/`文件夹下，会有存放相应密钥的文件，默认为`~/.ssh/id_rsa.pub`。

（2）修改ssh配置：

```shell
$ vim ~/.ssh/config
```

在文档末尾追加：

```shell
Host github.com
    Hostname        github.com
    IdentityFile    ~/.ssh/id_rsa.pub
    IdentitiesOnly yes
```

注意，若有多个Github账号下的仓库需要配对，可以生成多个密钥，并对 Host 对应的域名进行自定义修改，以便于后续的配置，如：

```shell
Host second.github.com  #这里进行了自定义
    Hostname        github.com
    IdentityFile    ~/.ssh/id_rsa_second.pub  #这是另外一个密钥文件
    IdentitiesOnly yes
```

 （3）在Github上根据[官方文档](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)，将`~/.ssh/id_rsa.pub`中的密钥粘贴至账户相关设置中。

