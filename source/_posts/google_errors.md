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

- https://groups.google.com/forum/#!topic/google-admob-ads-sdk/p06NlN8Dn7s
- http://stackoverflow.com/questions/21304412/admob-integration-error-in-ios
