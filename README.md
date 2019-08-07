# 叉叉验证sdk文档

## sdk通用函数
叉叉验证sdk有个不常用的概念**sdk通用函数**, sdk内置有一些常用的函数,如**字符串拼接**, **字符串替换**来代替objc或者java原生函数,这些函数就是叉叉验证sdk中特有也是非常重要的**sdk通用函数**. 它们调用了iOS和Android的内核信息,检测了运行环境是否安全,sdk是否被改动,加密信息是否被破解,网络是否被劫持等情况,让破解者无从下手,极大地保证了您软件的安全性, 保证了您的正常收益不受侵犯. 

使用时请注意以下几点
1. 使用时候请在app启动时候调用**任一**一个通用函数**一次且只有一次**,来让sdk开始检测和监听安全信息,否则App会崩溃
2. 在收费功能中调用一次以上, sdk会自动检测app是否被破解
3. 请勿单独调用通用函数,比如创建局部字符串并打印,一定要让通用函数和您的app结合起来,水乳交融,增加hook难度
4. 初始化时候调用了哪个通用函数, 收费功能里面**一定**至少调用**相同**函数一次以上

### sdk通用函数包括

iOS
* (NSString*) concatString:(NSString*) dest src:(NSString*) src; // 字符串拼接
* (NSString*) replaceString:(NSString*) originalContent oldText:(NSString*) oldText newText:(NSString*) newText; //字符串替换

Android
* String concatString(String dest, String src);
* String replaceString(String originalContent, String oldText, String newText);



## sdk介绍
初次使用会弹框要求出入卡密,成功后弹框消失,下次启动会在后台自动验证, 通过后不会再弹框,免得影响用户体验. 如果之前验证通过,且当前网络不好,在一定时间内不会弹框,直到超过一定时间才会弹框要求输入卡密,以保证用户体验



## iOS

1. 将`VerifySDK.framework`拖入工程
2. 设置`TARGETS`->`General`->`Embedded Binaries`->`+` 添加 `VerifySDK.framework`
3. App启动时候调用`[VerifyCore.share init:@"111111"];`, 输入软件Id进行初始化
4. 初始化后调用任一sdk通用函数, 来判断软件运行环境是否合法, 注意只能调用通用函数一次
```
self.urls = [NSDictionary dictionaryWithObjectsAndKeys:
    [VerifyCore.share concatString:@"https://www.bai" src:@"du.com/s?wd=hello"], @"地址1",
    @"https://www.baidu.com/s?wd=world", @"地址2",
    nil];
```
5. 使用收费功能之前进行卡密验证
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
6. 在收费功能中调用一次以上, 初始化时候调用了哪个通用函数, 收费功能里面**一定**至少调用**相同**函数一次以上



## Android

1. TODO


## FAQ

### App崩溃了

sdk内置有个崩溃代码,来保证sdk被正确的调用,请逐次核对一下信息一一排查

* `[VerifyCore.sharedInstance initWithSoftId:]`是否被调用, 软件id是否为空
* App**启动时候**调用了sdk通用函数一次
* App启动时候**只调用**sdk通用函数**一次**
* App启动时候只调用sdk通用函数**其中之一一**一次, 不要所有的通用函数都调用
* 调用官方demo,查看是否崩溃
* 联系我们进行免费技术支持


## SDK下载
* [iOS SDK](#)
* [Android SDK](#)
