---
title: Firebase云消息传递实践
categories:
  - Firebase
tags:
  - Firbase
date: 2019-04-01 10:29:20
author: Ric
---

### cloud-messaging简介
cloud-messaging是google提供的跨平台消息传递解决方案,功能类似国内的**`极光推送`**

### cloud-messaging功能
1. 从我们的应用服务器发送通知消息(用于手机通知弹窗)或数据消息(app内处理的)
2. 从客户端(ios,android,web)将消息发送到我们的应用服务器

### cloud-messaging工作原理
1. 我们服务器和fcm通讯有两种协议,一种是http协议,一种是[xmpp协议](https://www.ibm.com/developerworks/cn/xml/x-xmppintro/index.html)
![工作原理](../images/cloud-messaging.png)
   2. 我们如何识别客户端呢,ios或android需要在谷歌的服务器Fcm(Firebase Cloud Messaging)注册一个令牌(token),然后把这个令牌传送到服务器,我们服务器就可以通过这个令牌给对应的客户端发送消息了,注意以下三种情况注册令牌会发生改变,改变后请把这个注册重新发送到我们服务器:
      1. 应用在新设备上恢复
      2. 用户卸载/重新安装应用
      3. 用户清除用户数据
   
### cloud-messaging使用注意
1. FCM 不保证传递顺序
2. 接到 FCM XMPP 服务器的速率限制为每个项目每分钟 400 次连接
3. 向单一设备`发送`最多 240 条消息/分钟和 5000 条消息/小时
4. 每个`项目`的`上行消息`限制为 15000 条/分钟
5. 将每台`设备`的`上行消息`限制为 1000 条/分钟
6. 主题订阅添加/移除率限制为每个项目 3000 QPS
7. 数据消息里面的键值只支持字符串

### cloud-messaging使用(php)
我们上行使用了XMPP协议,下行使用了http协议
为了简化开发我们使用了网上的[SDK](https://github.com/kreait/firebase-php/tree/dd34c653997473e3ef2c2d1eb648edd44cc6c2f0)和[SDK使用文档](https://firebase-php.readthedocs.io/en/stable/cloud-messaging.html#),这个SDK目前只支持http协议
