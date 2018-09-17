---
title: cell.detailTextLabel不显示
date: 2018-08-23 10:49:31
tags: UITableViewCell
categories: iOS
---


> 时光荏苒，声明短暂，别将时间浪费在争吵、道歉、伤心和责备上。用时间去爱吧，哪怕只有一瞬间，也不要辜负。&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;——马克吐温


```
static NSString *cellId = @"home_cellId";
UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellId];
if (cell == nil) {
    cell = [[UITableViewCell alloc]initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:cellId];
}
```

解决方法：`UITableViewCellStyleDefault` 替换为 `UITableViewCellStyleSubtitle`

-----
*到此结束，一把辛酸泪，好记性不如烂笔头。*
