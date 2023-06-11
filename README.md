# ubuntu-20.04-version-ctrl
我自己用的ubuntu20.04版本管理，记录变化

# 2306101435

1. 原始的20.04
2. 设置登录用户为 guof
3. git clone 了 ShFiles
4. 把 my_proxy.sh 复制到了 /etc/profile.d 下面（可以开机自启动了）

# 2306102317

远程桌面还是有点卡顿的

1. 安装了 gnome 桌面

        apt-mark hold acpid acpi-support
        apt install ubuntu-desktop gnome -y

2. 设置了xrdp

        sudo apt install xrdp
        sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
        sudo systemctl restart xrdp

3. 写入了 ~/.xsessionrc

        export GNOME_SHELL_SESSION_MODE=ubuntu
        export XDG_CURRENT_DESKTOP=ubuntu:GNOME
        export XDG_DATA_DIRS=/usr/share/ubuntu:/usr/local/share:/usr/share:/var/lib/snapd/desktop
        export WAYLAND_DISPLAY=
        export XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg

4. 成功登录桌面 -- 当然，为了流畅，还是要设置一下远程桌面连接的（windows端）

5. 执行 `bash /home/guof/ShFiles/my_set_lang.sh` 设置语言为中文

6. `apt purge firefox firefox-locale-zh-hans` 卸载了火狐 （为什么会安装 chromium-browser* ？）(不要用gnome自带的软件管理器，会变得不幸，他用的snap)

7. 安装了谷歌浏览器，并放到了收藏栏（收藏栏给我放到了最底下，并且默认隐藏）

8. 安装java8

9. 安装谷歌拼音，因为安装了界面，所以不需要写入机器码（ibus也不用删除，如果写了my_fcitx.sh到/etc/profile.d里，右上角会有两个键盘，不用奇怪）

10. 安装idea并进行配置，并放到了侧边栏

11. 安装好了vscode 并且导入配置

12. 打开了上下键补全命令

        sudo vim /etc/inputrc

        把里面的5~改成A，6~改成B了，是哪一个看里面的说明就是了

13. 安装zip unzip（不需要，安装桌面的时候会自动安装）

        sudo apt update && sudo apt install zip unzip -y

14. 配置了git
