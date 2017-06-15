---
title: Get Android app signatures
date: 2017-06-15 17:38:08
tags:
---

```
$ APK=app-release-signed.apk &&  keytool -list -printcert -jarfile $APK  | grep "MD5" | sed 's/://g' | tr '[:upper:]' '[:lower:]'
   md5  a4c9c04cbc91137b121e7cf3aac2xxxx
```

土著方法:

获取应用签名需要先在手机上面安装好应用的 release 签名版 app，再安装一个微信提供的 [签名生成工具](https://res.wx.qq.com/open/zh_CN/htmledition/res/dev/download/sdk/Gen_Signature_Android221cbf.apk) , 这个工具安装在手机上后，输入app的应用包名，即可生成应用签名。关键是还得把签名 **抄** 下来！
