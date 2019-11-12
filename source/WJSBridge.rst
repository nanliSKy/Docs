WJSBridge 方便 Javascript 与 OC 之前相互调用、传值、回调。

使用 pod ‘WJSBridge’

JavaScript 与 iOS 交互 iOS 使用 UIWebview 加载 H5 JavaScript调用 iOS
方法 walletBridge(method, params, callBack) 举例说明:JavaScript
调用objective-C(OC)方法

1.普通方法

walletBridge(‘download’) OC实现方法

-(void)download{} 2.带参数方法

walletBridge(‘share’, { title:‘title’, context: ‘context’, url: ‘url’ })
OC实现方法

-  (void)share:(NSDictionary \*)param {} 3.带参数及回调函数

walletBridge(“login”, { userName:‘nanli’, password:‘qwe’ },
function(data){ console.log(‘login success:’+ data); }) OC 实现方法

-  (void)login:(NSDictionary \*)param {} 回调函数

[self.jsBridge executCallBack:@[@“a little word”]]; OC 调用 JavaScript
方法 - (void)executJs:(NSString *)method param:(NSArray*)param; iOS使用
WKWebView 加载H5 JavaScript 调用 iOS方法 WJSBridge.walletCall(method,
param, callBack) 1.普通方法

WJSBridge.walletCall(‘download’); OC 实现方法

-(void)download{} 2.带参数的方法

WJSBridge.walletCall(‘share’, { title:‘title’, context: ‘context’, url:
‘url’ }) OC 实现方法

-  (void)share:(NSDictionary \*)param {} 3.带参数及回调函数

WJSBridge.walletCall(“login”, { userName:‘nanli’, password:‘qwe’ },
function(data){ console.log(‘login success:’+ data); }) OC 实现方法

-  (void)login:(NSDictionary \*)param :(walletCallBlock)callBlock; OC
   调用 JavaScript方法
-  (void)executJs:(NSString *)method params:(NSString*)params
   completed:(void(^)(id data, NSError \*error))completed; 核心类
   WJSBridge @interface WJSBridge : NSObject

+(instancetype)regist:(UIWebView *)webView target:(NSObject*)target;

/*\* native 主动调用 JS

@param method JS 方法 @param param native 传给 JS 参数 */ -
(void)executJs:(NSString*)method param:(NSArray \*)param;

/*\* 回调, native 调用 JS

@param param native 传给 JS 参数 */ -
(void)executCallBack:(NSArray*)param;

@end WJSBridgeHandler typedef void(^walletCallBlock) (id response);

@interface WJSBridgeHandler : NSObject /*\* */ @property (nonatomic,
strong) WKWebView*\ webView;

-  (instancetype)regist:(WKWebViewConfiguration *)config
   target:(NSObject*)target;

/*\* native 调用 JS 方法

@param method JS响应的方法名 @param params JS接收参数 @param completed
返回结果 */ - (void)executJs:(NSString*)method params:(NSString *)params
completed:(void(^)(id data, NSError*\ error))completed;; @end
WJSBridge.js var WJSBridge = { walletCall: function(method, params,
callBack) { var message; if (!callBack) { message = {‘method’: method,
‘params’: params};
window.webkit.messageHandlers.WJSBridge.postMessage(message); }else {
var callBackId = method+‘callBack’; message = {‘method’: method,
‘params’: params, ‘callBack’: callBackId}; if
(!WJSBridgeEvent._listeners[callBackId]) {
WJSBridgeEvent.addEvent(callBackId, function (data){ callBack(data); })
} window.webkit.messageHandlers.WJSBridge.postMessage(message); }