常用git命令
##### 创建版本库
- 克隆远程仓库：git clone <url>（仓库地址）
- 初始化本地仓库：git init

##### 修改提交
- 查看状态：git status
- 查看变更内容：git diff
- 跟踪所有改动过的文件：git add .
- 跟踪指定文件：git add <file>
- 删除文件：git rm <file>
- 提交所有更新过的文件：git commit -m "commit message"

##### 查看提交历史
- 查看提交历史：git log
- 查看指定文件的提交历史：git blame <file>

##### 撤销
- 撤销工作目录中所有未提交文件的修改内容：git reset --hard HEAD
- 撤销指定的提交：git revert <commit>

##### 分支与标签
- 显示所有本地分支：git branch
- 切换到指定分支或标签：git checkout <branch/tag>
- 创建新分支：git branch <new-branch>
- 删除本地分支：git branch -d <branch>
- 基于最新提交创建标签：git tag <tagname>
- 删除标签：git tag -d <tagname>

##### 合并
- 合并指定分支到当前分支：git merge <branch>

##### 远程操作
- 查看远程版本库信息：git remote -v
- 查看指定远程版本库信息： git remote show <remote>
- 添加远程版本库：git remote add <remote> <url>
- 从远程库获取代码：git fetch <remote>
- 下载代码并快速合并：git pull <remote> <branch>
- 上传代码并快速合并：git push <remote> <branch>
- 上传所有标签：git push --tags
- 删除远程分支或标签：git push <remote> :<branch/tagnames>

**vim命令详解**：https://www.cnblogs.com/usergaojie/p/4583796.html

**git上传文件到github步骤**：
1. 克隆远程仓库到本地；(git clone)
2. cd+仓库名找到本地仓库地址；
3. 在本地仓库添加/修改文件；
4. 将修改完的文件添加到暂存区；(git add)
5. 提交更改并附上更改信息；(git commit)
6. 推送到远程仓库。(git push)

**git分支(dev)与主支(master)关系**：并行开发，互不干扰。

解决代码冲突：
1. 将远程文件拉回本地；(git push)
2. 使用vim命令或直接在本地仓库修改();


    代码冲突显示例子：

    <<<<<<< HEAD:file.txt

    Hello world

    =======

    Goodbye

    >>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt
     
    修改方式：修改代码并将冲突标识符删除

3.重新执行上述上传文件的命令。