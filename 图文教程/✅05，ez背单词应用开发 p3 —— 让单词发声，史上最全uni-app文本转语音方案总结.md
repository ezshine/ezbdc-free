

# 前言

在 ez背单词 APP 中，将单词读出来，让用户锻炼听力，是很重要的一个特性。本节教程，我们就来学习在 uni-app 中让单词发出声音（也就是文本转语音）的各种方案，应该是史上最全的文本转语言方案的归纳总结了。

# 极简方案

最简单的方式，是直接调用有道词典的发音接口url

```
https://dict.youdao.com/dictvoice?audio=Hello&type=1
```

- type=1 英式
- type=2 美式

这个 API 也是很多小微型产品都会用到的一个发音 API，优点是：免费，支持英式和美式发音。

# 系统TTS

虽然有道词典的发音接口简单易用，但缺点是只有两种发音风格选择，并且还需要联网。而 ez背单词 是希望可以在完全无网的状态下也能正常使用的。

![[Pasted image 20241031034432.png]]

那怎么办呢？
其实现在主流的系统环境都自带了文本转语音的系统接口，我们只需要调用这些系统接口即可。

在 uni-app 中调用系统 API，在前面的教程中，我们已经初步尝试过了，还记得吗？就是用 plus. API来实现。而 plus. API 还有非常强大的 plus.ios 和 plus.android 两套 api，可以让我们使用 javascript 轻松调用系统级文本转语音功能。

	uni-app 的 plus.ios 和 plus.android 虽然强大，但调试并不方便，如果不是对双端原生开发很熟悉的老手，用起来还是比较费劲的。

## Android

```javascript
function andriodTTS(text,language="en_US") {
	console.log("andriodTTS",language)
	
	const TextToSpeech = plus.android.importClass("android.speech.tts.TextToSpeech");
	const Context = plus.android.importClass("android.content.Context");
	const activity = plus.android.runtimeMainActivity();
	
	function speak(text,language="en_US"){
		const Locale = plus.android.importClass("java.util.Locale");
		const LanguageS = language.split("-");
		let locale = new Locale(LanguageS[0], LanguageS[1]);
		this.tts.setLanguage(locale);
		this.tts.setSpeechRate(0.6);
		
		const HashMap = plus.android.importClass('java.util.HashMap');
		const params = new HashMap();
		this.tts.speak(text,TextToSpeech.QUEUE_FLUSH,params);
		console.log("speak done");
	}
	
	const OnInitListener = plus.android.implements('android.speech.tts.TextToSpeech$OnInitListener', {
		onInit: (status)=> {
			if (status === TextToSpeech.SUCCESS) {
				// TTS初始化成功
				console.log('TTS初始化成功');
				speak(text,language);
			} else {
				console.error('TTS初始化失败');
			}
		}
	});
	
	// 使用正确的监听器创建TTS实例
	if(!this.tts)this.tts = new TextToSpeech(activity, OnInitListener);
	else speak(text,language);
 }
```

## iOS

```javascript
function iosTTS(text,language="en-US") {
	var AVSpeechSynthesizer = plus.ios.importClass('AVSpeechSynthesizer');
  	var AVSpeechUtterance = plus.ios.importClass('AVSpeechUtterance');
  	var AVSpeechSynthesisVoice = plus.ios.import('AVSpeechSynthesisVoice');
	
  	var speech = new AVSpeechSynthesizer();
	var voice = AVSpeechSynthesisVoice.voiceWithLanguage(language);
	var utterance = AVSpeechUtterance.speechUtteranceWithString(text);
	
  	utterance.plusSetAttribute("voice",voice);
	utterance.plusSetAttribute("volume",1.5);
	utterance.plusSetAttribute("rate",0.5);
  	speech.speakUtterance(utterance);
  	
  	plus.ios.deleteObject(utterance);
  	plus.ios.deleteObject(voice);
}
```

在 uni-app 中，我们需要通过判断当前运行环境是 iOS 还是 android 来决定要调用哪个方法

```javascript
function textToSpeech(text,language="en_US"){
	const sysInfo = uni.getSystemInfoSync();
	
	if(sysInfo.platform==="android"){
		andriodTTS(text,language);
	}else if(sysInfo.platform==="ios"){
		iosTTS(text,language);
	}
}
```
## 浏览器

ez背单词除了可以运行在 iPhone，Apple Watch，Android 上，还有一个浏览器插件的版本。而现代浏览器已经给我们提供好了文本转语音的 API 可以直接调用。它就是 Web Speech API

![[Pasted image 20241031035824.png]]

从上方的截图我们也可以看到，桌面端的浏览器已经全部支持这个 API 了，使用起来也是非常非常简单

```javascript
var utterance = new SpeechSynthesisUtterance("text");

utterance.volume = 1.0;
utterance.rate = 0.5;

window.speechSynthesis.speak(utterance);
```

语言选择演示 https://codepen.io/ezshine/pen/rNXvmag

使用系统级文本转语音方案的最大好处就是离线了

# 自建 TTS 服务

可是如果你想把语音保存成mp3文件，那就只好用最后一个方案了，也是最强大，自由度最高的方案。但这个方案需要你有一台云服务器。不过好在对云服务器的要求不高，买一个最低配的就够用了，大概一年 23 刀。购买地址： https://my.racknerd.com/aff.php?aff=10399

	有一台自己的海外服务器可以做很多用途，比如你还可以用它自建一个通道来上网，这就不啰嗦了。

在拥有了自己的服务器后，我们就可以在服务器上安装这个 TTS 服务了，这是基于微软Edge TTS api 封装。 docker地址  https://hub.docker.com/r/mzzsfy/tts

![[Pasted image 20241031044114.png]]

# 结语

在本小节中，我们学习了将英文单词转为语音播放的各种实现方案。大家可以根据不同的产品形态，不同的使用场景来选择不同的方案。

---
在  ez背单词 APP中：

- ez背单词 Apple Watch 版使用的是iOS系统自带的 TTS 方案
- ez背单词 iOS/Android APP版实现了所有的方案供用户选择
- ez背单词浏览器插件版使用的有道 API 和浏览器自带TTS 方案