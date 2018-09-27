---
title: appStore上架问题集
date: 2018-05-04 17:44:47
tags: app Store上架
categories: iOS
---

> 不必太纠结于当下，也不必太忧虑未来，当你经历过一些事情的时候，眼前的风景已经和从前不一样了。&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;——村上村树《1Q84》

app每次上架过程就是一部血泪史，各种被拒，没看审核文档，一步一步踩着坑过来，其中还是有很多值得注意的地方的，将各种问题及解决办法收集起来汇成一部宝典，伴随自己成长。

<!-- more --->

---

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

> Guideline 2.3.3 - Performance - Accurate Metadata

```
We noticed that your screenshots do not sufficiently reflect your app in use.

Please see attached screenshots.

Next Steps

To resolve this issue, please revise your screenshots to ensure that they accurately reflect the app in use on the supported devices. For iPhone, you need a set of 5.5-inch display screenshots and for iPad, you need a set for 12.9-inch display. This set will be scaled appropriately down to other device sizes when viewed on the App Store in each territory.

Note that 5.8-inch display assets for iPhone X are optional and don't scale down to other devices sizes. Screenshots that include iPhone X layout features like rounded corners or sensor housing should only be used for the 5.8-inch display. 

Resources

For resources on creating great screenshots for the App Store, you may want to review the App Store Product Page information available on the Apple developer portal.

Please ensure you have made any screenshot modifications using Media Manager. You should confirm your app looks and behaves identically in all languages and on all supported devices. Learn how to use Media Manager to add custom screenshots for each display size and localization.

Since your App Store Connect status is Rejected, a new binary will be required. Make the desired metadata changes when you upload the new binary.

NOTE: Please be sure to make any metadata changes to all app localizations by selecting each specific localization and making appropriate changes.
```

问题原因：我们App Store上传的预览图是用iPhone X截屏的，本来应该是5.8英寸的，结果UI给做成了5.5英寸的，也就是说，尺寸是5.5英寸的，但预览图截屏是iPhone X的，不对应。

问题解决：
iPhone X截屏上传到5.8英寸的预览图中，iPhone Plus截屏上传到5.5英寸的预览图中，不能把iPhone X截屏处理成5.5英寸的。

> Guideline 2.3.6 - Performance - Accurate Metadata

```
The rating you have selected, 4+, is inconsistent with the content of your app. Since your app includes contests, sweepstakes, real money gambling, or real money betting, you must select "Yes" for Gambling and Contests in App Store Connect.

Next Steps

To resolve this issue, please update your Rating selections in App Store Connect.

- Log in to App Store Connect
- Click on "My Apps"
- Select your app
- Scroll down to select a Rating on the App Details page
- Click the Edit button next to "Rating"
- Select "Yes" for Gambling and Contests
- Click "Done"
- Once you've completed all changes, click the "Save" button at the top of the App Version Information page.

Note: Apps must be rated accordingly for the highest level of content that the user is able to access in the app.
```

问题原因：项目中接入了抽奖

问题解决：在app分级中，“赌博与竞赛”选项选择是，此时年龄会自动设为17+岁。


> Guideline 2.4.1 - Performance - Hardware Compatibility

```
We noticed that your app did not run or display as expected when viewed on iPad running iOS 11.4. Please see attached screenshots for details. 

Next Steps

To resolve this issue, please revise your app to ensure it runs as expected and displays properly at iPhone resolution on iPad. Even if your app was developed specifically for iPhone, users should still be able to use your app on iPad. 

Resources

For information on iOS device screen sizes and resolutions, please review the iOS Human Interface Guidelines as well as Points versus Pixels in the View Programming Guide for iOS. 

You may also want to view Size Classes and Core Components and Default Class Sizes for Different Devices for more information about designing apps for multiple screen sizes
```

问题原因：我们的登录页面在iPad上不能正常登录，因为没有登录按钮，上架前先用iPad运行一下，看看能不能正常登录，这种连登录都没通过的被拒，感觉好气呀，但还是要保持微笑。

问题解决：单独适配或底部放一个scrollViewview。

> Guideline 2.5.1 - Performance - Software Requirements

```
Your app uses the "prefs:root=" non-public URL scheme, which is a private entity. The use of non-public APIs is not permitted on the App Store because it can lead to a poor user experience should these APIs change.

Continuing to use or conceal non-public APIs in future submissions of this app may result in the termination of your Apple Developer account, as well as removal of all associated apps from the App Store.

Next Steps

To resolve this issue, please revise your app to provide the associated functionality using public APIs or remove the functionality using the "prefs:root" or "App-Prefs:root" URL scheme.

If there are no alternatives for providing the functionality your app requires, you can file an enhancement request.
```

问题原因：App跳转系统设置界面使用了[[UIApplication sharedApplication]openURL:[NSURL URLWithString:@"App-Prefs:root=LOCATION"] options:@{} completionHandler:nil]

解决：将上述代码换成[[UIApplication sharedApplication] openURL:[NSURL URLWithString:UIApplicationOpenSettingsURLString]]即可。



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


> Guideline 5.3.2 - Legal - Gaming, Gambling, and Lotteries

```
Your app includes a contest or sweepstakes but it does not:

- Include official rules for the sweepstake within the app, which is required.
- Indicate that Apple is not involved in any way with the contest or sweepstakes.
- Enforce an app age rating of 17+.

Next Steps

It is necessary to:

- Include official rules of the contest or sweepstakes in the app
- Include an explicit statement in the contest or sweepstakes rules specifying that Apple is not a sponsor.
- Enforce an age rating of 17+.
```

问题原因：App中有抽奖功能

问题解决：在抽奖规则和抽奖页面添加`*奖品与活动和设备生产商Apple Inc.公司无关`。然后在用户协议中添加`通过本软件参加的任何商业活动或者奖励活动，由本公司提供，均与Apple Inc.无关。`

> *持续更新...*

-----
*到此结束，一把辛酸泪，好记性不如烂笔头。*