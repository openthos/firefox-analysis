# firefox-analysis
## 环境搭建
目前是在ubuntu15下搭建的环境(16jdk太新，14太旧较难找到32位的libstdc++)

sudo add-apt-repository ppa:openjdk-r/ppa

sudo apt-get update

sudo apt-get install yasm git python-dbus mercurial automake autoconf autoconf2.13 build-essential ccache python-dev python-pip python-setuptools unzip uuid zip zlib1g-dev openjdk-7-jdk wget libncurses5:i386 libstdc++6:i386 zlib1g:i386

mkdir /firefox

cd /firefox

curl -sf -L https://static.rust-lang.org/rustup.sh

chmod +x rustup.sh

./rustup.sh

./rustup.sh --add-target=i686-linux-android


## 源码下载

git clone https://github.com/mozilla/gecko-dev.git

cd gecko-dev

git checkout -b r49-2016101919 remotes/origin/MOBILE4902_2016101919_RELBRANCH

## 预处理

./mach bootstrap
选择4（Firefox for android）
等下载完ndk后，即可强行停止（ctrl+c），对于r49分支，必须要用此对应的ndk，高版本的工具链内部会报错，低版本的功能不全。
此时ndk的路径是/root/.mozbuild/android-ndk-r11b

然后再找一个尽可能高版本的SDK，例如在180服务器上有一个，
cd ../
scp lh@192.168.0.180:/home/lh/wjx/sdk.tar.gz .
tar -zxvf sdk.tar.gz
解压后会在/firefox/Sdk目录下。

cd gecko-dev
vi mozconfig
增加以下内容
ac_add_options --enable-application=mobile/android
ac_add_options --target=i386-linux-android
ac_add_options --with-android-sdk="/firefox/Sdk"
ac_add_options --with-android-ndk="/root/.mozbuild/android-ndk-r11b"
mk_add_options MOZ_OBJDIR=./objdir-all
mk_add_options MOZ_MAKE_FLAGS="-j4"

vi mobile/android/confvars.sh
注释掉MOZ_INSTALL_TRACKING（在第49行），否则编译失败。

## 编译打包安装 
./mach build
./mach package
./mach install
大概需要1个小时左右（硬盘较快，40分钟左右；硬盘较慢，则1小时20分钟左右）。
如果是在OPENTHSO chroot ubuntu15下，则直接安装到本机，并可在开始菜单中直接运行。

## 参考链接
https://github.com/openthos/system-analysis/edit/master/firefox/build.mdhttps://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions/Simple_Firefox_for_Android_build
