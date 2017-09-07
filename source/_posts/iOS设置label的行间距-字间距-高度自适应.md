---
title: iOS设置label的行间距+字间距+高度自适应
date: 2017-09-07 15:02:12
tags: UILabel
categories: iOS项目问题集
---

> 越有本事的人越没脾气。因为素质、修为、涵养、学识、能力和财力会综合一个人的品格。

<!-- more -->

将这些方法写在UILabel的分类中，方便调用。

##### 1、设置行间距

```
//设置行间距
- (void)setLineSpace:(CGFloat)lineSpcae withString:(NSString *)str
{
    if (lineSpcae < 0.01 || !str) {
        return;
    }
    NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc]init];
    paragraphStyle.lineSpacing = lineSpcae;//行间距 默认为0
    paragraphStyle.lineBreakMode = NSLineBreakByWordWrapping;
    paragraphStyle.hyphenationFactor = 1;//连字符，换行的时候同一个单词用连字符连接
    NSDictionary *dic = @{NSParagraphStyleAttributeName:paragraphStyle};
    NSAttributedString *attributeStr = [[NSAttributedString alloc]initWithString:str attributes:dic];
    self.attributedText = attributeStr;
}
```

##### 2、设置字间距

```
//设置字间距
- (void)setWordLineSpace:(CGFloat)wordSpace withString:(NSString *)str
{
    if (wordSpace < 0.01 || !str) {
        return;
    }
    NSDictionary *dic = @{NSKernAttributeName:@(wordSpace)};
    NSAttributedString *attributeStr = [[NSAttributedString alloc]initWithString:str attributes:dic];
    self.attributedText = attributeStr;
}
```

##### 3、计算带行间距的文字长度 自适应高度

```
- (CGSize)sizeWithString:(NSString *)string withLineSpace:(CGFloat)lineSpace sizemake:(CGSize) withsize
{
    if (!string) {
        return CGSizeZero;
    }
    NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc]init];
    paragraphStyle.lineSpacing = lineSpace;
    paragraphStyle.hyphenationFactor = 1.0;
    NSDictionary *dic = @{NSParagraphStyleAttributeName:paragraphStyle,NSFontAttributeName:self.font};
    CGSize size = [string boundingRectWithSize:withsize options:NSStringDrawingTruncatesLastVisibleLine | NSStringDrawingUsesFontLeading | NSStringDrawingUsesLineFragmentOrigin attributes:dic context:nil].size;
    return size;
}
```