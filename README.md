# Note about Peration and Maintenance

1. [How to update Linux offline](offline_source.md)
1. [How to setup SSH service](ssh.md)
1. [How to setup VNC service](vnc.md)
1. [How to setup Jira service](jira.md)
1. [How to setup Confluence service](confluence.md)

-------------
## 将主目录下的中文目录都修改为英文
```bash
export LANG=en_US
xdg-usr-dirs-gtk-update  # Chose Yes
export LANG=zh_CN.UTF-8
xdg-usr-dirs-gtk-update  # Chose No and No more ask.
```
or edit file `~/.config/user-dirs.dirs`, mkdir, and relogin.

## 安装搜狗中文拼音输入法
### 问题
kali升级后无法安装使用搜狗输入法，提示信息：
```bash
没有可用的软件包 fcitx-libs-qt，但是它被其它的软件包引用了。
这可能意味着这个缺失的软件包可能已被废弃，
或者只能在其他发布源中找到
然而下列软件包会取代它：
  libfcitx-qt0
```
### 解决方法
1. 下载（2017年11月2日新版）`sogoupinyin_2.2.0.0102_amd64.deb`
2. 运行bash命令：
```bash
mkdir tmp
dpkg-deb -R sogoupinyin_2.1.0.0086_amd64.deb tmp
vi tmp/DEBIAN/control
```
3. 将其中依赖关系描述中的`fcitx-libs-qt`替换为`libfcitx-qt0`，类似下面：
> Depends: fcitx (>= 1:4.2.8.3-3~), fcitx-frontend-gtk2, fcitx-frontend-gtk3, fcitx-frontend-qt4,
> fcitx-module-kimpanel, im-config, libopencc2 | libopencc1, lsb-release, unzip, zip, x11-utils,
> fcitx-libs (>= 4.2.7), libfcitx-qt0 (>= 4.2.7), libc6 (>= 2.8), libgcc1 (>= 1:4.1.1),
> libgdk-pixbuf2.0-0 (>= 2.22.0), libglib2.0-0 (>= 2.16.0), libidn11 (>= 1.13), libnotify4 (>= 0.7.0),
> libqt4-dbus (>= 4:4.8.0), libqt4-declarative (>= 4:4.8.0), libqt4-network (>= 4:4.8.0),
> libqtcore4 (>= 4:4.8.0), libqtgui4 (>= 4:4.8.0), libqtwebkit4, libstdc++6 (>= 4.6), libx11-6, zlib1g (>= 1:1.2.0)
4. 运行bash命令：
```bash
dpkg-deb -b tmp/ sogoupinyin-fixed.deb
dpkg -i sogoupinyin-fixed.deb
```
5. 重启。

