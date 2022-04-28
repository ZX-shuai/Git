# Git
##### 1.在cnetos7上安装Git

（1）yum命令安装

```
sudo yum install -y git
```

（2）查看Git版本

```
git --version
```

#####  2.配置用户

配置一个用于提交代码的用户，同时配置一个用户的邮箱，输入命令：

![image-20220426220333830](C:\Users\张晓帅\AppData\Roaming\Typora\typora-user-images\image-20220426220333830.png)

否则就会报错

![image-20220426215956777](C:\Users\张晓帅\AppData\Roaming\Typora\typora-user-images\image-20220426215956777.png)

#####  3.创建一个版本库

在需要的位置创建一个裸仓库（最后以.git结尾）

```
cd /usr/local
mkdir git
cd git
git init --bare learngit.git
```

可以使用`git init`在当前目录创建 git 仓库，也可使用`git init <path>`在 path 路径下创建目录并创建 git 仓库。注意有个`--bare`参数可以生成一般作为远程仓库的裸仓库，其不包含工作区，可以参考[git init与git init --bare](https://www.jianshu.com/p/5b7ff91c5338)。

我们使用`git init`创建git仓库

```
mkdir git_test
cd git_test
git init learngit.git
```

![image-20220426151111854](C:\Users\张晓帅\AppData\Roaming\Typora\typora-user-images\image-20220426151111854.png)

##### 4.版本创建与回退

（1）在git_test目录下创建一个文件code.txt，编辑内容如下：
