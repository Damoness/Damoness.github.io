---
title: Mac 上GitHub 配置 SSH Key
date: 2020-05-18 16:02:01
tags:
---

## 创建一个新的SSH key

1. 打开终端,
2. 输入创建命令, 引号部分是你的GitHub 邮箱地址
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

3. 当提示出现”Enter a file in which to save the key” ,可以按enter,接受默认的文件名和位置, 后面会用到
```
Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
```

4. 提示你输入2次密码
```
Enter passphrase (empty for no passphrase): [输入密码]
Enter same passphrase again: [再次输入密码]
```

## 增加 SSH key 到ssh-agent

1. 启动ssh-agent
```
eval "$(ssh-agent -s)"
```
2. 如果你是使用的macOS Sierra 10.12.2或之后的系统,你需要修改你的~/.ssh/config配置文件 来加载keys到ssh-agent 和 存贮密码到keychain
```
Host *
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa
```
3. 增加你的SSH 私有key到ssh-agent, 并存储你的密码到keychain. 这里的id_rsa就是上面创建时填写的名字
```
$ ssh-add -K ~/.ssh/id_rsa
```

## 增加SSH key到 GitHub 账号

1. 拷贝你的SSH key到剪切板
这里id_ras.pub是上面创建名字

```
pbcopy < ~/.ssh/id_rsa.pub
```

2. 登录你的GitHub账号,进入Settings,点击SSH and GPG keys.
点击New SSH key 或 Add SSH key
把你复制的SSH key粘贴到key所对应的输入框中,记住SSH key代码的前后不能留空或回车. 上面的Title可以任意输入.


## 测试一下SSH key

1. 输入
```
ssh -T git@github.com
```
2. 可能会有一段警告,输入yes就可以了
3. 如果出现
```
Hi Damoness! You've successfully authenticated, but GitHub does not provide shell access.
```
就说明已经成功了