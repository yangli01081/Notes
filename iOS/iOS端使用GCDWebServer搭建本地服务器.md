## 一. 下载集成GCDWebServer


地址：[https://github.com/swisspol/GCDWebServer](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fswisspol%2FGCDWebServer)

将GCDWebServer文件夹拉进项目中即可。

## 二. 使用


基本的搭建跟使用源作者写的很清楚了，可以自己参考下。下面是我自己在实际使用中遇到的需要注意的问题，贴上代码跟说明：

#### 1.搭建本地服务器


```swift
#import "LocalServerTestController.h"
#import "GCDWebServer.h"
#import "GCDWebServerDataResponse.h"
#import "GCDWebServerURLEncodedFormRequest.h"
@interface LocalServerTestController ()<GCDWebServerDelegate>
{
    GCDWebServer *_localServer;
}
@end

@implementation LocalServerTestController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    [self startLocalServer];
}


- (void)startLocalServer{
    _localServer = [[GCDWebServer alloc] init];
    _localServer.delegate = self;
    //设置监听
    [_localServer addHandlerForMethod:@"OPTIONS"      //跨域请求会先发起该类型的请求
                            pathRegex:@"^/"           //接口正则匹配
                         requestClass:[GCDWebServerURLEncodedFormRequest class]
                         processBlock:^GCDWebServerResponse *(GCDWebServerRequest* request) {
                             GCDWebServerDataResponse *response;
                             NSLog(@"request %@",request);
                             //对请求来源做合法性判断
//                             NSString *origin = [request.headers objectForKey:@"Origin"];
//                             NSString *referer = [request.headers objectForKey:@"Referer"];
//
//                             if ([origin containsString:@"xxxxxxx"] && [referer containsString:@"yyyyyy"]) {//合法规则跟后台协商
//                                 //请求合法，继续其他操作
//                             }else{
//                                 //请求不合法，返回对应的状态
//                                 response = [GCDWebServerDataResponse responseWithText:@"failure"];
//                             }

                             //响应
                             response = [GCDWebServerDataResponse responseWithStatusCode:200];
                             //响应头设置，跨域请求需要设置，只允许设置的域名或者ip才能跨域访问本接口）
                             [response setValue:@"*" forAdditionalHeader:@"Access-Control-Allow-Origin"];
                             [response setValue:@"Content-Type" forAdditionalHeader:@"Access-Control-Allow-Headers"];
                             //设置options的实效性（我设置了12个小时=43200秒）
                             [response setValue:@"43200" forAdditionalHeader:@"Access-Control-max-age"];
                             return response;
                         }];
    [_localServer addHandlerForMethod:@"POST"
                            path:@"/test"                        //接口名
                         requestClass:[GCDWebServerURLEncodedFormRequest class]
                         processBlock:^GCDWebServerResponse *(GCDWebServerRequest* request) {
                             GCDWebServerDataResponse *response;
                             //对请求来源做合法性判断
//                             NSString *origin = [request.headers objectForKey:@"Origin"];
//                             NSString *referer = [request.headers objectForKey:@"Referer"];
//
//                             if ([origin containsString:@"xxxxxxx"] && [referer containsString:@"yyyyyy"]) {//合法规则跟后台协商
//                                 //请求合法，继续其他操作
//                             }else{
//                                 //请求不合法，返回对应的状态
//                                 response = [GCDWebServerDataResponse responseWithText:@"failure"];
//                             }
                             //获取请求中的参数（body）
                             NSJSONSerialization *json = [NSJSONSerialization JSONObjectWithData:[(GCDWebServerURLEncodedFormRequest*)request data] options:NSJSONReadingAllowFragments error:nil];
                             NSLog(@"请求参数 %@",json);
                             
                             response = [GCDWebServerDataResponse responseWithText:@"success"];
                             //响应头设置，跨域请求需要设置，只允许设置的域名或者ip才能跨域访问本接口）
                             [response setValue:@"*" forAdditionalHeader:@"Access-Control-Allow-Origin"];
                             
                             [response setValue:@"Content-Type" forAdditionalHeader:@"Access-Control-Allow-Headers"];
                             
                             return response;
                         }];
    
    
    //设置监听端口
    [_localServer startWithPort:5555 bonjourName:nil];
    NSLog(@"Visit %@ in your web browser", _localServer.serverURL);
}

- (void)webServerDidStart:(GCDWebServer *)server{
    NSLog(@"本地服务启动成功");
}

@end
```


#### 2.跨域说明（源使用文档没有具体提到的）


实际使用过程中会涉及到跨域请求：比如某个web，该web上的控件需要通过Http方式与你的App交互，web发起请求访问APP本地的接口就涉及到跨域请求。这时你会先收到一个OPTIONS类型的请求，只有你在APP本地的端口响应了该请求并设置了允许跨域请求后才能够监听到具体的接口访问。

#### 3.代码说明


以我上面的使用为例：web发起一个post请求访问APP本地的@"/test"接口，这时需要先监听OPTIONS请求，这里使用pathRegex正则匹配接口（其他像GET和POST的监听采用path就行，填具体接口名），接口设置为@"^/"可以监听所有收到的跨域请求发出的OPTIONS，这样设置就不用对每个接口的跨域请求都写一次OPTIONS的监听。然后在OPTIONS的监听里设置响应头，Access-Control-Allow-Origin设置允许哪些域名或者ip可以跨域访问你本地的接口，设置为*代表允许任何来源的跨域访问。Access-Control-max-age设置OPTIONS的有效期，在有效期内，web跨域访问其他本地接口都不会再发起OPTIONS的请求。在OPTIONS内设置了允许跨域后，就可以正常监听到web访问了本地的@"/test"接口。

#### 4.其他说明

- 在OPTIONS中设置完跨域权限后，同样需要在接口的监听中再次设置，否则也会收不到请求。
- 非跨域访问不需要监听OPTIONS，比如浏览器直接填测试手机的ip+端口+接口名就能正常监听访问。(源文档里面的demo就是非跨域访问的)
- 该库不支持HTTPS和长链接。
- 该库能够满足基本的本地服务器功能，写接口-监听接口请求-获取请求参数-响应返回各种数据类型，个人觉得使用起来还挺简单方便的。

作者：拂晓拂晓

链接：[https://www.jianshu.com/p/e167bff92633](https://www.jianshu.com/p/e167bff92633)

