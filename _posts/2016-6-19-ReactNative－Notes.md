---
layout:  post
title:  React Native 注意事项
tags: React RN
description: React Native的常见事项
comments: true
---

### 升级注意
- 依赖。用npm3 ，因为是包是平行关系，所以可以解决重复依赖问题。
- 可以通过npm info 查看最新的版本。
- 升级后，请运行, 可以对一些文件进行升级。
```
react-native upgrade
```
- 有些第三方组件需要执行， rnpm link

### 开发注意
- andrioid 。注意设置ip。

### 发布注意
- android 的发布 [参见传送门](http://reactnative.cn/docs/0.27/signed-apk-android.html#content)
	- 签名。使用工具创建私有密钥
	
	```
		$ keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
		```
	- build realse签名版本。 
	
		```
		cd android && ./gradlew installRelease
		```	
