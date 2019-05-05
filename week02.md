## Algorithm
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）

    <code>
    public static int maxProfit(int[] prices) {
        int total = 0;
        if (prices == null || prices.length <= 1) {
            return total;
        }

        int j = 1;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < prices[j]) {
                for (; j < prices.length - 1; j++) {
                    if (prices[j + 1] <= prices[j]) {
                        break;
                    }
                }
                total += prices[j] - prices[i];
                i = j;
                j += 2;
            } else {
                j++;
            }

            if (j >= prices.length) {
                break;
            }
        }
        return total;
    }
    </code>

## Review
阅读文章为[WebViews in Flutter](https://blog.geekyants.com/webviews-in-flutter-87194714ce3d)

Flutter是Google开源的跨平台开发移动端产品技术，但是Google爸爸没有使用Dart实现WebView。如果需要通过网页呈现内容，则需要通过WebView Plugin实现，最终iOS端通过WKWebView实现，Android通过[WebView](https://developer.android.com/reference/android/webkit/WebView)实现。
文章通过清晰的步骤说明Flutter接入基础WebView的方法和过程，主要包括如下步骤：

 > 1. 创建App，完成首页创建并添加触发WebView加载的按钮及其事件
 > 2. 通过路由方式打开WebView页面加载URL
 > 3. 简洁清晰的代码
 > 4. iOS接入时的注意事项，需要在Info.plist中配置io.flutter.embedded_views_preview为YES

非常棒的一篇Flutter WebView入门文章
由于Flutter未实现原生的WebView，通过Plugin实现的方式在现阶段有很多问题(哭笑脸)，一步一个坑呀，必要时候需要自己开发相关功能

## Tip
为保证用户数据和设备的安全，Google针对Android P(9.0) 及更高版本的应用程序，将要求默认使用[加密连接](https://developer.android.com/about/versions/pie/android-9.0-changes-28)。这意味着 Android P将禁止使用所有未加密的连接。因此在Android P使用HttpUrlConnection进行http请求会出现以下异常
 > W/System.err: java.io.IOException: Cleartext HTTP traffic to **** not permitted

使用OKHttp请求则出现
 > java.net.UnknownServiceException: CLEARTEXT communication ** not permitted by network security policy

在Android P及更高版本系统的设备上，如果应用使用http网络请求，则会导致该应用无法进行网络请求，需要使用https进行网络请求。如应用使用了WebView也同样受此影响。
针对这个问题，有以下三种解决方法：

 > App接口请求，WebView内容请求更新使用Https方式
 > 将targetSdkVersion降到27以下
 > 更改网络安全配置
 
个人认为第一种方式最好，推荐使用。但是如果暂时没有办法实现，可以通过后面两种方式实现使用http请求。
降低targetSdkVersion可能会影响应用市场审核获取引出其他适配性问题，不推荐使用。
可以使用如下方式，通过配置网络安全文件，实现App使用Http请求，用于在切换Https前的时间缓冲。

    <code>
    <?xml version="1.0" encoding="utf-8"?>
    <network-security-config>
        <base-config cleartextTrafficPermitted="true" />
    </network-security-config>
    </code>

    <code>
    <?xml version="1.0" encoding="utf-8"?>
    <manifest ... >
        <application android:networkSecurityConfig="@xml/network_security_config"
            ... >
            ...
        </application>
    </manifest>
    </code>

## Share
[全面剖析Android消息机制源码](https://juejin.im/post/5cb43a8de51d456e7b372059?utm_source=gold_browser_extension)，不是仅介绍Handler、Message、Looper，让我们更深入一点的了解
