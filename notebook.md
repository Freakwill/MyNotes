# My mac notebook

## Build sh files:

create a path, `~/mysh`;
new a file under the path, and write the codes

```bash
# ~/.bash_profile
export PATH=/Users/william/mysh:$PATH  #dd mysh where I save .sh files
```

run the file, for example 

```bash
test.sh (chmod +x ./mysh/openpy.sh)
```



## build link

```bash
ln -s /usr/local/lib/ruby/gems/2.5.0/gems/jekyll-3.8.5/exe/jekyll /usr/local/bin/jekyll
```





## Install asymptote for mac (terminal commands)

```bash
cd ~/Desktop
tar -xvzf asymptote-x.xx.src.tgz
cd asymptote-x.xx
curl -O http://www.hpl.hp.com/personal/Hans_Boehm/gc/gc_source/gc-7.1.tar.gz
./configure
make all
sudo make install
```

## configuration of asy in mac

place `config.asy` in '%USERPROFILE%/.asy/'

```c
import settings;
// outformat="eps";
//batchView=false;
//interactiveView=true;
//batchMask=false;
//interactiveMask=true;
home = "/Users/william/";
dir = home + "Folders/asymptote/mywork";
```

## change the screencapture type

```bash
defaults write com.apple.screencapture type jpg

defaults write com.apple.screencapture location ~/Pictures/
```



## color for mac terminal

### 打开 .bash_profile并编辑
```bash
export CLICOLOR=1
export LSCOLORS=1212121212121212121212
```

### 1对应前景色，2对应背景色。共有11组:

1.   directory
2.   symbolic link
3.   socket
4.   pipe
5.   executable (可执行文件，x权限)
6.   block special
7.   character special
8.   executable with setuid bit set (setuid=Set User ID，属主身份)
9.   executable without setgid bit set
10.   directory writable to others, with sticky bit
11.   directory writable to others, without sticky bit

### 字母代表的颜色如下：

```
a     black
b     red
c     green
d     brown
e     blue
f     magenta
g     cyan
h     light grey
A     bold black, usually shows up as dark grey
B     bold red
C     bold green
D     bold brown, usually shows up as yellow
E     bold blue
F     bold magenta
G     bold cyan
H     bold light grey; looks like bright white
x     default foreground or background
```

## 建立.开头的文件夹(隐藏文件夹)
```bash
mkdir .asy
```

## bash commands

`Ctrl + L` # clear the screen

```bash
open .  # open Finder
```

## bash commands about tex

```bash
texhash      # update the packages of tex
texdoc ctex  # document of ctex
```

## 7z压缩命令
```bash
# 7z a -t7z destination source
7z a -t7z folder.7z /Users/william/Teaching/毕业论文/folder
```

## Mac 编译 C

```bash
touch Hello.c
# edit Hello.c
gcc (g++) Hello.c # compile Hello.c to *.out
path/*.out # execute
```

## 绘制 uml 图

```bash
pyreverse -ASmy -o png ~/Python/mywork/fcool.py
```



## APP 安全性设置

```bash
sudo spctl --master-disable
sudo spctl --master-enable
```



## 启动 MongoDB

```bash
brew services start mongodb //现在和开机自启动mongo的话使用命令:
mongod --config /usr/local/etc/mongod.conf //不在后台启动mongo服务器使用
```

```bash
brew services stop  mongodb // 关闭MongoDB
```



## 导入shell文件

```bash
source path/to/file.sh
```



## 设置 env 下载Python第三方库（需要gcc）

```bash
env ARCHFLAGS="-arch x86_64" pip3 install --upgrade regex
```



##  Matlab 启动路径

```userpath('/Users/william/Programming/MATLAB')
userpath('/Users/william/Programming/MATLAB')
```



## 查ip地址

```b
ifconfig

en0 以太网地址
```



## selfcontrol.app

selfcontrol 配置/查看/调用

```bash
defaults write org.eyebeam.SelfControl BlockDuration -int 1440
defaults write org.eyebeam.SelfControl HostBlacklist -array facebook.com news.ycombinator.com
defaults read org.eyebeam.SelfControl
sudo /Applications/SelfControl.app/Contents/MacOS/org.eyebeam.SelfControl $(id -u $(whoami)) --install

defaults write org.eyebeam.SelfControl MaxBlockLength -int 43200
defaults delete org.eyebeam.SelfControl
```



## Homebrew speed up

```bash
cd “$(brew –repo)”
//清华镜像源
git remote set-url origin git://mirrors.tuna.tsinghua.edu.cn/homebrew.git
//中科大镜像源
git remote set-url origin http://mirrors.ustc.edu.cn/homebrew.git
//coding.net
git remote set-url origin https://git.coding.net/homebrew/homebrew.git


cd “$(brew –repo)/Library/Taps/homebrew/homebrew-core”
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

cd $home && brew update

// 官方源 https://github.com/Homebrew/brew.git
```



## curl

`curl`命令出现：

`curl: (7) Failed to connect to 127.0.0.1 port 1086: Connection refused`

~/.curlrc (`curl`配置文件)

`socks5 = "127.0.0.1:1086"`注释掉就好了。



## shell calls printer

```bash
lpr -P printer_name file_name.txt 
# printer_name: the name of the printer you use on your system.
# file_name.txt: the name of the text file used for printing.
lpq
lprm
lpstat
```



## 电脑盒盖省电

```bash
pmset -g
sudo pmset -a hibernatemode 25
sudo pmset -b tcpkeepalive 0  # 盒盖断网
```



## Python

### uninstall python

```
delete /Library/Frameworks/Python.framework

delete symlink under /usr/local/bin/ of python
```


### run python3 defaultly

```bash
alias python=/Library/Frameworks/Python.framework/Versions/3.5/bin/python3.5
```

### idle configuration

目录：`~\.idlerc\config-*.cfg` (Windows 7下路径为：`C:\Users\<用户名>\.idlerc\`)



### 设置为可执行文件

script.py 第一行为 `#!/usr/bin/env python`

```bash
chmod u+x script.py
```



### pip

```bash
pip install -r requirement.txt
# cd 到项目目录，生成requirements.txt
pip freeze > requirements.txt
```



## 生成二维码

```bash
myqr https://github.com -p bear.png -c
```



## Hot key

### shift input method

`ctrl + whitespace`

### cut screen

`shift+cmd+4`



## Swift

```bash
swift package generate-xcodeproj
```



### 使用

首先，创建

```
$ cd ~/Desktop
$ mkdir helloSwiftPM 
$ cd helloSwiftPM
```

然后

```
swift build --init
swift build

# 运行
.build/debug/helloSwiftPM
Hello, world!
```