---
title: iOS状态栏颜色
date: 2018-08-07 15:08:06
tags: 状态栏
categories: iOS
---

> 人这种卑鄙的东西，什么都会习惯的。&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;——《罪与罚》


状态栏分为前后两部分：

```
1、前景部分：指的是显示电池、时间、wifi信号那部分；
2、背景部分：指的是前景的背景部分；
```

一般情况下说的状态栏颜色都是指前景部分。

<!-- more -->

#### 1、更改启动图上的状态栏颜色

`TARGETS` -> `General` -> `Deployment Info` -> `Status Bar Style` 

```
默认的黑色（UIStatusBarStyleDefault）
白色（UIStatusBarStyleLightContent）
```

下面的 `Hide status bar` 勾选后可以隐藏启动图的状态栏，视图中的还显示。

如果在`Info.plist`里面设置`Status bar style` 效果是一样的。

#### 2、统一设置app所有控制器view的状态栏颜色

`View controller-based status bar appearance` 属性在`Info.plist`文件中，如果将该属性设为`YES`,则控制器对状态的设置优先级最高，可以在控制器中对状态栏颜色进行单独修改；如果设置为`NO`的话，则以启动图设置为准，即app内部所有控制器view上状态栏情景部分颜色和启动图上保持一致，但还可以通过`UIApplication`管理，iOS9 以后已经不建议使用`UIApplication`管理状态栏了。

#### 3、单独设置某个控制器的状态栏颜色

两种情况：1、该控制器带导航控制器； 2、该控制器不带导航控制器

不管哪种情况首先将`View controller-based status bar appearance`设为`YES`

**（不带导航控制器）**

在该控制器内添加：

```
- (UIStatusBarStyle)preferredStatusBarStyle
{
	return UIStatusBarStyleLightContent;
}
```

**（带导航控制器）**

在该控制器内添加：

```
- (UIStatusBarStyle)preferredStatusBarStyle
{
	return UIStatusBarStyleLightContent;
}
```

还不够，需要自定义navigationController，然后在里面重写方法：

```
- (UIViewController *)childViewControllerForStatusBarStyle 
{
	return self.topViewController;
}
```

#### 4、在控制器view中随时更改状态栏颜色

需要调用`setNeedsStatusBarAppearanceUpdate`方法，这个方法会通知系统调用当前控制器的`preferredStatusBarStyle`方法，然后在该方法里根据情况来返回不同状态就行了。

-------
*到此结束，一把辛酸泪，好记性不如烂笔头。*