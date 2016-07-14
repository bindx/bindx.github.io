bindx.github.io
===============

Bindx

```objc
#import <LinkedME_iOS/LinkedME.h>
```

```objc
-(IBAction)creatSearchableItem{
    NSSet *set5 = [NSSet setWithObjects:@"linkedME", nil];
    //
    NSDictionary *dict = @{@"test":@"test"};
    
    /*
     *  @param title             标题
     *  @param description       描述
     *  @param publiclyIndexable 是否公开
     *  @param type              NSUserActivity类型,获取MobileCoreServices框架中的列表
     *  @param thumbnailUrl      缩略图Url
     *  @param keywords          关键字
     *  @param userInfo          用户详情
     *  @param expirationDate    失效日期,设置失效日期会自动删除索引
     *  @param identifier        标志符
     *  @param callback          回调
     *  @param spotlightCallback Spotlight回掉
     */
    
    [[LinkedME getInstance] createDiscoverableContentWithTitle:@"LinkedME 国内第一家企业级深度链接" description:@"让APP不再是信息孤岛!" thumbnailUrl:[NSURL URLWithString:@"http://7xq8b0.com1.z0.glb.clouddn.com/logo.png"] linkParams:dict type:@"" publiclyIndexable:NO keywords:set5 expirationDate:nil spotlightIdentifier:@"bbcc" spotlightCallback:^(NSString *url, NSString *spotlightIdentifier, NSError *error) {
        [self performSelectorOnMainThread:@selector(showAlert:) withObject:@"索引创建成功" waitUntilDone:NO];
    }];
}
```

```objc
- (IBAction)deleteSearchableItemFormIdentifier{
    [LinkedME removeSearchItemWith:@[@"bbcc"]];
}```

```objc
- (IBAction)deleteAllSearchableItem{
    [LinkedME removeAllSearchItems];
}```


```objc
//Universal Links 通用链接实现深度链接技术
- (BOOL)application:(UIApplication*)application continueUserActivity:(NSUserActivity*)userActivity restorationHandler:(void (^)(NSArray*))restorationHandler{
    return  [[LinkedME getInstance] continueUserActivity:userActivity];
}
```

```objc
 
 - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    LinkedME* linkedme = [LinkedME getInstance];
    //获取跳转参数
    [linkedme initSessionWithLaunchOptions:launchOptions automaticallyDisplayDeepLinkController:NO deepLinkHandler:^(NSDictionary* params, NSError* error) {
        if (!error) {
            @try {
                
            } @catch (NSException *exception) {
                
            } @finally {
                
            }
        } else {
            NSLog(@"LinkedME failed init: %@", error);
        }
    }];
    return YES;
}
```


```objc
//创建短链
-(void)addPara{
    self.linkedUniversalObject = [[LMUniversalObject alloc] init];
    self.linkedUniversalObject.title = title;//标题
    LMLinkProperties *linkProperties = [[LMLinkProperties alloc] init];
    linkProperties.channel = @"";//渠道(微信,微博,QQ,等...)
    linkProperties.feature = @"Share";//特点
    linkProperties.tags=@[@"LinkedME",@"Demo"];//标签
    linkProperties.stage = @"Live";//阶段
    [linkProperties addControlParam:@"View" withValue:arr[page][@"url"]];//页面唯一标识
    [linkProperties addControlParam:@"LinkedME" withValue:@"Demo"];//Demo标识  
    //开始请求短链
    [self.linkedUniversalObject getShortUrlWithLinkProperties:linkProperties andCallback:^(NSString *url, NSError *err) {
        if (url) {
            NSLog(@"[LinkedME Info] SDK creates the url is:%@", url);
            //拼接连接
            [H5_LIVE_URL stringByAppendingString:arr[page][@"form"]];
            [H5_LIVE_URL stringByAppendingString:@"?linkedme="];
            H5_LIVE_URL = [NSString stringWithFormat:@"https://www.linkedme.cc/h5/%@?linkedme=",arr[page][@"form"]];
            //前面是Html5页面,后面拼上深度链接https://xxxxx.xxx (html5 页面地址) ?linkedme=(深度链接)
            //https://www.linkedme.cc/h5/feature?linkedme=https://lkme.cc/AfC/mj9H87tk7
            LINKEDME_SHORT_URL = [H5_LIVE_URL stringByAppendingString:url];            
        } else {
            LINKEDME_SHORT_URL = H5_LIVE_URL;
        }
    }];
}
```
