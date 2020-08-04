# Mac 笔记

[TOC]



## Shell

### 创建 sh 文件:

- create a path, `~/mysh`;
- new a file under the path, and write the codes

```bash
# ~/.bash_profile
export PATH=/Users/william/mysh:$PATH  #dd mysh where I save .sh files
```

- run the file, for example 

```bash
test.sh (chmod +x ./mysh/openpy.sh)
```



### 建立 link

```bash
ln -s /usr/local/lib/ruby/gems/2.5.0/gems/jekyll-3.8.5/exe/jekyll /usr/local/bin/jekyll
```



### 安装 asymptote（Mac）

```bash
cd ~/Desktop
tar -xvzf asymptote-x.xx.src.tgz
cd asymptote-x.xx
curl -O http://www.hpl.hp.com/personal/Hans_Boehm/gc/gc_source/gc-7.1.tar.gz
./configure
make all
sudo make install
```

### 配置 asy (Mac)

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

### 改变 screencapture 类型

```bash
defaults write com.apple.screencapture type jpg

defaults write com.apple.screencapture location ~/Pictures/
```



### mac terminal 颜色设置

#### 打开 .bash_profile并编辑

```bash
export CLICOLOR=1
export LSCOLORS=1212121212121212121212
```

#### 1对应前景色，2对应背景色。共有11组:

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

#### 字母代表的颜色如下：

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

### 建立.开头的文件夹(隐藏文件夹)

```bash
mkdir .asy
```

```bash
open .  # open Finder
```

### tex 命令

```bash
texhash      # update the packages of tex
texdoc ctex  # document of ctex
```

### 7z压缩命令

```bash
# 7z a -t7z destination source
7z a -t7z folder.7z /Users/william/Teaching/毕业论文/folder
```

### Mac 编译 C

```bash
touch Hello.c
# edit Hello.c
gcc (g++) Hello.c # compile Hello.c to *.out
path/*.out # execute
```

### 绘制 uml 图

```bash
pyreverse -ASmy -o png ~/Python/mywork/fcool.py
```



### APP 安全性设置

```bash
sudo spctl --master-disable
sudo spctl --master-enable
```



### 系统服务管理

```shell
brew services  # == systemctl in Linux
# https://www.jianshu.com/p/90939b788004
```

#### 启动 MongoDB

```bash
brew services start mongodb //现在和开机自启动mongo的话使用命令:
mongod --config /usr/local/etc/mongod.conf //不在后台启动mongo服务器使用
```

```bash
brew services stop  mongodb // 关闭MongoDB
```

#### 启动MySQL

```shell
brew services start mysql
mysql.service start
sudo mysql
```



### 导入shell文件

```bash
source path/to/file.sh
```



### 设置 env 下载Python第三方库（需要gcc）

```bash
env ARCHFLAGS="-arch x86_64" pip3 install --upgrade regex
```



### 查ip地址

```b
ifconfig

en0 以太网地址
```



### ssh

```shell
# 解决 SSH Permission denied 错误
sudo systemsetup -f -setremotelogin on
```



### selfcontrol.app

selfcontrol 配置/查看/调用

```bash
defaults write org.eyebeam.SelfControl BlockDuration -int 1440
defaults write org.eyebeam.SelfControl HostBlacklist -array facebook.com news.ycombinator.com
defaults read org.eyebeam.SelfControl
sudo /Applications/SelfControl.app/Contents/MacOS/org.eyebeam.SelfControl $(id -u $(whoami)) --install

defaults write org.eyebeam.SelfControl MaxBlockLength -int 43200
defaults delete org.eyebeam.SelfControl
```

### curl

`curl`命令出现：

`curl: (7) Failed to connect to 127.0.0.1 port 1086: Connection refused`

~/.curlrc (`curl`配置文件)

`socks5 = "127.0.0.1:1086"`注释掉就好了。



### 调用 printer

```bash
lpr -P printer_name file_name.txt 
# printer_name: the name of the printer you use on your system.
# file_name.txt: the name of the text file used for printing.
lpq
lprm
lpstat
```



### 电脑盒盖省电

```bash
pmset -g
sudo pmset -a hibernatemode 25
sudo pmset -b tcpkeepalive 0  # 盒盖断网
```



### 扩展属性

```bash
ls -l
// drwxr-xr-x@ //显示扩展属性

// 清除扩展属性
xattr -c fiename
xattr -rc directory
```



### wifi

```bash
airport -s
airport en0 sniff 6 //抓包
aircrack-ng /tmp/airportSniff8g0Oex.cap //破解
aircrack-ng -w dict.txt -b bc:46:99:df:6c:72 /tmp/airportSniffdaMCjH.cap
```



### expect: input password automatically

```shell
#!/usr/bin/expect
set timeout 3               
...  
expect "password:"
send "******\n"                                       
interact
```



### 小技巧

输出到应用程序

```shell
open test.txt
ls -la | open -f
ls -la | open -f -a TextMate
ls -la | pbcopy
```



剪贴板内容`pbpaste > test.txt`

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

pip install -i https://pypi.tuna.tsinghua.edu.cn/simple <package> # 加速，清华源
# 豆瓣源
-i https://pypi.doubanio.com/simple/
```

配置：

```bash
# mkdir ~/.pip
# vim ~/.pip/pip.conf

# 阿里源
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com

# 豆瓣源
[global]
index-url = http://pypi.douban.com/simple
trusted-host = pypi.douban.com
```



### youtube-dl

```bash
--no-check-certificate # option for Youtube - ssl: certificate_verify_failed
```



### nltk 下载时 ssl错误

先执行再下载

`/Applications/Python 3.6/Install Certificates.command`

自定义搜索路径

```python
from nltk import data
data.path.append(r"~/Programming/")
```



### matplotlib 字体

在文件`matplotlib/mpl-data/matplotlibrc`中设置

```
font.family         : sans-serif
font.sans-serif     : SimHei, DejaVu Sans, Bitstream Vera Sans, Computer Modern Sans Serif, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif
```

将字体`SimHei`拷贝到`fonts/ttf`子文件夹中。

必要时重新加载

```python
from matplotlib.font_manager import _rebuild
_rebuild()
```



### install rpy2

使用homebrew安装的gcc



### pdb

#### 脚本

```python
import pdb
pdb.run('...')
```



#### 命令行

```bash
python -m pdb ...
```



### install pygraphviz

设置 --install-option

```bash
version=2.44.1
pip install pygraphviz --install-option="--include-path=/usr/local/Cellar/graphviz/${version}/include" --install-option="--library-path=/usr/local/Cellar/graphviz/${version}/lib"

```





## 生成二维码

```bash
myqr https://github.com -p bear.png -c
```





## Homebrew

### Homebrew 加速

#### 清华源

```bash
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git

brew update
```

#### 复原

```bash
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew.git

git -C "$(brew --repo homebrew/core)" remote set-url origin https://github.com/Homebrew/homebrew-core.git

git -C "$(brew --repo homebrew/cask)" remote set-url origin https://github.com/Homebrew/homebrew-cask.git

brew update

```

#### 其他源

```bash
cd "$(brew --repo)"
//中科大镜像源
git remote set-url origin http://mirrors.ustc.edu.cn/homebrew.git
//coding.net
git remote set-url origin https://git.coding.net/homebrew/homebrew.git

// homebrew-core
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

cd $home && brew update

// 官方源 https://github.com/Homebrew/brew.git

//homebrew-bottles
HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles
```



### brew 系统服务

```shell
brew services
```



## Hot key

### shift input method

`ctrl + whitespace`

### cut screen

`shift+cmd+4`

`Ctrl + L` # clear the screen

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



## Lua

### 添加搜索路径

```lua
package.path = "/Users/William/Programming/Lua/?.lua"
require "mmcut"
```



## R

### 安装第三方

```R
install.packages('package_name')
detach("package:tseries", unload=TRUE)   # 卸载

# 修改默认安装目录
myPaths <- .libPaths()   # get the paths
myPaths <- c(myPaths[2], myPaths)  # switch them
.libPaths(myPaths)  # reassign them
```



### 加载

```R
library(randomForest)  # 加载randomForest
library('randomForest')  # 或者

p<-'randomForest'
library(p)  #报错
library(p,character.only=T)  #正常加载randomForest
```



### 其他

```R
installed.packages()  # 查看包
update.packages()  # 更新包
.libPaths()  # 包安装路径；设置环境变量R_LIBS

Sys.setenv(LANGUAGE = "en")  # R改成英文
```



## SublimeText

设置自动补全

`“auto_complete_selector”: “source, text”,`



## 第三方登录网易邮箱

打开网页设置

http://config.mail.163.com/settings/imap/index.jsp?uid=YOUR_EMAIL_NAME@163.com



## Matlab

###  Matlab 启动路径

```userpath('/Users/william/Programming/MATLAB')
userpath('/Users/william/Programming/MATLAB')
```



## Erlang

### 编译

```shell
erlc filename.erl  # filename.beam
```

### 运行

filename.erl 首行

`#!/usr/bin/env escript`

`$ filename.erl`

### 导入

```erlang
c(filename)
```



## Go

### 包管理

#### GOPATH

内部或外部包都在目录`$GOPATH/src`

`go get`也会安装到这个目录

`import package` 就会在这个目录下查找



## MySQL

my.cnf位置

`/etc/my.cnf  /etc/mysql/my.cnf  /usr/local/mysql/etc/my.cnf  ~/.my.cnf`

`/usr/local/etc/my.cnf` (由Homebrew安装)



解决pymysql 1045Error

1. 输入mysql -hlocalhost -uroot -p，回车输入密码，连接mysql

2. 输入ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY



## Sublime Text

Sublime Text 使用 login shell 获得 environment variables. 默认login shell 是 `zsh` 不是 `bash`, 使用 `.zprofile`配置.