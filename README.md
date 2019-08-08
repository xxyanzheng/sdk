# 叉叉验证sdk文档 - 极简文档

## [高级功能请跳转](https://github.com/xxyanzheng/sdk/blob/master/andvance.md)


## sdk介绍
初次使用会弹框要求出入卡密,成功后弹框消失,下次启动会在后台自动验证, 通过后不会再弹框,免得影响用户体验. 如果之前验证通过,且当前网络不好,在一定时间内不会弹框,直到超过一定时间才会弹框要求输入卡密,以保证用户体验


## iOS

1. 将`VerifySDK.framework`拖入工程
2. 设置`TARGETS`->`General`->`Embedded Binaries`->`+` 添加 `VerifySDK.framework`
3. App启动时候调用`[VerifyCore.share init:@"111111"];`, 输入软件Id进行初始化
4. 使用收费功能之前进行卡密验证
```
VerifyCoreDelegate* delegate = [[VerifyCoreDelegate alloc] initWithFailHandler:^(NSError * _Nonnull error) {
    NSLog(@"网络等其他位置错误，需要自己处理: %@", error);
} errorHandler:^(NSString * _Nullable error) {
    NSLog(@"卡密错误，直接弹框重新输入卡密: %@", error);
} successHandler:^(NSDate * _Nonnull startDate, NSDate * _Nonnull endDate, NSString * _Nonnull macAddr) {
    NSLog(@"登录成功，\n激活日期: %@ \n有效期至: %@ \n现在时间: %@\n硬件Id: %@", startDate, endDate, [NSDate new], macAddr);
    [self notFreeFeature]; //调用收费功能,跳转界面等
}];
[VerifyCore.share login: delegate];
```



## Android

1. TODO


## FAQ

### App崩溃了

sdk内置有个崩溃代码,来保证sdk被正确的调用,请逐次核对一下信息一一排查

* 检查log是否打印有crash的原因
* `[VerifyCore.sharedInstance initWithSoftId:]`是否被调用, 软件id是否为空
* 调用官方demo,查看是否崩溃
* 联系我们进行免费技术支持


## [高级功能请跳转](https://github.com/xxyanzheng/sdk/blob/master/andvance.md)

## SDK下载
* [iOS SDK](#)
* [Android SDK](#)
