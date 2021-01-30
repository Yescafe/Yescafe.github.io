---
layout: wiki
title: Linux/UNIX Deployment Help
---

- [What's This?](#whats-this)
- [To-do List](#to-do-list)
- [CLI 工具](#cli-工具)
  - [终端 HTTP 代理](#终端-http-代理)
    - [使用全局变量](#使用全局变量)
    - [使用 proxychians-ng](#使用-proxychians-ng)
    - [关于虚拟机的代理](#关于虚拟机的代理)
  - [Zsh](#zsh)
  - [Oh-my-zsh](#oh-my-zsh)
    - [powerlevel10k](#powerlevel10k)
    - [git](#git)
    - [z](#z)
    - [rails](#rails)
    - [sudo](#sudo)
    - [zsh-syntax-highlighting](#zsh-syntax-highlighting)
    - [zsh-autosuggestions](#zsh-autosuggestions)
  - [Git](#git-1)
    - [全局用户配置](#全局用户配置)
    - [全局 HTTP 协议代理 (http:// 或 https://)](#全局-http-协议代理-http-或-https)
    - [全局 SSH 协议代理 (git@github.com 等)](#全局-ssh-协议代理-gitgithubcom-等)
    - [全局 Git 协议代理 (git://)](#全局-git-协议代理-git)
    - [GitHub CLI](#github-cli)
  - [SSH](#ssh)
    - [SSH Key 生成](#ssh-key-生成)
    - [使用 SSH Key 进行 SSH 免密连接](#使用-ssh-key-进行-ssh-免密连接)
    - [SSH Config](#ssh-config)
  - [Vim](#vim)
  - [Homebrew (macOS)](#homebrew-macos)
  - [RVM](#rvm)
  - [GCC/Clang (macOS)](#gccclang-macos)
  - [Makefile、CMake](#makefilecmake)
  - [Python-pip](#python-pip)
  - [Anaconda](#anaconda)
  - [Thefuck](#thefuck)
- [GUI 工具](#gui-工具)
  - [浏览器代理](#浏览器代理)

# What's This?
原 [Ubuntu Wiki](https://yescafe.github.io/wiki/ubuntu/) 的再版，适用于多数 Linux/UNIX 系统，包括 macOS。

# To-do List

- [ ] 丰富 whats this
- [ ] 代理工具
- [ ] 某些 GUI 工具

# CLI 工具

## 终端 HTTP 代理

<span class="wikipage-warn">注意，所有代理的配置需要根据自身情况进行配置，尤其不要照搬 IP 和 端口号。Git 代理部分将不再重复这段话。</span>

### 使用全局变量

```bash
# 注意 IP 和端口号
# HTTPS 代理
export https_proxy=http://127.0.0.1:7890

# HTTP 代理
export http_proxy=http://127.0.0.1:7890

# socks 代理
export all_proxy=socks5://127.0.0.1:7890
```

以上配置参考 ClashX，其他代理工具在对应的 preferences/config 里查看端口号。

当然可以为上面的指令创建别名，添加进终端程序的 RC 文件中：

```bash
alias proxy_set="export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890; curl
ipinfo.io"
alias proxy_unset="unset http_proxy; unset https_proxy; unset all_proxy; curl ipinfo.io"
```

这里还提供一种作为局部变量修饰某一条指令的方式，参考 `env` 的工作原理：

```bash
alias proxy_prefix="https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890"
```

使用方式与 `env` 相同，如：

```bash
proxy_prefix gh repo clone Yescafe/.whichrc
```

### 使用 proxychians-ng

*这个工具白苹果使用非常麻烦，不建议白果子使用。黑果子请自行调教。*

大部分系统可以使用包管理工具安装，测试以下发行版可行：

```bash
# Ubuntu
sudo apt install proxychains

# Fedora
sudo dnf install proxychains
```

接着配置 proxychains。用编辑器打开 `/etc/proxychains.conf`，注释最后一行，在尾部添加内容。下面是尾部的参考内容：

```
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
#socks4   127.0.0.1 9050
http 127.0.0.1 7890
```

根据情况选择 HTTP 或者 socks，这里展示的是 ClashX HTTP 的配置。

### 关于虚拟机的代理

一些代理工具，如 Clash，会有一个允许 LAN 连接的功能，开启后，虚拟机可以通过母机的虚拟网卡 IP 访问母机的代理。如我的虚拟机的 proxychains 的配置为：

```
http 192.168.123.1 7890
```

## Zsh
macOS 的 login shell 程序默认为 zsh。其他发行版基本可以使用对应的包管理工具安装。
```bash
# Debian/Ubuntu/Deepin
sudo apt install zsh

# CentOS
sudo yum install zsh

# Fedora
sudo dnf install zsh
```

## Oh-my-zsh

 [https://ohmyz.sh/](https://ohmyz.sh/)

<span class="wikipage-warn">注：依赖于 curl、git。</span>

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

macOS 需在 `~/.zshrc` 中添加

```bash
export ZSH_DISABLE_COMPFIX=true
```

以禁用 zsh 启动警告。

**主题建议：**

### powerlevel10k

[https://github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k)

```bash
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

修改 `~/.zshrc`：

```
ZSH_THEME="powerlevel10k/powerlevel10k"
```

**自用插件列表：**

### git

git 常用指令的略写插件。oh-my-zsh 默认已经配置。

### z

常用路径自动记录，快速搜索跳转。无需额外安装。

### rails

rails 常用指令的略写插件。无需额外安装

### sudo

按两次 `<ESC>` 自动在当前命令或上一条命令前补 `sudo`。无需额外安装。

### zsh-syntax-highlighting

指令高亮。

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

### zsh-autosuggestions

指令自动建议。

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

## Git

### 全局用户配置

```bash
git config --global user.name "Foo Bar"
git config --global user.email "foobar@example.com"
```

### 全局 HTTP 协议代理 (http:// 或 https://)

```bash
git config --global http.proxy 127.0.0.1:HTTP_PORT
git config --global https.proxy 127.0.0.1:HTTP_PORT
```

### 全局 SSH 协议代理 (git@github.com 等)

修改 SSH 配置 `~/.ssh/config`：

以 GitHub 为例子：

```
Host github.com
    User git
    # ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=HTTP_PORT
    # ProxyCommand nc -v -x 127.0.0.1:SOCKS_PORT %h %p
```

两个 `ProxyCommand` 二选一，第一个为走 HTTP 协议代理，第二个为走 socks 协议代理。取消注释即使用。

### 全局 Git 协议代理 (git://)

克隆仓库 [https://github.com/cms-sw/cms-git-tools](https://github.com/cms-sw/cms-git-tools)，参考 `init.sh` 配置 PATH 变量。

执行：

```bash
git config --global core.gitproxy "git-proxy"
git config --global socks.proxy "HOSTNAME:PORT"
```

### GitHub CLI

[https://cli.github.com/](https://cli.github.com/)

与 GitHub Desktop 对立，有一定用处。

## SSH

### SSH Key 生成

```bash
ssh-keygen -t rsa -C foobar@example.com
```

### 使用 SSH Key 进行 SSH 免密连接

```bash
ssh-copy-id -i user@hostname
```

### SSH Config

这里只列举一种配置：Host 别名。举例：

```
Host ivan-vm-ubuntu
  HostName 192.168.123.2
  User ivan
```

而后即可使用

```bash
ssh ivan-vm-ubuntu
```

代替使用

```bash
ssh ivan@192.168.123.2
```

## Vim

Emacs 的配置不贴在这里，因为确实比较多。相较于类似于 IDE 的 Emacs，Vim 作为一个轻量级文本编辑器是非常便捷的。这里参考了[知乎的一篇专栏](https://zhuanlan.zhihu.com/p/69725463)，且本配置文件的上游在[此](https://github.com/Yescafe/.whichrc/blob/main/.vimrc)。无严重 bug 此 wiki 中的该配置将不再更新。

```bash
set nu
set relativenumber
set expandtab
set tabstop=2
set shiftwidth=2
set softtabstop=2

" 基本配置
" 禁用 swap 恢复文件
set noswapfile
" 编码
set encoding=utf-8
" 文件检测编码
" 防止 Windows 下保存的中文文本文件乱码
set fileencodings=utf-8,gb18030
" 插入模式自动锁进
set autoindent
" 修正 vim 退格键行为
set backspace=eol,start,indent
" 双行显示状态
set laststatus=2
" 80 列高亮
set colorcolumn=80
" 高亮所在行
set cursorline
" 自动折行
set linebreak
" 语法高亮
syntax on
" 自动识别文件类型
filetype plugin indent on

" 主题 tender
" git clone https://github.com/jacoborus/tender.vim ~/.vim/pack/vendor/start/tendor
" 开启 24 位真彩色支持
" 24位真彩色信息请参考 https://gist.github.com/XVilka/8346728
set termguicolors
" 设置主题
color tender
" tender 默认背景色是一种亮灰色，晚上特别不舒服，改成纯黑
highlight Normal guibg=#000001
" 改背景后选中区域无法高亮，也得调一下
highlight Visual guibg=#323232
" 灰色的状态栏很扎眼，调一下
highlight StatusLine guibg=#444444 guifg=#b3deef
" vim 的 terminal 状态栏需要单独配置
highlight StatusLineTerm guibg=#444444 guifg=#b3deef
highlight StatusLineTermNC guibg=#444444 guifg=#999999

" nerdtree
" git clone https://github.com/scrooloose/nerdtree ~/.vim/pack/vendor/start/nerdtree
nnoremap <silent> <leader>e :NERDTreeToggle<cr>
nnoremap <silent> <leader>f :NERDTreeFind<cr>

" fzf
" brew install fzf
" git clone https://github.com/lvht/fzf ~/.vim/pack/vendor/start/fzf
" 模糊路径搜索
nnoremap <silent> <c-p> :call fzf#Open()<cr>

" Mru
" git clone https://github.com/lvht/mru ~/.vim/pack/vendor/start/mru
" 最近打开文件列表
nnoremap <silent> <c-u> :Mru<cr>

" ack
" 文件搜索
" git clone https://github.com/mileszs/ack.vim ~/.vim/pack/vendor/start/ack
```

## Homebrew (macOS)

[https://brew.sh/](https://brew.sh/)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Homebrew 换源参考的是 [https://www.cnblogs.com/tp0829/p/Homebrew.html](https://www.cnblogs.com/tp0829/p/Homebrew.html)。网上大部分的文章都是在扯淡，根本没有完全换源，update 速度还是很慢。

替换 Homebrew、homebrew-core、homebrew-cask 的 Git remote 源：

```bash
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-cask"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
```

修改 Homebrew-bottles 源。将下面的全局变量加入终端程序的 RC 文件中：

```bash
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
```

详情参考 [USTC Mirror Help](http://mirrors.ustc.edu.cn/help/)。

## RVM

[https://rvm.io/rvm/install/](https://rvm.io/rvm/install/)

```bash
gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash
```

修改 RC 文件，配置 PATH 变量：

```bash
export PATH=$HOME/.rvm/bin:$PATH
```

获取 Ruby 版本列表，编译安装 Ruby，配置默认 Ruby：

```bash
rvm list known
rvm install 2.6.5
rvm use 2.6.5 --default
```

## GCC/Clang (macOS)

macOS 需在终端程序 RC 文件中添加

```bash
export LDFLAGS="-L/usr/local/lib"
export CPPFLAGS="-I/usr/local/include"
```

以便 C/C++ 编译器默认加载用户安装的 C/C++ 库。

此外，macOS 真实的 GCC 需要使用 Homebrew 进行安装。

## Makefile、CMake

修改 Makefile 和 CMake 默认 C/C++ 编译器参考：

```bash
alias make_by_gcc="export CC=/usr/local/bin/gcc-10;export CXX=/usr/local/bin/g++-10;export CMAKE_C_COMPILER=/usr/local/bin/gcc-10;export CMAKE_CXX_COMPILER=/usr/local/bin/g++-10"
alias make_by_clang="export CC=/usr/bin/clang;export CXX=/usr/bin/clang++;export CMAKE_C_COMPILER=/usr/bin/local/clang;export CMAKE_CXX_COMPILER=/usr/bin/local/clang++"
```

## Python-pip

pip 源的配置不再需要手动。先使用默认源安装一个非常小的工具：

```bash
# 系统安装
pip install pqi

# 用户安装
pip install pqi --user
```

注意，使用用户安装的位置根据不同系统而定。Linux 常见于 `$HOME/.local/bin` 中，macOS 则是根据不同 Python 版本在用户库目录下分开存放，如 `~/Library/Python/3.8/bin`。

然后使用 pqi 工具修改源。用法见指令本身。

亦可考虑手动修改 pip 源，文件位置 `~/.pip/pip.conf`：

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = https://pypi.tuna.tsinghua.edu.cn
```

提供其他的一些源：

1. Aliyun：http://mirrors.aliyun.com/pypi/simple/
2. Douban：http://pypi.douban.com/simple/
3. Tuna：https://pypi.tuna.tsinghua.edu.cn/simple/
4. USTC：http://pypi.mirrors.ustc.edu.cn/simple/

## Anaconda

Tuna 源，配置 `~/.condarc`：

```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

Linux 中 Anaconda GUI Desktop 文件配置参考：

```
[Desktop Entry]
Version=1.0
Type=Application
Name=Anaconda
Exec=/home/ivan/anaconda3/bin/anaconda-navigator
Icon=/home/ivan/anaconda3/pkgs/anaconda-navigator-1.9.7-py37_0/lib/python3.7/site-packages/anaconda_navigator/static/images/anaconda-icon-256x256.png
Categories=Development;
Terminal=false
StartupNotify=true
StartupWMClass=Anaconda-Navigator
Actions=notebook;jupyterlab;

[Desktop Action notebook]
Name=Jupyter Notebook
Exec=/home/ivan/anaconda3/bin/jupyter notebook

[Desktop Action jupyterlab]
Name=JupyterLab
Exec=/home/ivan/anaconda3/bin/jupyter lab
```

## Thefuck

这是一个 Python 写的工具，可以尝试帮助自动更正上一条指令中的错误。

使用 pip 安装：

```bash
# 系统安装
pip install thefuck

# 用户安装
pip install thefuck --user
```

配置终端程序 RC：

```bash
eval $(thefuck --alias)
```



# GUI 工具

## 浏览器代理

特别列出，非常建议使用 Proxy SwitchyOmega。<span class="wikipage-warn">Microsoft Edge 请于 Chrome Web Store 下载，被曝光 Edge 商店里的该插件有后门，现在情况未知。</span>
