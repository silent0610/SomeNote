---
Type:
  - Page
aliases: 
Status: 
tags:
  - 经验
modifiedDate: 星期一, 五月 26日 2025, 8:22:33 晚上
---
[经验](经验.md)
[Git](Git.md)

**原理**
[Git: 工作区、暂存区、本地仓库、远程仓库_git 工作 暂存 本地 远程-CSDN博客](https://blog.csdn.net/weixin_36750623/article/details/96189838)
![Git简单操作指南](assets/Git简单操作指南.png)
**基本操作**
[Git 基本操作 | 菜鸟教程 (runoob.com)](https://www.runoob.com/git/git-basic-operations.html)
- **工作区**：位于您本地文件系统的一个可见目录。
- **暂存区**：位于`.git/index`文件中，是逻辑上的一个区域。
- **本地仓库**：包含在工作区的`.git`隐藏目录中，存储了版本历史和元数据。
- **远程仓库**：位于远程服务器上，用于团队协作和代码共享。

## Git安装

## Git初始化

[将本地已有的git项目上传到新的空git仓库](https://blog.csdn.net/qq_22370409/article/details/120221769)
[代理端口问题](https://blog.csdn.net/hggjjkk/article/details/130528271)
设置本地用户名和电子邮箱

```
git config --global user.name "Jack"
git config --global user.email "silent0610@outlook.com"
```

指的是本地提交上去时显示的用户名

### 现有目录中建立仓库

```
git init
```

### 复制远程仓库到本地

```
git clone https://gitee.com/silent0610/note.git
```

## 使用

### 连接到远程仓库

```
git add remote https://gitee.com/silent0610/note.git
```

### 保存到仓库

```
git add -A
git commit -m "这里填备注"
git push
```

### 强制和远程保持一致

```
git reset --hard origin/main
```

### 强制push

```
git push origin master --force
```
