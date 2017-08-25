---
title: iOS原生二维码扫码全屏预览(可设置扫描区域)
date: 2017-08-07 16:47:42
tags: 二维码
categories: iOS项目问题集
---

项目有4个功能，扫码、手电筒、相册识别二维码、生成二维码。
为了方便几个功能都写一起了，下面分开实现，把我自己在项目中遇到的问题重点记录一下，代码demo中都有。

-------

<!-- more -->

先上效果图片[项目传送门](https://github.com/swb0711swl/SWBCodeScanViewDemo)![](http://ou3g2kov0.bkt.clouddn.com/hexo/image/二维码扫描.jpg?imageView2/0/q/75|watermark/2/text/5LiH5oG255qE5bCP5b2s5b2s/font/5a6L5L2T/fontsize/300/fill/I0ZGRkZGRg==/dissolve/80/gravity/SouthEast/dx/10/dy/10|imageslim)

iOS10访问权限适配
`iOS10调用相机、访问通讯录、相册等都要在plist中加入权限访问描述，不然项目会直接crash。`

项目中用到的
```
相机权限： Privacy - Camera Usage Description 是否允许此App使用你的相机？
相册权限： Privacy - Photo Library Usage Description 是否允许此App访问你的媒体资
```

##### 1、扫码

头文件：`#import <AVFoundation/AVFoundation.h>`
实现代理：`AVCaptureMetadataOutputObjectsDelegate`

主要属性：
```
@property (strong, nonatomic) AVCaptureDevice *myDevice;//创建相机
@property (strong, nonatomic) AVCaptureDeviceInput *deviceInput;//输入流
@property (strong, nonatomic) AVCaptureMetadataOutput *metadataOutput;//媒体输出流
@property (strong, nonatomic) AVCaptureSession *session;//捕捉会话
@property (strong, nonatomic) AVCaptureVideoPreviewLayer *previewLayer;//视觉预览层
```
代码就不往上贴了，demo里都有，这里有两行代码着重记录一下
```
1.//设置扫描区域的大小 (这里要宽和高要颠倒一下，具体原因我特么也不清楚)  默认值（0，0，1，1）
self.metadataOutput.rectOfInterest = CGRectMake((SCREEN_HEIGHT/2-64-CodeScanHeight/2)/SCREEN_HEIGHT, (SCREEN_WIDTH/2-CodeScanWidth/2)/SCREEN_WIDTH, CodeScanHeight/SCREEN_HEIGHT, CodeScanWidth/SCREEN_WIDTH);
    
2.//背景全屏预览
self.previewLayer.frame = CGRectMake(0, 0, SCREEN_WIDTH, SCREEN_HEIGHT);

```

中间那个扫描框搞的我也是醉醉的，刚开始直接设置的self.previewLayer.frame是扫描框的frame，但是背景就不能全屏预览了，所以还得使用rectOfInterest属性来限制扫描区域。

##### 2、打开手电筒

直接贴代码
```
- (void)openFlashLamp
{
	AVCaptureDevice *device = [AVCaptureDevice defaultDeviceWithMediaType:AVMediaTypeVideo];
    if ([device hasTorch])//判断设备是否有摄像头，模拟器没有
    {
    	//锁定系统摄像头
        [device lockForConfiguration:nil];
        if (device.torchMode == AVCaptureTorchModeOff)
        {
        	[device setTorchMode:AVCaptureTorchModeOn];
        }else {
        	[device setTorchMode:AVCaptureTorchModeOff];
        }
        //解除锁定
        [device unlockForConfiguration];
    }
}
```

##### 3、识别相册二维码

实现代理`UIImagePickerControllerDelegate,UINavigationControllerDelegate`
1.打开相册
```
- (void)openPhotoLibrary
{
	UIImagePickerController *picker = [[UIImagePickerController alloc]init];
    picker.sourceType = UIImagePickerControllerSourceTypePhotoLibrary;
    picker.allowsEditing = YES;
    picker.delegate = self;
    [[self getCurrentVC] presentViewController:picker animated:YES completion:nil];
}
```
2.实现代理
```
#pragma mark    UIImagePickerControllerDelegate
- (void)imagePickerControllerDidCancel:(UIImagePickerController *)picker
{
    [[self getCurrentVC] dismissViewControllerAnimated:YES completion:nil];
}
- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary<NSString *,id> *)info
{
    UIImage *img = info[UIImagePickerControllerEditedImage];
    if (!img) {
        img = info[UIImagePickerControllerOriginalImage];
    }
    @WeakObj(self);
    [[self getCurrentVC] dismissViewControllerAnimated:YES completion:^{
        @StrongObj(self);
        [self readQRcodeImg:img];
    }];
}
```
3.识别相册二维码，读取二维码信息
```
- (void)readQRcodeImg:(UIImage *)img
{
	NSData *imgData = UIIamgeJPEGRepresentation(img, 1);
    CIContext *context = [CIContext contextWithOptions:@{kCIContextUseSoftwareRenderer:@(true),kCIContextPriorityRequestLow:@(false)}];
    CIDetector *detector = [CIDetector detectorOfType:CIDetectorTypeQRCode context:context options:nil];
    CIImage *image = [CIImage imageWithData:imgData];
    NSArray *arr = [detector featuresInImage:image];
    if (arr.count >= 1) {
    	CIQRCodeFeature *feature = [arr firstObject];
        //读取结果
        if (self.delegateSubject) {
        	[self.delegateSubject sendNext:feature.messageString];
        }
    }else {
        UIAlertController *alter = [UIAlertController alertControllerWithTitle:@"扫描结果" message:@"不是二维码图片" preferredStyle:UIAlertControllerStyleAlert];
        UIAlertAction *cancleAction = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleCancel handler:nil];
        [alter addAction:cancleAction];
        [[self getCurrentVC] presentViewController:alter animated:YES completion:nil];
    }
}
```

##### 4、生成二维码

这个是用的一个别人写好的，挺不错的，可以自定义颜色和logo。一个方法调用就可生成，很方便。

-------
*到此结束，一把辛酸泪，好记性不如烂笔头。*

