---
title: ubuntu 安装增强ls命令ls-go
date: 2018-05-11 11:23:30
tags: ubuntu
---


**1. 安装 `ls-go`**

```bash
go get -u github.com/acarl005/ls-go
```

**2. 安装Nerd Fonts**

```bash
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts && curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf
```

**3. 设置终端字体**

**4. 设置命令别名**

```bash
vi ~/.bashrc # 如果使用的是zsh 则 vi ~/.zshrc
```

添加 `alias lg="ls-go -alnk"`

```bash
source ~/.bashrc # 如果使用的是zsh 则 source ~/.zshrc
```

**5. 效果图**

![](/images/lg-go.jpg)

具体使用查看 [https://github.com/acarl005/ls-go](https://github.com/acarl005/ls-go)