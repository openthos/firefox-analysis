# firefox-analysis
## 环境搭建
目前是在ubuntu15下搭建的环境(16jdk太新，14太旧较难找到32位的libstdc++)
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install yasm git python-dbus mercurial automake autoconf autoconf2.13 build-essential ccache python-dev python-pip python-setuptools unzip uuid zip zlib1g-dev openjdk-7-jdk wget libncurses5:i386 libstdc++6:i386 zlib1g:i386
curl -sf -L https://static.rust-lang.org/rustup.sh
chmod +x rustup.sh
./rustup.sh
./rustup.sh --add-target=i686-linux-android

## 源码下载
git clone https://github.com/mozilla/gecko-dev.git
cd gecko-dev
git checkout -b r49-2016101919 remotes/origin/MOBILE4902_2016101919_RELBRANCH
vi mobile/android/confvars.sh 注释掉MOZ_INSTALL_TRACKING（在第49行），否则编译失败。


## 参考链接
https://github.com/openthos/system-analysis/edit/master/firefox/build.mdhttps://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Build_Instructions/Simple_Firefox_for_Android_build
