---
title: "删除目录下空文件夹与文件"
date: 2023-10-03T18:08:42+08:00
type: "posts"
---

🎬场景：当服务器某一路径下存放文件后，想要删除**该文件**。但要**保留原始配置文件路径**。

🎞️例：配置文件配置了`\User\testDelete`的路径，当程序将一个zip包解压到该路径下时，假设文件上层的文件夹名与层级未知，如`\User\testDelete\zipFile\realFile\test.txt`。这时就要用到递归调用。删除该目录的test.txt 文件。



原始文件夹

```shell
User
└──testDelete
		└──test.txt
```

## 版本一

只删除文件夹内的文件，空文件夹不受影响

```java
//版本一，只删除文件，但空文件夹依然存在
public static void deleteDirectory(File folder){
        for(File file : Objects.requireNonNull(folder.listFiles())){
            if(file.isDirectory()){
                //递归调用文件删除
                deleteDirectory(file);
            }else{
                file.delete();
            }
        }
    }
```

执行⬇️代码后的目录

```java
deleteDirectory(new File("/User/testDelete"));
```

```shell
User
└──testDelete
```

已经达成目的了？不🙅🏻，其实这是因为文件目录仅有两层。

当文件目录

```shell
User
└──testDelete
		└──realFile
				└──file
						└──test.txt
```

执行⬇️后

```shell
deleteDirectory(new File("/User/testDelete"));
```

新的目录为

```shell
User
└──testDelete
		└──realFile
				└──file
```



## 版本二

删除该路径下所有文件与空文件夹，包括传参的文件夹

```java
    public static void deleteDirectory(File folder){
        for(File file : Objects.requireNonNull(folder.listFiles())){
            if(file.isDirectory()){
                //递归调用文件删除
                deleteDirectory(file);
            }else{
                file.delete();
            }
        }
        folder.delete();
    }
```

执行⬇️代码后的目录

```java
deleteDirectory(new File("/User/testDelete"));
```

```shell
User
```

原始文件夹路径

```shell
User
└──testDelete
		└──test.txt
```

## 版本三

只删除传参文件夹下的所有空文件与空文件夹，与版本二不同的是保留`User/testDelete`的文件夹目录

```java
//在递归调用时判断当前file的absoulutePath是否是baseUrl，是则保留该文件夹
    public static void deleteDirectory(File folder,String baseUrl){
        for(File file : Objects.requireNonNull(folder.listFiles())){
            if(file.isDirectory()){
                //递归调用文件删除
                deleteDirectory(file,baseUrl);
            }else{
                file.delete();
            }
        }
        if (!folder.getAbsolutePath().equals(baseUrl)) {
            folder.delete();
        }
    }
```

执行⬇️代码后的目录

```java
deleteDirectory(new File("/User/testDelete"),new String("/User/testDelete"));
```

```shell
User
└──testDelete
```

🥳
