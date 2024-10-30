
# 前言

本节教程我们要来认识一下 uni-app 的项目结构和体验一下uni-app 强大的跨端能力

# 新建 uni-app 项目

首先打开网址 https://www.dcloud.io/hbuilderx.html ，我们要下载 HBuilderX 编辑器 

下载安装好之后，打开 HBuilderX 选择菜单 File > New > Project
![[attachments/Pasted image 20240930061947.png]]

由于 `ez背单词APP` 是基于 `uni-app`，`vue2` 版本开发的，所以在本课程中我们也使用 `vue2` 来进行教学。在新建项目的窗口中，我们先在右下角找到 Vue version 切换到 2，

![[attachments/Pasted image 20240929044709.png]]

 在 Project Name 一栏，输入这个项目的名称，然后给这个项目选择一个存放的目录，点击 Create

![[attachments/Pasted image 20240929045133.png]]

我们就能在 `HBuilderX` 的左侧边栏看到一个有如图所示结构的新项目了

# uni-app 的项目结构

下面我们来逐一介绍一下这些文件的作用

## pages 文件夹

任何一个 APP 都是由一个或者多个页面组合成的，uni-app 也一样，这个 pages文件夹就是用于存放 `uni-app` 中所有的页面文件的，每个页面都会对应一个 `.vue` 的文件，在这个文件中我们来实现一个页面具体长什么样子，有什么功能。这个目录下的页面文件和 `pages.json` 里的配置通常是一一对应的。

我们可以来试着新建一个页面，在 pages 文件夹上点击鼠标右键，选择 New Page
![[attachments/Pasted image 20240930110550.png]]

![[attachments/Pasted image 20240930110512.png]]

## pages.json

 这是一个用于配置 APP 页面的文件，由于刚刚我们新建了一个名为 test 的页面，所以在这个文件里就出现了两个页面的配置项，还有一些全局的页面样式，背景色配置等。
 
![[attachments/Pasted image 20240930113421.png]]

## static 文件夹

这个文件夹我们通常用来存放 `uni-app` 应用中使用到的所有静态资源文件，比如图片，音频，视频，数据库文件等等。

## App.vue

app.vue 在 `uni-app` 项目中是应用的根组件，通常我们在这个文件中做两件事情
1，在这里定义全局样式，那么在所有的页面和组件中都可以直接使用这些样式。
2，APP 的生命周期事件监听，例如 APP 的启动，进入后台，回到前台等等。

## index.html 和 main.js 文件

是 `uni-app` 应用的入口，也是核心启动文件，负责初始化 Vue 实例、引入全局组件和样式、配置插件等，确定应用的基础结构和功能，为后续的页面和组件提供支持。一般情况下，我们也不需要去修改它。

## manifest.json

用于配置这个 APP 的基本信息，比如的名称，图标，版本号，这个 APP 会用到哪些系统模块，比如照相机，蓝牙，定位等等。

## 其它

uni.scss 和 uni.primisify.adaptor.js 两个文件我们可以忽略，因为通常我们无需对这两个文件进行修改。

一般在开发过程中，我们还会新建一些自己使用的目录和文件，这个就后面用到的时候再讲。

# 运行 uni-app

`uni-app` 的跨端能力非常强悍，一套代码既可以发布为网页，又可以发布为 `iOS`，`Android` 的 APP，还可以发布成各种小程序。下面我们就让我们来看看，如何把刚刚创建的这个 `uni-app` 项目在完全不同的几个平台上运行起来

## 运行到浏览器

首先我们来试一下将 uni-app 运行到浏览器里，也就是测试它发布为网页会是什么样子。

在 `HBuilderX` 的菜单栏里，选中 Run，选择 Browser，选择 Chrome

![[attachments/Pasted image 20240930054757.png]]

![[attachments/Pasted image 20240929133417.png]]

也可以选择 Built In Browser，就会打开一个侧边栏，在侧边栏里显示网页的运行效果
![[attachments/Pasted image 20240929133525.png]]


## 运行到 Android 模拟器

接着我们学习下如何把 uni-app 项目运行到 Android 模拟器上

首先我们需要在电脑上下载并安装 `Android Studio`

下载地址 https://developer.android.com/

![[attachments/Pasted image 20240929053039.png]]

安装好后打开 Android Studio，点击 More Actions ，选择 Virtual Device Manager

![[attachments/Pasted image 20240929055038.png]]

然后点 next，选择 `API35` ，等待下载完成

![[attachments/Pasted image 20240929055642.png]]

![[attachments/Pasted image 20240929055710.png]]

点击右侧的三角形图标启动模拟器，当模拟器启动后，我们在 `HBuilderX` 中选择 Android Run

![[attachments/Pasted image 20240929060946.png]]

![[attachments/Pasted image 20240929061109.png]]

再点击 run，我们就可以在 Android模拟器中将我们 `uni-app` 的项目运行起来了，在项目里做的修改，都会自动同步到模拟器中，非常方便。

![[attachments/Pasted image 20240929090359.png]]


## 运行到 iOS 模拟器 （仅 mac 可用）

注意：必须有 MacOS 系统，苹果电脑或者[黑苹果](https://osx.cx/)都可以

接下来我们来把 uni-app 项目运行到 iOS 模拟器上，要运行 iOS 模拟器必须使用Mac 系统

首先打开 AppStore， 下载 `Xcode` 

![[attachments/Pasted image 20240930142107.png]]

下载好之后打开 Xcode，安装 iOS 模拟器

![[attachments/Pasted image 20240929090052.png]]

完事后再从 `HBuilderX` 的菜单中选择 iOS-Simulator Run 就能够看到模拟器的选项了

![[attachments/Pasted image 20240929090307.png]]

![[attachments/Pasted image 20240929090232.png]]

点击 Run 按钮，我们就可以在 iOS模拟器中将我们的 uni-app 项目运行起来了
![[attachments/Pasted image 20240929090325.png]]


## 运行到 Android 和 iOS 真机

运行到真机和模拟器类似，需要我们把设备用 USB 线连接至电脑，即可在选择iOS Run 和 Android Run 的这个界面检测出来

![[attachments/Pasted image 20240929103243.png]]

别忘了在你的 iOS 设备或 Android 设备启用开发者模式，并且在 iOS 设备上运行必须使用苹果证书来进行签名。

	添加大帅老师微信（itdashuai）进入答疑群获取源码和用于测试的苹果开发者证书

![[attachments/Pasted image 20240930060837.png]]

依次填入以下信息后，点击 Start Signing 按钮运行
Bundle ID：com.ezshine.ezbdctest
Certificate private key password: 11111111
Certificate profile file: 选择 wildcard_profile.mobileprovision
Private key certificate: 选择 itdashuai.p12


## 运行到微信小程序

接下来我们再来尝试一下运行到微信小程序

首先我们要下载微信开发者工具 

https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html

然后在 HBuilderX 的菜单栏中点击 Wechat devtools 

![[attachments/Pasted image 20240929192045.png]]

等待编译完成，我们启动微信开发者工具

![[attachments/Pasted image 20240929192242.png]]

点击 + 号，暂时我们先用体验账号来试用微信开发者工具

![[attachments/Pasted image 20240929192624.png]]

点击文件夹图标，选择 uni-app 项目下的 unpackage > dist > dev > mp-weixin 目录

![[attachments/Pasted image 20240929192809.png]]

这样，我们的 uni-app 的项目就已经运行到微信小程序开发工具中了

![[attachments/Pasted image 20240929193036.png]]

	运行其它平台小程序步骤大致相同，都需要注册指定平台的小程序账号以及下载指定平台的开发者工具


# 将 ez背单词 项目运行起来

	添加大帅老师微信（itdashuai）进入答疑群获取源码和用于测试的苹果开发者证书

打开 ez背单词 项目的源码仓库，[https://github.com/ezshine/ezBDC](https://github.com/ezshine/ezBDC)

![[attachments/Pasted image 20240930060442.png]]

点击 Download ZIP 或者使用命令行 git clone 到本地后，从 HBuilderX 的菜单中选择 File > Import > Import From Local Directory

![[attachments/Pasted image 20240930060554.png]]

导入成功后，本小节前面学习到的运行方法，你都可以试试看了。