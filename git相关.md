#### git 相关

+ git reset --hard: 加载一个旧记录并删除所有比之新的记录。
+ git checkout: 加载一个旧记录，但如果你在这个记录上玩，游戏状态将偏离第 一轮的较新状态。你现在打的所有游戏记录会在你刚进入的、代表另一个真实的分支里
+ git diff HEAD -- readme.txt”命令可以查看工作区和版本库里面最新版本的区别




1. create a repository at github
2. create a .ssh/rsa.pub and add the ssh key to the github
3. git init a directory
4. git add ,commit
5. git remote add origin git@github.com:m3n78am/learngit.git
6. 把本地库的内容推送到远程，使用git push命令，由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
7. 从现在起，只要本地作了提交，就可以通过命令git push origin master
8. git log --graph --pretty=oneline --abbrev-commit