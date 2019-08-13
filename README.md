# 叉叉验证sdk文档 - 极简文档

#### [极简用法](#)
#### [高级功能](https://github.com/xxyanzheng/sdk/blob/master/advance.md)
#### [平台说明](https://github.com/xxyanzheng/sdk/blob/master/platform.md)


## sdk介绍
初次使用会弹框要求出入卡密,成功后弹框消失,下次启动会在后台自动验证, 通过后不会再弹框,免得影响用户体验. 如果之前验证通过,且当前网络不好,在一定时间内不会弹框,直到超过一定时间才会弹框要求输入卡密,以保证用户体验


## iOS

1. 将`YanzhengSDK.framework`拖入工程
2. 设置`TARGETS`->`General`->`Embedded Binaries`->`+` 添加 `YanzhengSDK.framework`
3. 引用 `#import <YanzhengSDK/YanzhengSDK.h>`
4. App启动时候调用`setup`, 传入软件Id进行初始化
```
[YanzhengCore.share setup:@"111111"]; // objc
(YanzhengCore.share() as! YanzhengCore).setup("1111111") // swift
```
5. 使用收费功能之前进行卡密验证

objc
```
YanzhengCoreDelegate* delegate = [[YanzhengCoreDelegate alloc] initWithFailHandler:^(NSError * _Nonnull error) {
    NSLog(@"网络等其他位置错误，需要自己处理: %@", error);
} errorHandler:^(NSString * _Nullable error) {
    NSLog(@"卡密错误，直接弹框重新输入卡密: %@", error);
} successHandler:^(NSDate * _Nonnull startDate, NSDate * _Nonnull endDate, NSString * _Nonnull macAddr) {
    NSLog(@"登录成功，\n激活日期: %@ \n有效期至: %@ \n现在时间: %@\n硬件Id: %@", startDate, endDate, [NSDate new], macAddr);
    [self notFreeFeature];//调用收费功能,跳转界面等, 也可以啥都不做
}];
[YanzhengCore.share login: delegate];
```

swift
``` 
let delegate = YanzhengCoreDelegate.init(failHandler: { (error) in
    print("网络等其他位置错误，需要自己处理: \(error)")
}, errorHandler: { (error) in
    print("卡密错误，直接弹框重新输入卡密: \(error!)")
}) { (startDate, endDate, macAddr) in
    print("登录成功，\n激活日期: \(startDate) \n有效期至: \(endDate) \n现在时间: \(Date())\n硬件Id: \(macAddr)")
    self.notFreeFeature()
}
(YanzhengCore.share() as! YanzhengCore).login(delegate)
```



## Android

1. 解压sdk文件到工程文件目录`app/libs`, 最终的目录结构为`app/libs/yanzhengSDK.aar`
```
-app
----libs
-------- yanzhengSDK.aar
-------- x86
-------- x86_64
-------- arm64-v8a
-------- armeabi-v7a
```
2. 在 `app/build.gradle` 添加配置
```
android { 
    repositories { flatDir { dirs 'libs' } }
    sourceSets { main { jniLibs.srcDirs=['libs'] }}
}
compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
}
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation (name:'yanzhengSDK', ext:'aar')
    implementation 'com.squareup.okhttp3:okhttp:3.14.1'
}
```
3. 在`AndoridManifest.xml`中申请权限
```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<application android:usesCleartextTraffic="true">
```
4. App启动时候调用`VerifyCore.share().setup(activity, "1111111");`, 传入当前activity和软件Id进行初始化
5. 使用收费功能之前进行卡密验证
```
VerifyCore.VerifyCoreCallback callback = new VerifyCore.VerifyCoreCallback() {
    @Override
    public void successHandler(Date startDate, Date endDate, String madAddr) {
        Log.d(TAG, "登录成功，\n激活日期: " + startDate + "\n有效期至: " + endDate + "\n现在时间: " + new Date() + "\n硬件Id: " + madAddr);
        notFreeFeature(); //调用收费功能,跳转界面等, 也可以啥都不做
    }
    @Override
    public void failHandler(Exception exception) {
        Log.e(TAG, "网络等其他位置错误，需要自己处理: " + exception);
    }
    @Override
    public void errorHandler(String message) {
        Log.e(TAG, "卡密错误，直接弹框重新输入卡密: " + message);
    }
};
VerifyCore.share().login(callback);
```



## FAQ

### App崩溃了

sdk内置有个崩溃代码,来保证sdk被正确的调用,请逐次核对一下信息一一排查

* 检查log是否打印有crash的原因
* `[VerifyCore.sharedInstance initWithSoftId:]`是否被调用, 软件id是否为空
* 在[高级功能](https://github.com/xxyanzheng/sdk/blob/master/advance.md)查看更多可能原因
* 调用官方demo,查看是否崩溃
* 联系我们进行免费技术支持

## SDK下载
* [iOS SDK](https://github.com/xxyanzheng/sdk/blob/master/sdk/ios.zip)
* [Android SDK](https://github.com/xxyanzheng/sdk/blob/master/sdk/android.zip)
