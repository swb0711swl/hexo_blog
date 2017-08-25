---
title: iOS工具类，常用方法
date: 2017-08-25 17:10:03
tags: iOS工具类
categories: iOS项目问题集
---


	自我成长与提升重在平常的总结和积累

开场装了个小比，下面开始正题

<!-- more -->

##### 1、判断字符串是否为空
```
+ (BOOL)isEmpty:(NSString *)str
{
	if (!str) {
    	return YES;
    }else {
    	//去空格
    	NSCharacterSet *set = [NSCharacterSet whitespaceAndNewlineCharacterSet];
        NSString *trimedString = [str stringByTrimmingCharactersInSet:set];
        if ([trimedString length] == 0) {
        	return YES;
        } else {
        	return NO;
        }
    }
}
```

##### 2、对象万能判空
```
+ (BOOL)isNull:(id)obj
{
	if ([obj isKindOfClass:[NSString class]]) {
    	return obj == nil || [obj isEqual:[NSNull null]] || [obj isEqualToString:@"<null>"] || [self isEmpty:obj];
    }else {
    	return obj == nil || [obj isEqual:[NSNull null]];
    }
}
```

##### 3、判断是否是手机号
```
+ (BOOL)isPhoneNum:(NSString *)mobileNum
{
    NSString *MOBILE = @"^1(3[0-9]|4[57]|5[0-35-9]|8[0-9]|7[0678])\\d{8}$";
    NSPredicate *regextestmobile = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", MOBILE];
    return [regextestmobile evaluateWithObject:mobileNum];
}
```

##### 4、精确的判断身份证号码
```
+ (BOOL)accurateVerifyIDCardNumber:(NSString *)value
{
    value = [value stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
    
    int length =0;
    if (!value) {
        return NO;
    }else {
        length = (int)value.length;
        
        if (length !=15 && length !=18) {
            return NO;
        }
    }
    // 省份代码
    NSArray *areasArray =@[@"11",@"12", @"13",@"14", @"15",@"21", @"22",@"23", @"31",@"32", @"33",@"34", @"35",@"36", @"37",@"41", @"42",@"43", @"44",@"45", @"46",@"50", @"51",@"52", @"53",@"54", @"61",@"62", @"63",@"64", @"65",@"71", @"81",@"82", @"91"];
    
    NSString *valueStart2 = [value substringToIndex:2];
    BOOL areaFlag =NO;
    for (NSString *areaCode in areasArray) {
        if ([areaCode isEqualToString:valueStart2]) {
            areaFlag =YES;
            break;
        }
    }
    
    if (!areaFlag) {
        return false;
    }
    
    NSRegularExpression *regularExpression;
    NSUInteger numberofMatch;
    
    int year =0;
    switch (length) {
        case 15:
        year = [value substringWithRange:NSMakeRange(6,2)].intValue +1900;
        
        if (year %4 ==0 || (year %100 ==0 && year %4 ==0)) {
            
            regularExpression = [[NSRegularExpression alloc] initWithPattern:@"^[1-9][0-9]{5}[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}$"
                                                                     options:NSRegularExpressionCaseInsensitive
                                                                       error:nil];//测试出生日期的合法性
        }else {
            regularExpression = [[NSRegularExpression alloc]initWithPattern:@"^[1-9][0-9]{5}[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))[0-9]{3}$"
                                                                    options:NSRegularExpressionCaseInsensitive
                                                                      error:nil];//测试出生日期的合法性
        }
        numberofMatch = [regularExpression numberOfMatchesInString:value
                                                           options:NSMatchingReportProgress
                                                             range:NSMakeRange(0, value.length)];
        
        if(numberofMatch >0) {
            return YES;
        }else {
            return NO;
        }
        case 18:
        year = [value substringWithRange:NSMakeRange(6,4)].intValue;
        if (year %4 ==0 || (year %100 ==0 && year %4 ==0)) {
            
            regularExpression = [[NSRegularExpression alloc] initWithPattern:@"^[1-9][0-9]{5}19[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}[0-9Xx]$"
                                                                     options:NSRegularExpressionCaseInsensitive
                                                                       error:nil];//测试出生日期的合法性
        }else {
            regularExpression = [[NSRegularExpression alloc] initWithPattern:@"^[1-9][0-9]{5}19[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))[0-9]{3}[0-9Xx]$"
                                                                     options:NSRegularExpressionCaseInsensitive
                                                                       error:nil];//测试出生日期的合法性
        }
        numberofMatch = [regularExpression numberOfMatchesInString:value
                                                           options:NSMatchingReportProgress
                                                             range:NSMakeRange(0, value.length)];
        
        if(numberofMatch >0) {
            int S = ([value substringWithRange:NSMakeRange(0,1)].intValue + [value substringWithRange:NSMakeRange(10,1)].intValue) *7 + ([value substringWithRange:NSMakeRange(1,1)].intValue + [value substringWithRange:NSMakeRange(11,1)].intValue) *9 + ([value substringWithRange:NSMakeRange(2,1)].intValue + [value substringWithRange:NSMakeRange(12,1)].intValue) *10 + ([value substringWithRange:NSMakeRange(3,1)].intValue + [value substringWithRange:NSMakeRange(13,1)].intValue) *5 + ([value substringWithRange:NSMakeRange(4,1)].intValue + [value substringWithRange:NSMakeRange(14,1)].intValue) *8 + ([value substringWithRange:NSMakeRange(5,1)].intValue + [value substringWithRange:NSMakeRange(15,1)].intValue) *4 + ([value substringWithRange:NSMakeRange(6,1)].intValue + [value substringWithRange:NSMakeRange(16,1)].intValue) *2 + [value substringWithRange:NSMakeRange(7,1)].intValue *1 + [value substringWithRange:NSMakeRange(8,1)].intValue *6 + [value substringWithRange:NSMakeRange(9,1)].intValue *3;
            int Y = S %11;
            NSString *M =@"F";
            NSString *JYM =@"10X98765432";
            M = [JYM substringWithRange:NSMakeRange(Y,1)];// 判断校验位
            if ([M isEqualToString:[value substringWithRange:NSMakeRange(17,1)]]) {
                return YES;// 检测ID的校验位
            }else {
                return NO;
            }
            
        }else {
            return NO;
        }
        default:
        return NO;
    }
}
```

##### 5、获取当前VC
```
+ (UIViewController *)getVisibleVcFrom:(UIViewController*)vc {
    if ([vc isKindOfClass:[UINavigationController class]]) {
        return [self getVisibleVcFrom:[((UINavigationController*) vc) visibleViewController]];
    }else if ([vc isKindOfClass:[UITabBarController class]]){
        return [self getVisibleVcFrom:[((UITabBarController*) vc) selectedViewController]];
    } else {
        if (vc.presentedViewController) {
            return [self getVisibleVcFrom:vc.presentedViewController];
        } else {
            return vc;
        }
    }
}
+ (UIViewController *)getCurrentVC
{
	[self getVisibleVcFrom:(UIViewController *)appDelegate.window.rootViewController]];
}
```

##### 6、打电话
```
+ (void)dialPhone:(NSString *)phoneNum
{
	NSURL *url = [NSURL URLWithString:[NSString stringWithFormat:@"telprompt://%@",phoneNum]];
    [[UIApplication sharedApplication] openURL:url];
}
```

-----

时间相关的常用方法

##### 1、获取当前时间戳
```
+ (NSString *)getCurrentTimeStamp
{
	NSDate *dat = [NSDate dateWithTimeIntervalSinceNow:0];
    NSTimeInterval interval = [dat timeIntervalSince1970];
    NSString *timeStamp = [NSString stringWithFormat:@"%0.f",interval];
    return timeStamp;
}
```

##### 2、与当前时间比较：显示 昨天、今天、刚刚
```
+ (NSString *)dateFormat:(NSString *)dateString
{
    @try {
        NSString *targetDateStr = [self timeStamp:dateString format:@"yyyy年MM月dd日 HH:mm:ss"];
        NSDateFormatter *dateFormatter = [[NSDateFormatter alloc]init];
        [dateFormatter setDateFormat:@"yyyy年MM月dd日 HH:mm:ss"];
        NSDate *nowDate = [NSDate date];
        
        NSDate *targetDate = [dateFormatter dateFromString:targetDateStr];
        //取当前时间和转换时间两个日期对象的时间间隔
        NSTimeInterval time = [nowDate timeIntervalSinceDate:targetDate];
        
        NSString *resultStr = [[NSString alloc] init];
        if (time <= 60) {//1分钟以内
            resultStr = @"刚刚";
        }else if (time < 3600) {//一个小时以内
            resultStr = [NSString stringWithFormat:@"%d分钟前",(int)time/60];
        }else if (time <= 60*60*24) {//在两天内
            [dateFormatter setDateFormat:@"yyyy年MM月dd日"];
            NSString *target_yMd = [dateFormatter stringFromDate:targetDate];
            NSString *now_yMd = [dateFormatter stringFromDate:nowDate];
            [dateFormatter setDateFormat:@"HH:mm"];
            if ([target_yMd isEqualToString:now_yMd]) {
                //同一天
                resultStr = [NSString stringWithFormat:@"今天 %@",[dateFormatter stringFromDate:targetDate]];
            }else {
                //昨天
                resultStr = [NSString stringWithFormat:@"昨天 %@",[dateFormatter stringFromDate:targetDate]];
            }
        }else {
            [dateFormatter setDateFormat:@"yyyy"];
            NSString *target_y = [dateFormatter stringFromDate:targetDate];
            NSString *now_y = [dateFormatter stringFromDate:nowDate];
            if ([target_y isEqualToString:now_y]) {
                //同一年
                [dateFormatter setDateFormat:@"MM月dd日"];
                resultStr = [dateFormatter stringFromDate:targetDate];
            }else {
                [dateFormatter setDateFormat:@"yyyy年MM月dd日"];
                resultStr = [dateFormatter stringFromDate:targetDate];
            }
        }
        return resultStr;
    } @catch (NSException *exception) {
        return @"";
    } @finally {
        
    }
}
```

##### 3、判断某个时间是否在一天中的某个时间段内
```
+ (BOOL)isBetweenFromHour:(NSInteger)fromHour toHour:(NSInteger)toHour
{
    NSDate *date8 = [self getCustomDateWithHour:fromHour];
    NSDate *date23 = [self getCustomDateWithHour:toHour];
    
    NSDate *currentDate = [NSDate date];
    
    if ([currentDate compare:date8]==NSOrderedDescending && [currentDate compare:date23]==NSOrderedAscending)
    {
        NSLog(@"该时间在 %d:00-%d:00 之间！", (int)fromHour, (int)toHour);
        return YES;
    }
    return NO;
}
 
 //生成当天的某个点,如hour为“8”，就是上午8:00（本地时间）
+ (NSDate *)getCustomDateWithHour:(NSInteger)hour
{
    //获取当前时间
    NSDate *currentDate = [NSDate date];
    NSCalendar *currentCalendar = [[NSCalendar alloc] initWithCalendarIdentifier:NSCalendarIdentifierGregorian];
    NSDateComponents *currentComps = [[NSDateComponents alloc] init];
    
    NSInteger unitFlags = NSCalendarUnitYear | NSCalendarUnitMonth | NSCalendarUnitDay | NSCalendarUnitWeekday | NSCalendarUnitHour | NSCalendarUnitMinute | NSCalendarUnitSecond;
    
    currentComps = [currentCalendar components:unitFlags fromDate:currentDate];
    
    //设置当天的某个点
    NSDateComponents *resultComps = [[NSDateComponents alloc] init];
    [resultComps setYear:[currentComps year]];
    [resultComps setMonth:[currentComps month]];
    [resultComps setDay:[currentComps day]];
    [resultComps setHour:hour];
    
    NSCalendar *resultCalendar = [[NSCalendar alloc] initWithCalendarIdentifier:NSCalendarIdentifierGregorian];
    return [resultCalendar dateFromComponents:resultComps];
}
```

##### 4、RAC实现的倒计时
```
+ (RACSignal *)timerCountdown:(int)times
{
    RACSignal *timer = [RACSignal interval:1 onScheduler:[RACScheduler mainThreadScheduler]];
    return [[[timer scanWithStart:@(times) reduce:^id(NSNumber *running, id _) {
        return @(running.intValue - 1);
    }]takeUntilBlock:^BOOL(NSNumber *x) {
        return x.intValue < 0;
    }]takeUntil:self.rac_willDeallocSignal];
}
```

##### 5、RAC实现的正计时
```
+ (RACSignal *)timerPositive:(int)times withStartTime:(NSTimeInterval)startTime
{
    RACSignal *timer = [RACSignal interval:1 onScheduler:[RACScheduler mainThreadScheduler]];
    return [[[timer scanWithStart:@(startTime) reduce:^id(NSNumber *running, id _) {
        return @(running.intValue + 1);
    }]takeUntilBlock:^BOOL(NSNumber *x) {
        return x.intValue > times;
    }]takeUntil:self.rac_willDeallocSignal];
}
```

##### 6、RAC实现的每隔多长时间执行一次
```
+ (RACSignal *)handleInterval:(int)time
{
	@WeakObj(self);
    [[[[RACSignal interval:time onScheduler:[RACScheduler mainThreadScheduler]] takeUntilBlock:^BOOL(id x) {
        @StrongObj(self);
        return 表达式;//为YES时停止
    }]takeUntil:self.rac_willDeallocSignal] subscribeNext:^(id x) {
        @StrongObj(self);
        //你自己的操作（每隔time秒执行一次）
    }];
}
```

-------
*到此结束，一把辛酸泪，好记性不如烂笔头。*
