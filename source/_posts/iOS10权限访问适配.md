---
title: iOS10权限访问适配
date: 2017-08-25 16:45:26
tags: iOS10
categories: iOS项目问题集
---

iOS10访问权限适配
iOS10调用相机、访问通讯录、相册等都要在plist中加入权限访问描述，不然项目会直接crash。

<!-- more -->

iOS10常用权限许可设置

	  
	相机权限： Privacy - Camera Usage Description  是否允许此App使用你的相机？
	相册权限： Privacy - Photo Library Usage Description 是否允许此App访问你的媒体资料库？
	通讯录权限： Privacy - Contacts Usage Description  是否允许此App访问你的通讯录？
	蓝牙权限：Privacy - Bluetooth Peripheral Usage Description 是否许允此App使用蓝牙？
    持续定位权限： Privacy - Location Always Usage Description 是否允许此App持续使用定位服务？
	定位权限：Privacy - Location When In Use Usage Description 是否允许此App使用定位服务？
	语音转文字权限：Privacy - Speech Recognition Usage Description 是否允许此App使用语音识别？
	日历权限：Privacy - Calendars Usage Description 是否允许此App使用日历？
    健康访问权限：Privacy - Health Share Usage Description 是否允许此App访问你的健康
    健康写入权限：Privacy - Health Update Usage Description 是否允许此App写入健康数据
    麦克风权限：Privacy - Microphone Usage Description 是否允许此App使用你的麦克风？
    运动与健身权限： Privacy - Motion Usage Description 是否允许此App访问您的运动与健身
       

--------

*到此结束，一把辛酸泪，好记性不如烂笔头。*