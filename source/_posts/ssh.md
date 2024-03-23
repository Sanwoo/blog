---
title: ssh
---
在生成SSH密钥前，应先检查是否已存在SSH key

1、打开Git Bash
2、输入ls -al ~/.ssh以查看是否存在现有密钥
3、检查目录列表是否已有类似id_rsa和id_rsa.pub的两个文件，前者是密钥文件，后者是公钥文件

默认情况下，公钥的文件名是以下之一：

id_dsa.pub


id_ecdsa.pub


id_ed25519.pub


id_rsa.pub

如果未发现有类似的公钥和私钥对，则可以按照如下方法生成一个SSH key：

1、打开Git Bash
2、输入下面的命令，将邮箱地址替换你的GitHub邮箱地址

>$ ssh-keygen -t rsa -b 4096 -C " your_email@example.com"
各参数含义：
-t：密钥类型，一般为dsa，ecdsa，ed25519和rsa这几种，默认为rsa，可省略；
-b：密钥的位数；
-C：注释文字，比如邮箱。

3、当出现以下提示指定保存位置时可选择直接Enter回车，即选择默认保存位置

>Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]

4、接着会提醒设置密码，可以直接回车回车，当然也可以选择设置密码

>Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]

到这里就生成了一个新的SSH key，可以使用ls命令查看生成后的文件目录：

id_rsa和id_rsa.pub分别为私钥和公钥
win系统如果此处如果看到两个id_rsa文件，只需点击查看->点击显示->勾选显示文件扩展名

新建一个config文件
>$ touch ~/.ssh/config

然后在config文件中添加以下内容：

>Host *.github.com
    IdentityFile ~/.ssh/id_rsa
    User '你的用户名'

接着将id_rsa.pub内的内容复制并添加到你的GitHub账户中（Settings->SSH and GPG keys->New SSH key)
OK，进行一下链接测试

>$ ssh -T git@github.com

回车后出现（如果设置了密码，则需输入密码）：

>Hi xxx! You've successfully authenticated, but GitHub does not # provide shell access.

就表示设置成功了，Bingo！
