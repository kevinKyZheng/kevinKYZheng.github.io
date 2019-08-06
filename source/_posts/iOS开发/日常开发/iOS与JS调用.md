---
title: iOS与JS调用
date: 2019-06-12 14:53:04
tags: 面试 
categories: iOS
---
## h5中调用原生

h5中

```js
webkit.messageHandlers.jsToSwift.postMessage('Hello World');
webkit.messageHandlers.jsToSwift2.postMessage({key: 'Hello', value: 'World'});
```

原生中

1. 初始化

    ```swift
    let config = WKWebViewConfiguration()
    let wkCotnroller = WKUserContentController()
    config.userContentController = wkCotnroller
    webView = WKWebView.init(frame: CGRect(x: 0, y: 0, width: SCREEN_WIDTH, height: SCREEN_HEIGHT), configuration: config)

    webView.configuration.userContentController.add(self, name: "jsToSwift")
    ```

2. 代理方法,实现`WKScriptMessageHandler`这个协议

```swift
func userContentController(_ userContentController: WKUserContentController, didReceive message: WKScriptMessage) {

    print(message.name)
    print(message.body)

    if let dic = message.body as? Dictionary<String,String>{
        let key = dic["key"]
        let value = dic["value"]
        print(key)
        print(value)

    }
}
```

## 原生调h5

```swift
webView.evaluateJavaScript("document.title='111111'") { (any, error) in
}
```

## 常用的代理方法

<https://www.cnblogs.com/zhanggui/p/6626483.html>
