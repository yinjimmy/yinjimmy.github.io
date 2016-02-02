---
title: Google Errors
tags: google
date: 2016-01-22
---

1. GoogleMobileAds.framework

    ```
    Undefined symbols for architecture i386:
      "GADAdSizeFromCGSize(CGSize)", referenced from:
    ```

    change
    ```
    GADAdSize GADAdSizeFromCGSize(CGSize size);
    GADAdSize GADAdSizeFullWidthPortraitWithHeight(CGFloat height);
    GADAdSize GADAdSizeFullWidthLandscapeWithHeight(CGFloat height);
    ```
    to
    ```
    GAD_EXTERN GADAdSize GADAdSizeFromCGSize(CGSize size);
    GAD_EXTERN GADAdSize GADAdSizeFullWidthPortraitWithHeight(CGFloat height);
    GAD_EXTERN GADAdSize GADAdSizeFullWidthLandscapeWithHeight(CGFloat height);
    ```

    + https://groups.google.com/forum/#!topic/google-admob-ads-sdk/p06NlN8Dn7s
    + http://stackoverflow.com/questions/21304412/admob-integration-error-in-ios
    
2. android 缓存 ad 时遇到内部错误

    可能是因为 `package="org.cocos2dx.cpp"` 的原因，更换一个 id 就能迅速工作了。
    
3. android 缓存不下广告

    需要注意 logcat 的输出：
    ```
    Use AdRequest.Builder.addTestDevice("FE20924C46522E2E204587EB339897C6") to get test ads on this device.
    ```
