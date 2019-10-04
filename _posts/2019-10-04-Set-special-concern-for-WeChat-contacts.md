---
title: 给安卓版微信的某人设置特别关心
tags:
    - Tasker
    - 女朋友
---

众所周知，QQ 有一个功能是可以给好友设置特别关心，设置特别关心好友后，特别关心好友发来的信息提示音将是个性化提示音。但我所在的交际圈几乎没有人使用 QQ ，都是用微信的居多。

一直都有想给微信加上特别关心功能，连原理我都想好了，但就是没动力去做。终于，在9月26日发生的一件事让我有了动力：我有女朋友了 :kissing_heart:

<!--more-->

## 1. 原理

其实原理并不复杂。

首先，微信的提醒要开着，但是把提醒的声音和振动给关掉，目的就是让微信的消息静默地推到系统通知栏。然后使用第三方的软件读取通知栏的信息，当发现是微信的消息而且联系人是指定的联系人时，就发出提醒（提示音或振动）。是不是很简单！（没错，我就是来水文章的）

## 2. 实现方法

### 2.1 取消微信提醒

首先要取消掉微信的提示音和振动，我的小米手机可能有点不一样，大致就是：微信APP -> 我 -> 设置 -> 新消息提醒 -> 声音与振动。如下图所示。

![WeChat-settings](https://img4blog-1252202799.cos.ap-guangzhou.myqcloud.com/aomnisz/2019-10-04-Set-special-concern-for-WeChat-contacts/WeChat-settings-min.png)

### 2.2 Tasker APP

我们需要借助第三方的软件来实现提醒功能，这里选择的软件是 Tasker ([官网地址](https://tasker.joaoapps.com/))。这个软件的功能和 iOS 的捷径差不多，但比捷径要强大得多，毕竟是在毕竟开放的 Android 系统。这个软件在 [Google Play](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm) 上售价 2.99 刀，在国内[豌豆荚](https://www.wandoujia.com/apps/237829)等市场能免费下载，不过还是希望大家如果有能力支持正版的话就尽量支持啦。

在说明如何实现之前，我想先介绍一下这个软件。软件分为初学者模式和高级玩家模式，可以在首选项中调整，不过能看我这篇文章的一般都是初学者啦，我也是初学者，所以就在初学者模式下讲解了。打开软件可以看到有三个 Tab ，分别是“配置文件”，“任务”和“场景”，场景可以不用管，主要是前两个。在初学的时候，整个软件可以理解成 IF This Then That ，其中的 This 就是配置文件，That 就是任务。配置文件就是执行任务的条件，当符合条件时，就执行任务；任务就是具体的操作细节。一个配置文件至少要有一个任务，可以有多个任务，任务可以没有配置文件对应（有点绕，动手就容易懂了）。

![Tasker-APP](https://img4blog-1252202799.cos.ap-guangzhou.myqcloud.com/aomnisz/2019-10-04-Set-special-concern-for-WeChat-contacts/Tasker-APP-min.png)

那么回到实现来，我的需求可以翻译成，当符合“有来自女朋友的微信信息”这个条件时，就执行“发出提示音并振动”这个任务。因为配置文件必须有任务对应，所以我先建任务。

### 2.3 Tasker 中添加振动提醒

切换到“任务” Tab ，点击右下角的加号“+”，就会要求输入“新任务名称”，这里就填“女朋友专属微信提醒”吧。新的任务需要添加操作，依然是点右下角的加号，就会出现一个弹窗，里面有特别特别多的选项，但不必去找，在搜索栏中输入“振动”，就出来了。然后设置好振动时长（单位是毫秒），返回，一个操作就新建完成了。操作如下图所示。

![Tasker-Add-tasks](https://img4blog-1252202799.cos.ap-guangzhou.myqcloud.com/aomnisz/2019-10-04-Set-special-concern-for-WeChat-contacts/Tasker-Add-tasks-min.png)

### 2.4 Tasker 中添加声音提醒

如果希望声音提醒和振动一起来，可以在同一个任务中继续添加操作（也可以新建一个任务）。依然是点击右下角的加号按钮，同样是直接搜索“播放铃声”。然后类型选择“通知”（这里随便选都没问题好像），接着在声音那一栏，点击右边的放大镜符号，去找到自己想要给女朋友的专属铃声。我的不知道为什么选了没反应，就算了，当然也没有铃声了呜呜呜。操作如下图所示。

![Tasker-Add-ringtone-action](https://img4blog-1252202799.cos.ap-guangzhou.myqcloud.com/aomnisz/2019-10-04-Set-special-concern-for-WeChat-contacts/Tasker-Add-ringtone-action-min.png)

### 2.5 Tasker 中新增配置文件

任务创建好了之后，回到“配置文件”这个 Tab ，点击右下角的加号按钮新建配置文件。

首先需要选择类型，实现特别关心的原理是需要监控系统的通知栏，所类型以选择“事件” -> “界面” -> “通知”。接着指定监控哪个程序的通知，点击“所有者程序”右侧的 Icon，在里面找到微信，并返回。然后添加一些筛选条件，在标题中填写“女朋友”，使得只有女朋友发来的信息才会提醒，这里填的标题是微信提醒的标题，也就是备注名称。

这样只有女朋友发微信消息过来，才会触发这个配置文件了。再按返回退出配置文件的编辑，这时会要求选择一个任务，之前说了一个配置文件至少要有一个任务，如果没有的话，这个配置文件就会被自动删掉，这里选择之前创建的“女朋友专属微信提醒”任务就行。

![Tasker-Add-profiles](https://img4blog-1252202799.cos.ap-guangzhou.myqcloud.com/aomnisz/2019-10-04-Set-special-concern-for-WeChat-contacts/Tasker-Add-profiles-min.png)

至此，微信的特别关心提醒就完成啦。

## 3. 进阶：与小米手环联动

### 3.1 设置小米手环APP通知提醒

小米手环可以设置接受手机的通知，需要在 “小米运动” APP 中设置。打开小米运动APP，沿着路径找到添加需要通知的应用的地方：我的 -> 我的设备（选择小米手环） -> APP通知提醒 -> 管理应用。在管理应用中将 Tasker 的勾选上，这样当有来自 Tasker 的通知时，手环也会一起提醒。

![Mi-Band-settings](https://img4blog-1252202799.cos.ap-guangzhou.myqcloud.com/aomnisz/2019-10-04-Set-special-concern-for-WeChat-contacts/Mi-Band-settings-min.png)

### 3.2 让 Tasker 发出通知

接着，需要让 Tasker 发出一个通知，小米手环才能接受到。在任务中继续添加动作，搜索“通知”。然后标题填写“女朋友”，这个是通知标题，就抄一下微信的通知格式吧，下面的文字可以填 `%evtprm3`，这个是提醒中 Text 的部分，具体的细节可以看 [Tasker免插件提取Android通知正文](https://medium.com/@xtarin/tasker%e5%85%8d%e6%8f%92%e4%bb%b6%e6%8f%90%e5%8f%96android%e9%80%9a%e7%9f%a5%e6%ad%a3%e6%96%87-6b3d4051f650) 这篇文章。这样通知就完成了。

不过这样的通知需要去手动清理，为了让它自动消失，我加上了“等待”和“通知取消”这两个操作，在发出通知 10s 之后就将通知清掉。这样我既能在手环收到提醒，又不用手动清理通知，美滋滋。

![Tasker-Add-notify-action-min](https://img4blog-1252202799.cos.ap-guangzhou.myqcloud.com/aomnisz/2019-10-04-Set-special-concern-for-WeChat-contacts/Tasker-Add-notify-action-min.png)

最后，来一张效果图吧！

![Mi-Band-Notification](https://img4blog-1252202799.cos.ap-guangzhou.myqcloud.com/aomnisz/2019-10-04-Set-special-concern-for-WeChat-contacts/Mi-Band-Notification-min.png)

## 4. 后记

刚弄好的时候非常兴奋，但时间久了一点就发现，会对正常生活造成影响 qaq ... 例如，有时候年级群这些群里发的一些重要通知（申请奖学金什么的），又或者是，家庭群突然发红包，这些提醒都收不到了，就很亏。

而且，我家那小朋友说了，“不能让恋爱成为你生活的全部，还是要好好认真做其他该做的事”，于是我在国庆的时候调整了这个特别关心的提醒，只保留手环的提醒，而微信的声音和振动都恢复，这样就不会很影响日常生活啦。
