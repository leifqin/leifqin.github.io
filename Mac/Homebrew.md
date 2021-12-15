# Homebrew

## 切换源

``` bash
# 替换brew.git:
$ cd "$(brew --repo)"
# 中国科大:
$ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# 替换homebrew-core.git:
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
# 中国科大:
$ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# 替换homebrew-bottles:
# 中国科大:
# $ echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
# $ source ~/.bash_profile

$ echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
$ source ~/.zshrc

# 应用生效:
$ brew update


# 重置brew.git:
$ cd "$(brew --repo)"
$ git remote set-url origin https://github.com/Homebrew/brew.git

# 重置homebrew-core.git:
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
$ git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

## 常用命令

``` bash
# 安装软件
brew install wget
# 搜索软件
brew search wget
# 卸载软件
brew uninstall wget
# 更新所有软件：
brew update
# 更新具体软件
brew upgrade git
# 显示已安装软件
brew list
# 查看软件信息
brew info git 
brew home git
# 显示包依赖
brew reps
# 显示安装的服务
brew services list
# 安装服务启动、停止、重启
brew services start/stop/restart serverName
# 移除软件包
brew cleanup
```