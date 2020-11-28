## 基础操作

- `git init`: 初始化本地仓库
- `open.git`: 打开git文件夹
- `cat index.html`: 可以在后台打印出这个文件的所有html代码
- `git status`: 查看工作区里文件的状态，红色高亮就是未保存进暂存区的文件
- `git add .`: 将所有文件增加到暂存区
- `git commit -m`: 将文件从暂存区提交到版本区,`-m`后面是提交的说明
- `git log` : 操作日志，`commit`后面的就是版本号

## diff指令

- `git diff`: 比较工作区和暂存区的差异，工作区代码更新后，可以通过这个指令看到更新的代码是哪条
- `git diff --cached`: 比较暂存区与版本区的差异
- `git diff master`: 比较工作区与版本区的差异

## 其它命令

- `git reset HEAD <file>`: 暂存区与版本区保持一致，版本区的内容会覆盖到暂存区，也就是暂存区的内容回退到add命令之前。
- `git checkout <file>`: 暂存区（暂存区没有内容就找版本区）覆盖工作区的内容。也就是回退到之前提交的文件。
- `git rm <file> --cached`: 删除暂存区文件
- `git commit -a -m <msg>`: git add.   git commit -m ' '
- `git reset --hard <version>`: 恢复版本区指定版本的内容到工作区，复制版本号前7位
- `git reflog`: 查看引用版本号

## 分支

- `git branch`: 查看分支
- `git branch dev`: 创建开发分支
- `git checkout dev`: 切换到dev分支
- `git checkout -b dev`: 创建并切换dev分支
- `git branch -d dev`: 删除dev分支
- `git merge dev`: 在master里输入指令， 将dev合并到master里去
- `git log --oneline --graph`: 展示历史操作图

