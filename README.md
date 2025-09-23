# 手机上安装完整python

## 介绍
在**Termux**中通过**proot-distro**容器安装**Debian**系统，并配置了完整的**Python**环境。
本教程适合希望在手机上体验接近桌面级Linux的用户或者没有电脑的用户，支持：科学计算、爬虫开发、GUI、数据可视化与图像处理等，让手机也能变身为随身携带的开发环境，适合学习、实验和轻量级开发。

Termux和Termux: X11安装包：
[点击下载Termux.rar](https://gitee.com/hgn977/install-python-on-your-phone/releases/download/v/Termux.rar)

## 安装教程

### 1. Termux 基础配置
获取存储权限
```
termux-setup-storage
```
换清华源
```
termux-change-repo
```
更新软件源
```
pkg update && pkg upgrade
```
安装必要软件
```
pkg install x11-repo pulseaudio proot-distro wget git android-tools
```
```
pkg install termux-x11-nightly
```
### 2. 安装Debian
```
proot-distro install debian
```
进入Debian系统
```
proot-distro login debian
```
### 3. 配置软件源与依赖
更新软件源
```
apt update && apt upgrade
```
安装基础软件等各种依赖
```
apt install sudo nano adduser xfce4 xfce4-terminal mousepad nomacs vlc python3 python3-pip python3-tk python3-venv dbus-x11 build-essential cmake pkg-config python3-dev python3-wheel git curl wget unzip xz-utils liblapack-dev libblas-dev gfortran libopenblas-dev libffi-dev libgmp-dev libjpeg-dev libpng-dev libtiff-dev libwebp-dev libfreetype-dev liblcms2-dev libharfbuzz-dev libfribidi-dev libxcb1-dev libxkbcommon-dev libxkbcommon-x11-dev libsqlite3-dev libpq-dev libmariadb-dev libmariadb-dev-compat unixodbc-dev libodbccr2 libodbc2 libssl-dev libcurl4-openssl-dev zlib1g-dev libbz2-dev liblzma-dev libgl1-mesa-dev libglu1-mesa-dev libgles2-mesa-dev libegl1-mesa-dev libxrender-dev libxi-dev libxcomposite-dev libxcb-xinerama0 libxcb-cursor0 libxcb-icccm4 libxcb-keysyms1 libxcb-render-util0 libx11-xcb-dev libxrandr-dev libxtst-dev libxml2-dev libxslt1-dev libyaml-dev libreadline-dev libncurses-dev libevent-dev libsnappy-dev libhdf5-dev libavcodec-dev libavformat-dev libswscale-dev libavdevice-dev libsndfile1-dev portaudio19-dev locales fonts-wqy-zenhei fonts-wqy-microhei tzdata htop fonts-noto-color-emoji -y
```
### 4. 创建普通用户
创建用户名：demo
```
adduser demo
```
打开/etc/sudoers文件
```
nano /etc/sudoers
```
```
demo ALL=(ALL:ALL) ALL
```
测试是否成功
```
su - demo
```
```
sudo whoami
```
输入exit退出，进入普通用户
```
proot-distro login debian --user demo
```
### 5. 设置中文环境与时区
设置默认环境为zh_CN.UTF-8
```
sudo dpkg-reconfigure locales
```
- 第一次输入**zh_CN.UTF-8 UTF-8**对应的编号
- 第二次输入**zh_CN.UTF-8**对应的编号

使用root权限打开environment文件
```
sudo nano /etc/environment
```
添加至/etc/environment
```
LANGUAGE="zh_CN:zh"
LANG="zh_CN.UTF-8"
LC_ALL=zh_CN.UTF-8
```
- 退出重新进入Debian

查看是否已经设置为`zh_CN.UTF-8`
```
locale
```
设置时区
```
sudo dpkg-reconfigure tzdata
```
- 第一次输入**Asia**对应的编号
- 第二次输入**Shanghai**对应的编号
查看当前时区
```
date
```

### 6. 下载启动脚本
退出Debian，回到Termux环境
```
wget https://gitee.com/hgn977/install-python-on-your-phone/raw/master/debian/debianxfce4.sh
```
给脚本添加执行权限
```
chmod +x debianxfce4.sh
```
运行
```
./debianxfce4.sh
```

### 7. 安卓12+解除进程限制
1. 打开开发者模式
2. 开启无线调试
3. 进行配对
```
adb pair ip:port
```
4. 建立连接
```
adb connect ip:port
```
- 取消连接
```
adb disconnect ip:port
```
5. 永久关闭同步
```
adb shell device_config set_sync_disabled_for_tests persistent 
```
6. 设置最大进程
```
adb shell device_config put activity_manager max_phantom_processes 1024
```
- 查看设置的进程数
```
adb shell device_config get activity_manager max_phantom_processes
```
- 恢复默认
```
adb shell settings delete global device_config_sync_disabled_for_tests
```
```
adb shell device_config delete activity_manager max_phantom_processes
```
### 8. 创建Python虚拟环境
创建虚拟环境
```
python3 -m venv venv
```
激活虚拟环境
```
source venv/bin/activate
```
退出虚拟环境
```
deactivate
```
### 9. 安装谷歌浏览器
```
sudo apt install chromium chromium-l10n chromium-driver
```
- 搜索引擎插件：`New Tab Redirect`

启动谷歌浏览器
```
chromium --no-sandbox
```
- **--no-sandbox**表示禁用沙箱模式（sandbox），由于环境限制，在Termux/proot容器中，沙箱无法启动。

### 10. 安装VSCcode
访问VSCode官网进行下载：https://code.visualstudio.com/Download
```
https://code.visualstudio.com/Download
```
- 下载ARM64版.deb软件包

安装VSCode
```
sudo dpkg -i 包名.deb
```
如果遇到缺少依赖报错，使用sudo apt -f install修复依赖
```
sudo apt -f install
```
启动vscode
```
code --no-sandbox
```
设置中文语言
- 打开扩展（Extensions）面板
- 搜索`chinese`
- 点击`Install`进行安装
- 重启生效

安装Python扩展
- 打开扩展（Extensions）面板
- 搜索`python`
- 点击安装

设置Python解释器环境为虚拟环境
- 打开命令面板
-- 快捷键：`Ctrl + Shift + P`
- 输入并选择
```
Python: Select Interpreter
```
### 11. 安装Pycharm
访问Pycharm官网进行下载：https://www.jetbrains.com/zh-cn/pycharm/download/?section=linux
```
https://www.jetbrains.com/zh-cn/pycharm/download/?section=linux
```
- 下载ARM64版.tar.gz压缩包

解压压缩包
```
tar -zxvf pycharm-*.tar.gz
```
进入Pycharm文件夹内存放运行文件的位置
```
cd pycharm-*/bin
```
运行Pycharm
```
./pycharm.sh
```
将pycharm文件夹移动至/opt文件夹内
```
mv pycharm-* /opt
```
设置Pycharm启动器
```
sudo nano /usr/share/applications/pycharm.desktop
```
将以下内容添加至`pycharm.desktop`文件内
```
[Desktop Entry]
Version=1.0
Type=Application
Name=PyCharm
Icon=/opt/pycharm-*/bin/pycharm.png
Exec="/opt/pycharm-*/bin/pycharm.sh" %f
Comment=Python IDE for Professional Developers
Categories=Development;IDE;
Terminal=false
StartupWMClass=jetbrains-pycharm
```



## 版权与许可证

本仓库仅用于学习、演示和分发方便。Termux 和 Termux:X11 的 APK 文件原始版权属于 **The Termux Project**。

- Termux 官方主页：https://termux.com/
- Termux:X11 官方主页：https://wiki.termux.com/wiki/X11

这些软件遵循 **Apache License 2.0**，详见原项目 LICENSE 文件。

Apache License 2.0 文档：https://www.apache.org/licenses/LICENSE-2.0

本仓库没有修改 APK，用户下载后请遵守原项目的开源许可。



