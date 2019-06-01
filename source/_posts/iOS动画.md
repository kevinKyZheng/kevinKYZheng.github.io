---
title: iOS动画
date: 2019-06-01 16:37:13
tags: 动画
categories: iOS
---

## Constrains动画

* 与普通的frame动画相同,只不过操作的是contrain.

* 注意点:在动画之前需要调用`view.layoutIfNeeded()`,确保子控件已经完成布局,在动画回调中,也要调用,刷新布局.

* 原理:在动画closure里面修改约束属性的时候,AutoLayour会在计算完之后进行设置.

为什么要调用`layoutIfNeeded()`
>If you hadn’t called layoutIfNeeded(), UIKit would have performed a layout anyway since you changed a constraint, which marked the layout as dirty.


> In this case, you’ve already updated the constraint value, but iOS hasn’t had a chance to update the layout yet. By calling layoutIfNeeded() from within the animation closure, you set the center and bounds of every view involved in the layout. That’s it — there’s no magic happening in the background!

```swift
self.view.layoutIfNeeded()
UIView.animate(withDuration: 1.0, delay: 0.0, usingSpringWithDamping: 0.4, initialSpringVelocity: 10.0, options: .curveEaseIn, animations: {
    conBootom.constant = -imageView.frame.size.height / 2
    conWidth.constant = 0.0
    self.view.layoutIfNeeded()
}, completion: nil)
```
