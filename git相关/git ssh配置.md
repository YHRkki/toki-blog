Git安装完之后，需要做最后一步配置，如果你没有做这项配置，是没有git公钥和私钥的，而上传代码到远程仓库的时候需要秘钥进行验证是否本人上传的，想要创建可以使用下面的方法：

```
打开git bash，分别执行以下两句命令
git config --global user.name “用户名”
git config --global user.email “邮箱”
```

#### SSH配置

- 打开git bash。
- 使用`cd ~/.ssh`可以查看是否已配置SSH。
- 执行生成公钥和私钥的命令`ssh-keygen -t rsa` 并按回车3下（为什么按三下，是因为有提示你是否需要设置密码，如果设置了每次使用Git都会用到密码，一般都是直接不写为空，直接回车就好了）。会在一个文件夹里面生成一个私钥 id_rsa和一个公钥id_rsa.pub。（可执行`start ~`命令，生成的公私钥在 .ssh的文件夹里面）。
- .ssh如果不做特殊处理的话，一般是在C:\Users\Administrator目录下。如果看不到.ssh文件，可以使用`ls -ah`指令查看隐藏文件夹即可，这是存放秘钥的文件，打开这个文件会看到id_rsa和id_rsa.pub。id_rsa是私钥文件，id_rsa.pub是公钥文件。
- 执行查看公钥的命令`cat ~/.ssh/id_rsa.pub`  。

