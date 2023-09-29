---
title: "修改Mac终端的前缀"
date: 2023-09-26T18:21:43+08:00
type: "posts"
---

进入终端时前缀太长了😤

```shell
jeff jeffzhangdeMacBook-Pro ~ % 	
```

想要修改并保留有用信息⬇️

```shell
jeff ~ > 
```

## 1. 修改文件权限

根据自己系统选择 **其中一个** 命令执行

 macOS Catalina之后（包含）【大多数选择】

```shell
sudo chmod -R 777 /etc/zshrc
```

 macOS Catalina之前（不包含）注：如旧系统 以下修改配置不适用

```shell
sudo chmod -R 777 /etc/bashrc
```

输入当前用户的密码，权限修改完成

## 2. 修改配置

以`/etc/zshrc`为例

```shell
vi /etc/zshrc
```

输入后会显示如下

![image-20230920111346909](http://ossimgup.oss-cn-beijing.aliyuncs.com/img/image-20230920111346909.png)

### · 各个字段含义

键入`E`进入编辑，在文件底部有如下配置

```shell
# Default prompt
PS1="%n %m %1~ %# "
```

对应终端实际显示前缀为

```shell
jeff jeffzhangdeMacBook-Pro ~ % 	
```

| %n       | %m                     | %1～         | %#       |
| -------- | ---------------------- | ------------ | -------- |
| jeff     | jeffzhangdeMacBook-Pro | ~            | %        |
| 用户名称 | 主机名称               | 当前所在目录 | 分隔符号 |

### · 修改PS1变量

如希望终端前缀显示为

```shell
jeff ~ > 
```

按下`I`执行插入操作

将PS1修改为

```
PS1="%n %1~ > "
```

确认无误后，按`ESC`退出编辑

键入`:wq`保存当前修改

## 3. 验证

`command+Q`退出终端，并重新打开，终端显示

```shell
jeff ~ > cd Documents
jeff Documents > 
```

配置成功！🥳

