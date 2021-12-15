# Mac通过ClashX配置iTerm2终端代理

1.在 .zshrc 添加(注意端口替换成自己控制台梯子的端口号)
``` bash
alias proxy_on="export no_proxy=localhost,127.0.0.1,localaddress,.localdomain.com;export http_proxy=http://127.0.0.1:7890;export https_proxy=$http_proxy;export all_proxy=socks5://127.0.0.1:7890;"
alias proxy_off="unset http_proxy https_proxy all_proxy"
```

2.加载文件
``` bash
source .zshrc
```

3.终端输入，查看访问情况
``` bash
# 开启代理
proxy_on

# 关闭代理
proxy_off
```

> 注意事项: source .zshrc 失败

compinit: no such file or directory: /usr/local/share/zsh/site-functions/_docker<br/>
compinit: no such file or directory: /usr/local/share/zsh/site-functions/_docker_compose

``` bash
brew cleanup
```

Error: Permission denied @ apply2files - /usr/local/lib/docker/cli-plugins

``` bash
sudo chown -R $(whoami) $(brew --prefix)/*
```
