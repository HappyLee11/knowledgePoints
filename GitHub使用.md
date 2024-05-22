

### **初始设置**

#### 在Git　Bash中：

首先来设置使用Git时的姓名和邮箱地址。名字需要用英文来设置

```c
$ git config --global user.name "Honker"
$ git config --global user.email "your-email" 
```

设置SSH Key:

```
ssh-keygen -t rsa -C "your-email"
```

![img](https://gitee.com/happy-zhang/pic-go_image/raw/master/G:%5CPicGo_image/20200104224721180.png)

输入密码回车之后会出现下图显示效果

![img](https://gitee.com/happy-zhang/pic-go_image/raw/master/G:%5CPicGo_image/20200104224734712.png)

### 基本操作

1、git init 

初始化该文件夹

2、git status

查看状态

3、git add .提交所有文件到缓冲区

4、git commit -m '备注'     提交

### 添加远程仓库

首先，确保你已经添加了远程仓库。如果还没有，可以使用以下命令添加远程仓库：

```c
git remote add origin https://github.com/你的用户名/你的仓库名.git
```

### 推送更改到远程仓库

使用以下命令将本地提交推送到远程仓库：

```c
git push -u origin master

```

如果你希望将本地`master`分支推送到远程的`main`分支

```c
git push -u origin master:main
```

# github问题：master和main分支合并

问题原因，github创建仓库后默认分支是main，而本地创建是master。所以，提交后发现两个分支，解决步骤如下：



1.先给本地分支master改名

```text
$ git branch -M main 
```

说明：“-M”对分支重命名

2.查看所有分支

```text
$ git branch -a 
* main   
remotes/origin/main   
remotes/origin/master  
```

3.删除远程分支master

```text
$ git push origin --delete master 
To https://github.com/***/learnOpenGL.git
  - [deleted]         master  
```

4.确认删除情况

```text
$ git branch -a 
* main   
remotes/origin/main  
```

5.切换到当前分支main，也就要保留下来的分支

```text
$ git checkout main 
Already on 'main'  
```

说明：“Already on 'main'”已经说明在当前分支

6.合并分支

```text
$ git merge remotes/origin/main 
fatal: refusing to merge unrelated histories 
```

说明：拒绝合并，需要忽略这个限制，添加“--allow-unrelated-histories”

```text
$ git merge remotes/origin/main --allow-unrelated-histories 
Merge made by the 'recursive' strategy. 
```

7.提交修改

```text
$ git push origin main 
Enumerating objects: 34, done. 
Counting objects: 100% (34/34), done. 
Delta compression using up to 8 threads 
Compressing objects: 100% (28/28), done. 
Writing objects: 100% (32/32), 1.46 MiB | 1.03 MiB/s, done. 
Total 32 (delta 1), reused 0 (delta 0), pack-reused 0 
remote: Resolving deltas: 100% (1/1), done. 
To https://github.com/test/learnOpenGL.git 
   76a2889..d34cf75  main -> main  
```

8.再次查看分支情况

```text
$ git branch -a 
* main   
remotes/origin/main 
```



注意：期间可能会弹出一些文本框，直接关闭。

### 历史不匹配问题

根据您的记录，您遇到了一些连接问题和历史不匹配问题。让我们逐步分析并解决这些问题。

1. 连接问题:

您的错误日志显示了多次尝试推送到 GitHub 时出现了连接超时和连接重置错误。这可能是由于网络连接问题、代理设置或防火墙配置引起的。您可以尝试以下步骤来解决：

1. **检查网络连接**：
   确保您当前的网络连接正常，并且可以访问 GitHub 网站。

2. **配置代理（如果有）**：
   如果您在公司或使用代理服务器，请确保配置正确的代理设置。您可以通过以下命令检查和设置 Git 的代理配置：

   ```sh
   git config --global --get http.proxy
   git config --global --get https.proxy
   ```

   如果未配置代理或配置错误，可以使用以下命令设置代理：

   ```sh
   git config --global http.proxy http://proxyuser:proxypwd@proxy.server.com:8080
   git config --global https.proxy https://proxyuser:proxypwd@proxy.server.com:8080
   ```

3. **尝试更换网络环境**：
   如果您使用的是公司网络或受限网络环境，尝试切换到不同的网络环境（例如家庭网络或移动热点）以排除网络限制的可能性。

2. 历史不匹配问题

从日志中可以看到，您遇到了以下错误：

- `error: src refspec main does not match any`
- `fatal: refusing to merge unrelated histories`
- `! [rejected] main -> main (fetch first)`

这些错误表明本地和远程仓库的提交历史不匹配。以下是解决方案：

1. **重命名分支**：
   您已经将本地分支从 `master` 重命名为 `main`，并删除了远程的 `master` 分支。这是正确的步骤。继续确保本地和远程分支名称一致。

2. **拉取远程更改**：
   在推送前，需要拉取远程分支的最新更改并解决冲突：

   ```sh
   git pull origin main --allow-unrelated-histories
   ```

   这个命令会强制合并不相关的历史记录。

3. **推送本地更改**：
   一旦拉取并解决冲突后，可以推送本地更改到远程分支：

   ```sh
   git push -u origin main
   ```

具体步骤:

以下是详细的步骤：

1. **检查和配置代理（如果需要）**：

   ```sh
   git config --global --get http.proxy
   git config --global --get https.proxy
   # 如果没有设置代理，跳过以下命令
   git config --global http.proxy http://proxyuser:proxypwd@proxy.server.com:8080
   git config --global https.proxy https://proxyuser:proxypwd@proxy.server.com:8080
   ```

2. **重命名本地分支并删除远程 master 分支（已经完成）**：

   ```sh
   git branch -m master main
   git push origin --delete master
   ```

3. **拉取远程更改**：

   ```sh
   git pull origin main --allow-unrelated-histories
   ```

4. **推送本地更改**：

   ```sh
   git push -u origin main
   ```

如果这些步骤中仍然遇到连接问题，请检查您的网络配置和代理设置，确保没有防火墙或网络限制阻止访问 GitHub。如果问题持续存在，可以尝试联系网络管理员或使用不同的网络环境进行操作。

### 重命名本地分支

好的，让我们详细说明这些步骤并确保您能够顺利进行操作。以下是从重命名本地分支到确保本地和远程分支名称一致的具体步骤：

具体步骤

1. **重命名本地分支**
   首先，重命名本地分支从 `master` 到 `main`：

   ```sh
   git branch -m master main
   ```

   这一步已经完成，不需要再次执行。

2. **删除远程的 master 分支**
   删除远程仓库中的 `master` 分支：

   ```sh
   git push origin --delete master
   ```

   您已经完成了这一步，不需要再次执行。

3. **拉取远程 main 分支的最新更改**
   在推送本地更改之前，需要拉取远程 `main` 分支的最新更改。如果本地和远程历史不匹配，需要允许合并不相关的历史记录：

   ```sh
   git pull origin main --allow-unrelated-histories
   ```

   这一步非常重要，它将确保您的本地分支与远程分支保持一致。

4. **推送本地更改到远程 main 分支**
   一旦您拉取并解决了任何冲突，可以推送本地更改到远程分支：

   ```sh
   git push -u origin main
   ```

检查和配置代理（如果需要）

如果您在上述步骤中遇到连接问题，可能需要检查和配置代理设置。以下是检查和设置代理的步骤：

1. **检查当前代理配置**：

   ```sh
   git config --global --get http.proxy
   git config --global --get https.proxy
   ```

2. **设置代理（如果需要）**：

   如果您使用代理服务器，请按照以下命令设置：

   ```sh
   git config --global http.proxy http://proxyuser:proxypwd@proxy.server.com:8080
   git config --global https.proxy https://proxyuser:proxypwd@proxy.server.com:8080
   ```

综合示例

以下是一个完整的示例，结合了上述所有步骤：

```sh
# Step 1: Rename the local branch from master to main
git branch -m master main

# Step 2: Delete the remote master branch
git push origin --delete master

# Step 3: Pull the latest changes from the remote main branch
git pull origin main --allow-unrelated-histories

# Step 4: Push local changes to the remote main branch
git push -u origin main

# Optional: Check and set proxy settings if necessary
# Check current proxy settings
git config --global --get http.proxy
git config --global --get https.proxy

# Set proxy settings (if needed)
git config --global http.proxy http://proxyuser:proxypwd@proxy.server.com:8080
git config --global https.proxy https://proxyuser:proxypwd@proxy.server.com:8080
```

这些步骤将帮助您解决分支名称不一致和连接问题，并成功将本地更改推送到远程仓库。如果您仍然遇到问题，请确保网络连接正常，并检查是否有任何防火墙或网络配置阻止访问 GitHub。