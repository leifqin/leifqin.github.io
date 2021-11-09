# Git常用命令

## 初始化提交
``` bash
cd ~/Workspace/leifqin.github.io
git init
touch README.md
git add .
git commit -m "first commit"
git remote add origin https://github.com.cnpmjs.org/leifqin/leifqin.github.io.git
git push -u origin master

# 加了参数 -u 后，以后即可直接用 git push 代替 git push origin master
```

## git remote 命令
``` bash
# 显示所有远程仓库
git remote -v

# 添加远程版本库
git remote add origin https://github.com.cnpmjs.org/leifqin/leifqin.github.io.git

# 提交到 Github
git push -u origin master

# 删除远程仓库
git remote rm name  

# 修改仓库名
git remote rename old_name new_name  
```