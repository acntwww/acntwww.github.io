![](http://p09ypv7br.bkt.clouddn.com/blog/android/adb.png)

![@&|center](https://www.jpmorganchase.com/corporate/Corporate-Responsibility/images/cr-2016-ampersand.png)
在开发的过程中, 会经常用到`adb`这个工具, 在使用的过程中, 比如下面的的命令用来启动一个`Activity`
```shell
adb shell am start -W -a android.intent.action.VIEW -d "hubble://hubble.MA-B5BA-D69CCCCF880F"
```
如果只是想给`URI`增加参数的话, 直接的`a=1&b=23`是不行的
> 因为这是两个shell, 一个是电脑的, 另一个是Android手机的. 而`&`是一个特殊的字符, 会被电脑的shell给处理掉.

解决的办法
---
#### 1. 使用`\&`转义
```shell
adb shell am start -W  -n com.netease.hubbledata/.module.splash.page.SplashActivity -a android.intent.action.VIEW -d "hubble://hubble.MA-B5BA-D69CCCCF880F?token=abck4\&type=qax" 
```

#### 2. 使用`Android`的shell, 避免被吃掉
```shell
> adb shell
> am start -W  -n com.netease.hubbledata/.module.splash.page.SplashActivity -a android.intent.action.VIEW -d "hubble://hubble.MA-B5BA-D69CCCCF880F?token=abck4&type=qax" 
```

### 其他的要转义的字符
`( ) < > | ; & * \ ~ " '`

总结
----
之前也遇到一个类似的问题, 好像给符号英语化之后, 加上一些关键词就能够找到想要的答案了.
需要给常见的标点符号的英文给记忆一些, 能省不少的时间!


参考文档
---
[adb shell input text can not input & text](https://issuetracker.google.com/issues/36982137)
[How to input ampersand with adb shell input?](https://stackoverflow.com/a/28468561/1294349)
[adb shell input text does not take & ampersand character](https://stackoverflow.com/a/31371987/1294349)

[Specification for intent arguments](https://developer.android.com/studio/command-line/adb#IntentSpec)
> The reason you need to doubly-escape the string is that there are two shells: your shell, and the adb shell