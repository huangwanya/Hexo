---
title: git踩坑
date: 2020-08-08 17:38:41
tags: git
categories: 踩坑
---
## Git 将本地代码推到 Coding 远程仓库

1 首先创建文件夹，将要推的项目文件夹拷贝过来，进入文件夹 右键 Git Bash Here 输入以下代码 把这个目录变成git管理的仓库

```
git init
```

2 把文件添加到版本库中，使用命令 git add 文件夹名称 /  添加到暂存区里面去，不要忘记后面的斜杠“/”，意为添加文件夹下的所有文件

```
git add XXX/
```

3 用命令 git commit告诉Git，把文件提交到仓库。引号内为说明文字，这一步是把暂存区内容提交到分支

```
git commit -m "first commit"
```

4 关联到远程库 (首先要先创建你的远程库)

```
git remote add origin 远程库地址
```

5 获取远程库与本地同步合并（如果远程库不为空必须做这一步，否则后面的提交会失败）

```
git pull --rebase origin master
```

6 把本地库的内容推送到远程，使用 git push命令，实际上是把当前分支master推送到远程。执行此命令后会要求输入用户名、密码，验证通过后即开始上传。

```
git push -u origin master
# 使用强制推送'-f'是因为一般新建仓库的时候会生成read me文件，导致需要先git fetch才能推送，但这个read me文件其实是不需要的，因为在生成本地项目的时候一般也会生成一个read me文件，所以选择直接强制推送过去。
git push origin master -f
```

```
git push origin master -f
```

## 总结：

最主要的就是最后一步，这里建议直接强制推送，不然总是有错误，本人也是搞了很久才出来！