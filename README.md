# Latipay支付服务
## 主要功能
1. 通过LatipaySDK唤起微信支付和支付宝支付.

## 使用方法
1 将整个文件夹拖到你的项目中,包括AlipaySDK和LatipaySDK.(注意用group形式,目录会显示成黄色).

   ![image](images/CCOpenService_Tree.png)

2 添加AlipaySDK所依赖的动态库

   ![image](images/zhifubao.png)

3 在AppDelegate.m文件顶部引入头文件Latipay.h,并且写入下面的配置信息
``` objectivec
//AppDelegate.m

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    //配置开发环境，默认为LatipayDebug模式，上线请改成LatipayRelease模式
    [[LatipaySDK shareInstance]setEnvironment:LatipayDebug];
    
    return YES;
}

```

4 支付接口使用说明:
``` objectivec

/**
Latipay request

@param order 订单信息
@param schemeStr 设置url schemes，以供支付宝返回到app
@param completionBlock 支付结果回调
*/
- (void)payOrder:(LatipayOrder *)order fromScheme:(NSString *)schemeStr callback:(CompletionResultBlock)completionBlock;

##设置url schemes
   ![image](images/urlschemes.png)

## 支付实例：

- (IBAction)pay:(id)sender {
    LatipayOrder *order = [LatipayOrder new];
    order.amount = @"0.01";
    order.api_key = @"y64ckNrSd5";
    order.callback_url = @"https://api.latipay.net/confirmation";
    order.ip = @"127.0.0.1";
    order.merchant_reference = @"W000000037";
    order.payment_method = @"alipay";
    order.product_name = @"W000000037";
    order.return_url = @"https://api.latipay.net/confirmation";
    order.signature = @"aef492d15e6e5f52f278f0be39d32a3a0b0e897388ce45b3e0df3e4561a7c1a8";
    order.user_id = @"U000000051";
    order.wallet_id = @"W000000037";
    order.source = @"ios";

    [[LatipaySDK shareInstance] payOrder:order fromScheme:@"latipayDemo" callback:^(NSDictionary *resultDic, NSError *error) {

    }];
}
```

5 设置支付宝支付结果回调 

``` objective

// NOTE: 9.0之前
- (BOOL)application:(UIApplication *)application
openURL:(NSURL *)url
sourceApplication:(NSString *)sourceApplication
annotation:(id)annotation {

    // 支付跳转支付宝钱包进行支付，处理支付结果
    [[LatipaySDK shareInstance]processOrderWithLatipayResult:url standbyCallback:^(NSDictionary *resultDic, NSError *error) {

    }];

    return YES;
}

// NOTE: 9.0以后使用新API接口
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options
{
    // 支付跳转支付宝钱包进行支付，处理支付结果
    [[LatipaySDK shareInstance]processOrderWithLatipayResult:url standbyCallback:^(NSDictionary *resultDic, NSError *error) {

    }];
    return YES;
}

```
6 error Code 说明
