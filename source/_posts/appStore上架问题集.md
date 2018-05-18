---
title: appStore上架问题集
date: 2018-05-04 17:44:47
tags: app Store上架
categories: iOS
---

app每次上架过程就是一部血泪史，各种被拒，没看审核文档，一步一步踩着坑过来，其中还是有很多值得注意的地方的，将各种问题及解决办法收集起来汇成一部宝典，伴随自己成长。

<!-- more --->

---

> Guideline 4.2.1 - Design - Minimum Functionality

```
Your app uses the HealthKit APIs but does not indicate integration with the Health app in your app description and clearly identify the HealthKit and CareKit functionality in your app's user interface.

Next Steps

To resolve this issue, please revise your app description to specify that your app integrates with the Health app.

To resolve this issue, please clearly identify the HealthKit functionality in app's user interface to avoid confusion.
```
> Guideline 5.1.1 - Legal - Privacy - Data Collection and Storage

```
Your app uses the HealthKit frameworks but does not include the required privacy policy.

Next Steps

To resolve this issue, please update your app metadata to include a privacy policy URL and ensure that the URL you provide directs the user to your privacy policy.
```

问题原因：我们的app中使用到了 **HealthKit** ，读取用户的步数；


问题解决：

在app info.plist文件中加入两个key  

Privacy - Health Share Usage Description  
**APPName**需要您的同意才能访问您的健康，若不同意，则无法读取您的健康数据。  

Privacy - Motion Usage Description  
**APPName**需要您的同意才能访问您的运动与健康，若不同意，则无法使用您的健康与运动数据。

在 iTunes Connect 我的App -> App信息 中有个`隐私政策网址(URL)` 这里面需要填写一个URL，就是和登录或注册界面的用户协议那个URL一样的；

在 iTunes Connect 我的App -> 待审核或可供销售 -> App描述中加上一个提示语`“appName“已接入HealthKit，可同步训练数据到”个人“步数，授权 Apple Health 可一键同步运动数据。`

> Guideline 2.1 - Information Needed

```
We have started the review of your app, but we are not able to continue because we need access to a video that demonstrates the location feature while your app is running on the background.

Please ensure the video you provide shows a physical iOS device (not a simulator).

Next Steps

To help us proceed with the review of your app, please provide us with a link to a demo video in the App Review Information section of iTunes Connect and reply to this message in Resolution Center.

To provide a link to a demo video:

- Log in to iTunes Connect
- Click on "My Apps"
- Select your app
- Click on the app version on the left side of the screen
- Scroll down to "App Review Information"
- Provide demo video access details in the "Notes" section
- Once you've completed all changes, click the "Save" button at the top of the Version Information page.

If your iTunes Connect status shows as Metadata Rejected, we do not require a new binary to correct this issue. Please reply to this message in Resolution Center to confirm the availability of a valid demo video, and we will continue with the review.
```
问题原因：App -> TARGETS -> Capabilities -> Background Modes 中勾选了 `Location updates` 

问题解决：

需要给苹果审核团队提供一个视频，我们App中是跑步需要这个功能，然后我就拿着两部手机出去跑了一圈，一个手机用来录视频，一个手机运行App，然后把视频放到了优酷视频上，给审核团队提供了一个视频链接；

在 iTunes Connect 我的App -> 待审核或可供销售 -> App描述中加上一个提示语 `持续运行GPS在后台可能会降低您的电池续航时间。`最后完美通过。

> *持续更新...*

-----
*到此结束，一把辛酸泪，好记性不如烂笔头。*