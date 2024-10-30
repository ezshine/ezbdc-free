
# 前言

当我们开发完成一个 APP，我们需要将它提交到应用市场给全世界的用户使用。我们可以加广告赚钱，也可以设置购买付费或者是订阅付费。

要发布到苹果应用商店必须先成为苹果开发者，虽然每年需要支付 $99USD（688 元）的费用，但全世界最愿意付费的用户群体大部分都在使用苹果生态的产品，iPhone，iPad，AppleWatch，AppleVisionPro, Macbook等等。而苹果开发者证书可以用于苹果全系列产品上的软件开发，这么看还是非常超值的。

	此教程必须通过 Mac 系统来完成，没有苹果电脑的同学使用黑苹果也是可以的。
# 准备工作

### 注册苹果开发者账号

首先我们要前往 https://developer.apple.com/ 苹果开发者门户，登录自己的 AppleID。然后选择 Apple Developer Program进行注册，在本教程中我们仅以个人身份注册作为示例
![[Pasted image 20241022090115.png]]

点击注册后，会要求我们下载 Apple Developer APP，在 APP 中完成注册的后续步骤和支付。
![[Pasted image 20241022091203.png]]

## 新建开发证书

![[Pasted image 20241022093745.png]]
付款完成后，再点击 https://developer.apple.com/ 里的Account（账户）就可以看到这个界面了，首先我们选择证书。
![[Pasted image 20241022093906.png]]
点击 + 号，选择 iOS Distribution，然后点击右上角的 Continue
![[Pasted image 20241022093941.png]]
![[Pasted image 20241022094030.png]]
这里会要求我们上传一个证书签名的请求文件，在Mac 电脑中的 spotlight 搜索中输入 Keychain Access
![[Pasted image 20241022094344.png]]

打开后从菜单中选择 Request a Certificate From a Certificate Authority（从证书认证机构请求证书）
![[Pasted image 20241022094438.png]]
然后输入你的邮箱地址，证书显示的名称，勾选 Saved to disk (保存至本地)
![[Pasted image 20241022094648.png]]
完成后会生成一个证书请求文件，在开发者证书创建后台选择此文件上传即可生成你的证书
![[Pasted image 20241022094817.png]]
点击 Download 下载后双击，能从 Keychain Access 里看到这个证书就成功了
![[Pasted image 20241022094932.png]]

## 新建 APP 签名文件

首先我们点击做侧边栏的 Identifiers 菜单，然后点击 + 号来新建一个 App IDs
![[Pasted image 20241022100037.png]]
![[Pasted image 20241022100127.png]]
![[Pasted image 20241022100145.png]]

Description：这个 AppId 的介绍，防止你看 AppId 想不起来这个是啥
BundleId：App 的包 id，也是 APP 的唯一 ID，很重要。一般格式是 com. 开发者名称字母 . APP名称字母
![[Pasted image 20241022100834.png]]
	下方的 Capabilities 中勾选使用到的功能（非必须）

![[Pasted image 20241022101307.png]]
然后，我们点击左侧边栏中的 Profiles 菜单，新建 Profile 文件
![[Pasted image 20241022101421.png]]
选择 App Store Connect
	如果你要进行真机调试，则选择iOS App Development
![[Pasted image 20241022101508.png]]
然后选择我们刚才创建的 App ID 即可生成
![[Pasted image 20241022101611.png]]
生成好之后下载，双击完成本步骤

# 打包

## 云打包

云打包是 uni-app 公司提供的一个免费服务，可以让开发者打包更轻松方便，甚至在没有 Mac 电脑的情况下，也可以打 iOS 的包。

打开我们的项目文件，在 HBuilderX 的菜单中选择 App-Android/iOS - Cloud Packing（云打包）

![[Pasted image 20241022152925.png]]

然后我们就能看到云打包的配置界面，本篇教程仅介绍 iOS 相关的打包配置
![[Pasted image 20241022160708.png]]

可以勾选你的 APP 是否支持在 iPad 上运行 （ Support iPad ）

下面我们先打开 Keychain Access，搜索 iPhone
选择咱们之前创建的证书文件，然后单击鼠标右键选择 Export 导出 .p12 文件
![[Pasted image 20241022171702.png]]
![[Pasted image 20241022171746.png]]
点击保存会让我们输入密码，记住这个密码，然后回到云打包的配置界面

![[Pasted image 20241022172039.png]]

注意，Bundle Identifier 需要填写成之前在苹果开发者后台创建的 App ID

	com.开发者名称字母.APP名称字母

然后就可以点击 submit 提交打包了，需要排队等待打包，完成后会在提示窗口显示一个 .ipa 文件的地址。这个 ipa 文件就是我们打包完成的 app 了。

快的话 1 分钟，高峰期慢的话得一些时间，耐心等待就好。如果不想排队，那就要继续学习下面的离线打包了。

## 离线打包

### 申请离线打包 Key

打开 https://dev.dcloud.net.cn/ 等你的 dcloud 开发者账号，在左侧边栏中找到应用管理-我的应用
![[Pasted image 20241023000154.png]]
点击应用名称，在 tab 标签中切换到各平台信息
![[Pasted image 20241023000236.png]]
点击新增，选择 iOS App，正式版，输入这个应用的 BundleId，其它的空可以不填

	com.开发者名称字母.APP名称字母

![[Pasted image 20241023000312.png]]

### 生成离线打包资源

首先我们从 HBuilderX 的菜单中选择 App-Android/iOS - Local Packaging (离线打包)，Generate App Resources（生成离线打包资源）
![[Pasted image 20241022225251.png]]
等待编译完成后，在 unpackage/resources 目录下会出现一个 __UNI__XXXXXXX 的文件夹
![[Pasted image 20241022225458.png]]

暂时我们先把这个文件夹放一边，然后我们去下载离线打包最新的 SDK https://nativesupport.dcloud.net.cn/AppDocs/download/ios.html

### 修改离线打包 SDK

离线打包 SDK 下载完成解压缩后，我们能得到这样一个目录，使用 Xcode 打开 HBuilder-Hello 下的 .xcodeproj 文件
![[Pasted image 20241022220039.png]]
将我们刚才生成好的离线打包资源文件 __UNI__XXXXXXX 移动到 HBuilder-Hello/HBuilder-Hello/Pandora/apps 目录下
![[Pasted image 20241022225927.png]]
	原 apps 目录下有一个类似的文件夹，我们可以删掉它

然后在 Xcode 的侧边栏中找到 Supporting Files 下的 control.xml , 将其中的 appid 更改为咱们 uni-app 项目的 appid，也就是那个 __UNI__XXXXXXX ，后面的 appver 更改为 uni-app 的版本号
![[Pasted image 20241022235701.png]]

点击左侧边栏中的 HBuilder，点击 Targets 中的 HBuilder，切换至 info
![[Pasted image 20241023000800.png]]
找到 dcloud_appkey 这个字段，将我们生成的 离线打包Key 填写到后面的赋值框中

然后切换至 Signing & Capabilities 标签，将 Bundle Identifier 修改为这个应用的 BundleId

	com.开发者名称字母.APP名称字母

![[Pasted image 20241023001257.png]]

将下方的 Provisioning Profile 选择为我们前面生成的 .mobileprovision 文件对应的内容，如果前面的步骤都正确的话，下面的 Signing Certificate 会自动切换为正确的证书文件。

然后我们切换回 General 标签，修改 Display Name , 这个字段是 App 在桌面图标下显示的名字
![[Pasted image 20241023001139.png]]

此时我们打包的全部工作就几乎都完成了，当然我们还可以修改 App 图标，App 启动图，多语言设置等等，篇幅过长，此节教程中我们就先跳过。

然后在 Product 菜单中选择 Archive。就会开始进行打包了
![[Pasted image 20241023001858.png]]
打包成功后，会弹出下面的窗口
![[Pasted image 20241023002013.png]]

此时我们再点击 Distribute App ，选择 App Store Connect 可以直接上传。
![[Pasted image 20241023002228.png]]

为了和前面云打包的流程同步，我们选择 Custom 然后再选择 App Store Connect 
![[Pasted image 20241023002408.png]]

Export （导出）
![[Pasted image 20241023002430.png]]

点 Next ，选择证书和签名文件
![[Pasted image 20241023002720.png]]
在 Next 之后点击 Export
![[Pasted image 20241023002813.png]]

即可在目标文件夹找到 .ipa 文件
![[Pasted image 20241023002607.png]]

# 上传打包好的 ipa

在 Mac App Store 中下载 Transporter https://apps.apple.com/es/app/transporter/id1450874784
![[Pasted image 20241023003524.png]]

打开 Transporter App 登录你的苹果开发者账号
![[Pasted image 20241023003555.png]]
登录完成后，我们在 Transporter 中选择 ADD APP 选择我们生成好的 .ipa 文件
![[Pasted image 20241023003708.png]]
![[Pasted image 20241023003741.png]]

就会开始进行校验
![[Pasted image 20241023003753.png]]

校验成功后就可以点击 DELIVER 上传到 AppStore 的发布后台了
![[Pasted image 20241023004048.png]]

# 提审

## 填写商店信息和定价

进入 AppStore 的发布后台 https://appstoreconnect.apple.com/ 选择 App
![[Pasted image 20241023004447.png]]

如果前面上传都正常完成的话，我们就能看到一个准备提交的 APP
![[Pasted image 20241023004624.png]]
点击这个 APP，将 AppStore 要求的信息填写上内容，例如

- App 的预览和截屏
- 推广文本
- App 的描述
- App 的关键词
- 技术支持网址
- 营销网址
- 版本号
- 版权信息

![[Pasted image 20241023004917.png]]
![[Pasted image 20241023004953.png]]

这些信息我就不一一讲解了，根据提示填写即可，在左侧边栏的价格与销售范围中可以给你的 APP 定价以及设置在哪些国家/地区进行销售

全部填写完成后点击右上角的 添加以供审核，等待审核人员的反馈。如果一切顺利自然是可以直接发布，如果显示审核被驳回，就要根据反馈信息将问题一一解决后重新提交审核。

## 被驳回的处理

被驳回是非常正常的，即便是一个老手，也很有可能因为遗漏一些信息而遇到驳回。但是，苹果的审核会非常详细的告诉你问题出在哪里，你应该如何修复或解决。

# 发布

能完成到这一步都太棒了，全世界的iPhone用户都可以在 AppStore 下载到你开发的 APP 了，根据用户的反馈不断迭代你的产品吧，预祝大家未来的产品都能获得成功。
