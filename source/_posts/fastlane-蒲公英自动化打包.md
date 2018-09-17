---
title: fastlane+蒲公英自动化打包
date: 2018-09-14 14:05:01
tags: fastlane自动化打包
categories: 自动化打包
---

> 人类似乎有这样的倾向，建立一项规则叫别人遵守，同时又极力使自己成为例外，不受它的约束。&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;——卢梭《忏悔录》


---

本文参考[自动化打包之fastlane](https://blog.csdn.net/kuangdacaikuang/article/details/80443515) 作者写的特别详细，之前自己装这个，执行到安装蒲公英插件，总是报下面的错误

```
An error occurred while installing unf_ext (0.0.7.5), and Bundler
cannot continue.
Make sure that `gem install unf_ext -v '0.0.7.5'` succeeds before bundling.
```

后来看到这篇博客，一次性成功，中间没遇到什么很难解决的问题，直到最后打包成功。很丝滑很流畅。再次感谢。

<!--more-->

## 1、安装rvm

RVM 是一个命令行工具,可以提供一个便捷的多版本 Ruby 环境的管理和切换。

执行命令`rvm -v`，查看是否已经安装了rvm

```
$ rvm -v
-bash: rvm: command not found
```

执行命令安装rvm：

```
$ curl -L https://get.rvm.io | bash -s stable
$ source ~/.bashrc
$ source ~/.bash_profile
```

再次执行命令`rvm -v`

```
$ rvm -v
rvm 1.29.4 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io]
```

mac自带ruby，也可以自己安装，使用rvm查看ruby的版本

执行命令：

```
$ rvm list known

# MRI Rubies
[ruby-]1.8.6[-p420]
[ruby-]1.8.7[-head] # security released on head
[ruby-]1.9.1[-p431]
[ruby-]1.9.2[-p330]
[ruby-]1.9.3[-p551]
[ruby-]2.0.0[-p648]
[ruby-]2.1[.10]
[ruby-]2.2[.10]
[ruby-]2.3[.7]
[ruby-]2.4[.4]
[ruby-]2.5[.1]
[ruby-]2.6[.0-preview2]
ruby-head

# for forks use: rvm install ruby-head-<name> --url https://github.com/github/ruby.git --branch 2.2
.
.省略好多行
.
```

## 2、brew

```
$ brew update          # 更新 Homebrew 的信息
$ brew outdated        # 看一下哪些软件可以升级
$ brew upgrade <xxx>   # 如果不是所有的都要升级，那就这样升级指定的

$ brew upgrade; 		#全部升级
$ brew cleanup    # 如果都要升级，直接升级完然后清理干净
```

执行命令：

```
$ brew update

fatal: unable to access 'https://github.com/Homebrew/homebrew-cask/': LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 54
Error: Fetching /usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask failed!
Updated 1 tap (homebrew/core).
==> New Formulae
picat
==> Updated Formulae
bat                        glm                        rust
erlang                     libiscsi                   stlink
freexl                     opensc                     swift-protobuf
git-cola                   php@7.1                    webpack
git-credential-manager     root
```

报错，别慌，可能是网络问题，多执行几遍就好了，我执行了两次输出以下结果

```
Updated 1 tap (homebrew/cask).
No changes to formulae.
```

执行命令：

```
$ brew outdated

ack (2.14) < 2.24
automake (1.15, 1.15.1) < 1.16.1_1
go (1.9.2) < 1.11
libgpg-error (1.24, 1.27) < 1.32
libyaml (0.1.6_1, 0.1.7) < 0.2.1
openssl (1.0.2h_1, 1.0.2n) < 1.0.2p
readline (6.3.8, 7.0.3_1) < 7.0.5
```

执行命令：

```
$ brew upgrade

Error: The following directories are not writable by your user:
/usr/local/share/man/man3
/usr/local/share/man/man5
/usr/local/share/man/man7

You should change the ownership of these directories to your user.
  sudo chown -R $(whoami) /usr/local/share/man/man3 /usr/local/share/man/man5 /usr/local/share/man/man7
```

报错了，按照提示，执行命令：

```
$ sudo chown -R $(whoami) /usr/local/share/man/man3 /usr/local/share/man/man5 /usr/local/share/man/man7
```

## 3、bundler

执行命令：

```
$ gem install bundler

Fetching: bundler-1.16.4.gem (100%)
ERROR:  While executing gem ... (Errno::EACCES)
    Permission denied @ rb_sysopen - /Library/Ruby/Gems/2.3.0/cache/bundler-1.16.4.gem
```

没有权限，执行命令：

```
$ sudo gem install bundler
```
输入密码，完成安装。

## 4、fastlane

执行命令：

```
$ xcode-select --install
```

会弹出一个弹窗，安装即可。

执行命令：

```
$ sudo gem install fastlane --verbose
```

## 5、match

执行命令：

```
$ sudo gem install match
```

安装成功以后会提示使用fastlane match代替match

## 6、deliver

上传屏幕截图,元数据,和APP到AppStore

执行命令：

```
$ sudo gem install deliver
```

安装成功以后会提示使用fastlane deliver代替deliver

## 7、初始化fastlane

**a：**打开项目，导航栏->product->scheme->manage schemes...选择你的项目，勾选shared

**b：**在终端中，cd到你的项目目录，执行命令：

```
$ fastlane init

[11:37:59]: Created new folder './fastlane'.
[11:37:59]: Detected an iOS/macOS project in the current directory: 'wasaiSports.xcworkspace'
[11:37:59]: -----------------------------
[11:37:59]: --- Welcome to fastlane 🚀 ---
[11:37:59]: -----------------------------
[11:37:59]: fastlane can help you with all kinds of automation for your mobile app
[11:37:59]: We recommend automating one task first, and then gradually automating more over time
[11:37:59]: What would you like to use fastlane for?
1. 📸  Automate screenshots
2. 👩‍✈️  Automate beta distribution to TestFlight
3. 🚀  Automate App Store distribution
4. 🛠  Manual setup - manually setup your project to automate your tasks
?  3
```

有4个选项，先选3，上传到App Store

```
.
.省略很多
.
[11:38:17]: - Manage your App Store Connect app metadata and screenshots
[11:38:17]: 
[11:38:17]: Your Apple ID credentials will only be stored in your Keychain, on your local machine
[11:38:17]: For more information, check out
[11:38:17]: 	https://github.com/fastlane/fastlane/tree/master/credentials_manager
[11:38:17]: 
[11:38:17]: Please enter your Apple ID developer credentials
[11:38:17]: Apple ID Username:
```

输入你的开发者账号邮箱，如果有多个team的话，选择自己的team序号，然后

```
[11:39:11]: This way, you'll be able to edit your app's metadata in local `.txt` files.
[11:39:11]: After editing the local `.txt` files, just run fastlane and all changes will be pushed up.
[11:39:11]: If you don't want to use this feature, you can still use fastlane to upload and distribute new builds to the App Store
[11:39:11]: Would you like fastlane to manage your app's metadata? (y/n)
y
```


是否允许fastlane管理app的数据信息，输入y，我分别在两台电脑上走到这个过程遇到了两种不同的情况：

◆ 第一次直接成功了，但是在项目文件目录下没有生成`Gemfile.lock`文件，其他都有，然后我就斗胆执行了命令`$ bundle update`，执行成功后就有了`Gemfile.lock`这个文件。

◆ 第二次卡在了`$ bundle update`这一行，这个不是我手动输入的命令，而是上面输入y之后输出的，然后我又斗胆终止了，按`ctrl + c`还终止不了，重新打开一个窗口，cd到项目目录，执行命令`$ bundle update`，很快的就成功了。

以上是我遇到的两种情况，到此为止，`fastlane init`就成功了，此时你的项目目录下会出现`fastlane文件夹`、`Gemfiel`、`Gemfile.lock`这三项。

## 8、插件


☞ 安装蒲公英pgyer插件 ☜，当然有很多插件可以选择，本篇以蒲公英为例，执行命令：

```
$ bundle exec fastlane add_plugin pgyer
```

☞ 安装版本控制versioning插件 ☜，执行命令：

```
$ bundle exec fastlane add_plugin versioning
```

还有很多插件，可以参考[fastlane官方文档可以找到的插件](https://docs.fastlane.tools/plugins/available-plugins/)

## 9、打包

在Fastfile中增加上传到蒲公英的lane

```
lane :beta do
  	build_app(export_method: "ad-hoc")
  	pgyer(api_key: "******", user_key: "******")
  end
```

其中api_key和user_key的获取方法：登录你的蒲公英账号（提前先上传一个版本），在应用概述里面找到这两个值。

执行命令：

```
$ fastlane beta
```

成功后，如果你的蒲公英绑定了微信或手机，就会收到相应的信息通知。


---

*到此结束，一把辛酸泪，好记性不如烂笔头。*

