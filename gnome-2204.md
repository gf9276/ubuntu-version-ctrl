<!-- TOC -->

- [1. ubuntu22.04](#1-ubuntu2204)
- [2. 版本 2306111955](#2-版本-2306111955)
  - [2.1. 直接就是从微软商店下载下来的](#21-直接就是从微软商店下载下来的)
  - [2.2. 在 wsl.conf 下加入了](#22-在-wslconf-下加入了)
  - [2.3. 打开上下键补全历史命令](#23-打开上下键补全历史命令)
  - [2.4. zip](#24-zip)
  - [2.5. ~/.gitconfig 写入用户信息](#25-gitconfig-写入用户信息)
  - [2.6. 代理配置](#26-代理配置)
  - [2.7. 设置root密码 -- 926219](#27-设置root密码----926219)
  - [2.8. 安装apt-fast](#28-安装apt-fast)
  - [2.9. 设置snap代理（22.04火狐和软件商店用的都是snap，然鹅这玩意不会读取全局代理）](#29-设置snap代理2204火狐和软件商店用的都是snap然鹅这玩意不会读取全局代理)
  - [2.10. 锁住！](#210-锁住)
  - [2.11. 安装gnome（安装snap-firefox会等一会儿）](#211-安装gnome安装snap-firefox会等一会儿)
  - [2.12. 安装xrdp](#212-安装xrdp)
  - [2.13. 重装dbus](#213-重装dbus)
  - [2.14. 修复网络](#214-修复网络)
  - [2.15. 重启并远程连接](#215-重启并远程连接)
- [3. 版本 2306112027](#3-版本-2306112027)
  - [3.1. 执行配置中文 zh\_CN\_utf-8](#31-执行配置中文-zh_cn_utf-8)
  - [3.2. 安装谷歌拼音](#32-安装谷歌拼音)
  - [3.3. 好像没有变成中文](#33-好像没有变成中文)
  - [3.4. 细枝末节](#34-细枝末节)
  - [3.5. 下载了vscode](#35-下载了vscode)
- [4. 版本 2306112149](#4-版本-2306112149)
  - [4.1. 安装谷歌浏览器，我还是喜欢谷歌一点](#41-安装谷歌浏览器我还是喜欢谷歌一点)
  - [4.2. 卸载火狐浏览器（我只卸载了snap的）](#42-卸载火狐浏览器我只卸载了snap的)
  - [4.3. 安装jdk8](#43-安装jdk8)
  - [4.4. 安装idea](#44-安装idea)
  - [4.5. 安装了miniconda](#45-安装了miniconda)
- [5. 版本 2308161708](#5-版本-2308161708)
- [6. now](#6-now)

<!-- /TOC -->

# 1. ubuntu22.04
记录ubuntu22.04使用情况


# 2. 版本 2306111955

## 2.1. 直接就是从微软商店下载下来的

## 2.2. 在 wsl.conf 下加入了

```        
[user]
default=guof
```
  
## 2.3. 打开上下键补全历史命令

```
sudo vim /etc/inputrc
```

把里面的5~改成A，6~改成B了，是哪一个看里面的说明就是了


## 2.4. zip

直接输入以下指令

```
apt update && apt install zip unzip
```

## 2.5. ~/.gitconfig 写入用户信息

git clone 了几个文件夹，然后配置了用户信息

```
git config --global user.name "gf9276"
```
```
git config --global user.email "927621609@qq.com"
```

## 2.6. 代理配置

```
cd ~/ && git clone https://github.com/gf9276/ShFiles.git && cd ShFiles && git pull
```

```
sudo mv ~/ShFiles/my_proxy.sh /etc/profile.d/
```

## 2.7. 设置root密码 -- 926219

```
sudo passwd root
```

## 2.8. 安装apt-fast

切换成root用户，su -l，执行

```
bash /home/guof/ShFiles/my_install_aptfast.sh
```

apt --> 12 --> No

## 2.9. 设置snap代理（22.04火狐和软件商店用的都是snap，然鹅这玩意不会读取全局代理）

看看代理ip是啥
```
echo $http_proxy
```
分开执行（下面那个也是没有s的）
```
snap set system proxy.http="http://172.22.160.1:7890"

snap set system proxy.https="http://172.22.160.1:7890"
```

## 2.10. 锁住！

安装gnome前，锁住三个东西，避免变得不幸

```
apt-mark hold acpid acpi-support modemmanager
```

acpid是管理电源的，systemctl检测到容器（就是wsl2）会因为这玩意卡死的

[参考链接](https://github.com/microsoft/WSL/discussions/9350#discussioncomment-6119188)

modemmanager是管理网络的，也是检测到容器网络设置直接卡死

[参考链接](https://forum.ubuntu.com.cn/viewtopic.php?t=490583)

## 2.11. 安装gnome（安装snap-firefox会等一会儿）

先更新

```
apt update -y && apt upgrade -y
```

再下载（apt-fast快很多，不然大半个小时）

```
apt-fast install ubuntu-desktop gnome -y
```

执行完，再执行这个，看看有没有漏的

```
apt install ubuntu-desktop gnome -y
```

## 2.12. 安装xrdp

1. 安装

安装xrdp
```
apt install xrdp
```
换端口，防止和win打架，这是匹配更换指令，把3389字符串改成3390
```
sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
```
重启服务
```
systemctl restart xrdp
```

2. 设置

写入信息（切回普通用户）
```
vim ~/.xsessionrc
```
内容如下
```
export GNOME_SHELL_SESSION_MODE=ubuntu
export XDG_CURRENT_DESKTOP=ubuntu:GNOME
export XDG_DATA_DIRS=/usr/share/ubuntu:/usr/local/share:/usr/share:/var/lib/snapd/desktop
export WAYLAND_DISPLAY=
export XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
```

## 2.13. 重装dbus

必须重装，不然会不幸（su -l变回root）

```
apt install --reinstall dbus
```

下面的不要执行，我只是记录一下（这玩意权限是`-rwsr-xr--`就是对的）
```
sudo chmod 4754 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
sudo chown root:messagebus /usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

## 2.14. 修复网络

这文件就是空的，不过这么干能把网络图标给搞回来。。。

不搞这个软件与更新进不去，说识别不到网络。。。

```
touch /etc/NetworkManager/conf.d/10-globally-managed-devices.conf
```

## 2.15. 重启并远程连接

测试了一下打开设置

打开软件与更新

https://www.testufo.com/ 显示帧率59（实际估计只有40）

一切正常

www.google.com

www.youtube.com

www.github.com

也都连接正常


---

# 3. 版本 2306112027

开始个性化配置

## 3.1. 执行配置中文 zh_CN_utf-8

```
bash /home/guof/ShFiles/my_set_lang.sh
```

## 3.2. 安装谷歌拼音

```
apt -y install fcitx fonts-noto-cjk fonts-noto-color-emoji dbus-x11 fcitx-googlepinyin
```

查看一下机器码
```
cat /var/lib/dbus/machine-id
```
没有的话
```
dbus-uuidgen > /var/lib/dbus/machine-id
```

全局变量（这是为了wslg的，有了这个右上角会有两个键盘。。。但是没这个wslg用不了输入法）

```
cp /home/guof/ShFiles/my_fcitx.sh /etc/profile.d/
```

重启一下，再次进入远程桌面，选择保留文件为英文名字


## 3.3. 好像没有变成中文

打开 gnome-language-selector，它会提示没有装完

安装没装完的，然后语言换成中文（记得输入法换成Fcitx 4，省得后面调） 然后就变成中文了

执行下面的改一下快捷键
```
fcitx-configtool
```

## 3.4. 细枝末节

1. 调整终端大小
2. 侧边栏放到下面（外观那里改），颜色换成紫色了
3. `ln -s /usr/lib/wsl/lib/nvidia-smi /usr/bin/nvidia-smi` 显卡，这样就可以正常识别nvidia-smi了
4. 换了个背景
5. 这玩意还是放到文档下面好  `mv ShFiles ~/Documents/` 直接挪过去了
6. 其实我还想删掉firefox，启动好慢啊

## 3.5. 下载了vscode 

直接官网下载 .deb 后

dpkg -i 就行了

添加到收藏栏

导入配置


# 4. 版本 2306112149

## 4.1. 安装谷歌浏览器，我还是喜欢谷歌一点

1. 将目录更改为 temp 文件夹：`cd /tmp`
2. 使用 wget 下载它：`wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb`
3. 获取当前稳定版本：`dpkg -i google-chrome-stable_current_amd64.deb`
4. 修复包：`apt install --fix-broken -y`
5. 配置包：`dpkg -i google-chrome-stable_current_amd64.deb`

设置默认浏览器为谷歌，并放到侧边栏

谷歌浏览器会使用桌面的代理，所以还要配置一下桌面的代理


## 4.2. 卸载火狐浏览器（我只卸载了snap的）

这jb有两个火狐，一个apt，一个snap...

1. apt的，先找到包，然后卸载，但是22.04好像有点奇怪。而且，卸载了火狐 （为什么会安装 chromium-browser* ？）
```
dpkg --get-selections |grep firefox
```
```
sudo apt-get purge firefox firefox-locale-en firefox-locale-zh-hans
```

2. snap的，直接打开 **软件** 应用，找到firefox直接卸载就行，标签是蓝色的。。。对了，打开软件会报错，这个暂时好像没有影响，以后再看

## 4.3. 安装jdk8

```
bash /home/guof/Documents/ShFiles/my_install_java.sh
```

## 4.4. 安装idea

直接下载到 /opt 下面，然后执行 idea.sh

顺便拷贝了一下 .m2

顺便搞了一下配置

顺便装了插件 mybatisx和codeglancepro

创建桌面快捷方式

放到了收藏夹栏

图标改成小号的了

## 4.5. 安装了miniconda

并且在base环境下安装了打包工具

```
conda install -c conda-forge conda-pack
```

# 5. 版本 2308161708

* fwupd
  <details>
  <summary>不要删这个，没准还真有点用（暂时没删除）</summary>
  关闭 fwupd --> 这是一个更新固件的软件，我在容器里要这个干嘛

  好像有点奇怪，别乱用

  ```
  sudo systemctl stop fwupd.service
  sudo systemctl disable fwupd.service
  sudo systemctl mask fwupd.service
  ```

  想打开就

  ```
  sudo systemctl unmask fwupd.service
  sudo systemctl enable fwupd.service
  sudo systemctl start fwupd.service
  ```
  </details>

* 安装了nload

  ```
  sudo apt-get install nload
  ```
  启用他的命令
  ```
  nload -m
  ```
  
* 绷不住了，昨天还是59帧，今天突然变50了

* 设置dock
  
    ![](picture\配置dock行为.png)

* 安装了NoMachine

* 优化了idea的图标，嘿嘿

# 6. now

* 测井环境配置

* py3.7conda环境拷贝

* 安装mysql

* 安装minio

* git config --global core.autocrlf false

* 安装maven

* 安装hdf

* nlogging项目配置
