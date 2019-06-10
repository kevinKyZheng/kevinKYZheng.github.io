---
title: iOS动画
date: 2019-06-01 16:37:13
tags: 动画
categories: iOS
---
## UIView动画

包括普通动画,弹性动画,key动画和Transition,

```swift
//弹性动画
UIView.animate(withDuration: TimeInterval, delay: TimeInterval, usingSpringWithDamping: CGFloat, initialSpringVelocity: CGFloat, options: UIView.AnimationOptions, animations: {}) { (Bool) in}
```

### Transitions动画

添加或者删除view时的动画,也属于UIView动画,是UIView动画中3D的唯一实现方式

```swift
//添加,删除view,隐藏或显示View
UIView.transition(with:)
//替换view
UIView.transition(from:)
```

几种动画属性

* transitionFlipFromLeft
* transitionFlipFromRight
* transitionCurlUp
* transitionCurlDown
* transitionCrossDissolve
* transitionFlipFromTop
* transitionFlipFromBottom

### Constrains动画

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

## Layer动画

1. 简述

Layer有预定义的可视化特点,即一堆属性来决定内容怎么显示;
并且CoreAnimation可以优化layer内容的缓存并且是在GPU中完成绘制,更快;

2. Layer的一些基本属性

* Boarder
* Shaodw
* Content:contents,mask,opacity

3. 基本使用

```swift
let ani = CABasicAnimation(keyPath: "position.x")
ani.fromValue = 50
ani.toValue = 300
ani.duration = 1
ani.fillMode = .both
ani.isRemovedOnCompletion = false
newView.layer.add(ani, forKey: nil)
```

4. CAAnimation有两个代理方法

```swift
func animationDidStart(_ anim: CAAnimation)
func animationDidStop(_ anim: CAAnimation, finished flag: Bool)
```

创建animation的时候,用`ani.setValue("form", forKey: "name")`,设置key可以在代理方法中,取出对应值

5. Groups

* 对多个动画进行同步执行,是CAAnimation的子类,所以属性都可以通用

* 用法:设置基本属性后,往animations添加动画

```swift
let group = CAAnimationGroup()
group.beginTime = CACurrentMediaTime() + 0.5
group.duration = 0.5
group.animations = [scaleDown,rotate,fade]
```

* 常用属性

```swift
repeatCount//执行次数
repeatDuration//反复执行的时间
autoreverses//倒着执行
```

### 动画和实际内容

动画一个view的时候,真实的view是被隐藏了的,被动画的是一个复制的view;当动画完成时,原来的view显示,执行动画的view隐藏.因此`isRemovedOnCompletion`属性就是控制执行动画的view是否隐藏,默认是true隐藏.但是尽量不要使用这个属性.

### CASpringAnimation

layer层的弹性动画,设置方法基本一样如下,有一些特殊属性值

```swift
let flash = CASpringAnimation(keyPath: "borderColor")
flash.damping = 7.0
flash.stiffness = 200.0
flash.fromValue = UIColor(red: 1.0, green: 0.27, blue: 0.0, alpha: 1.0).cgColor
flash.toValue = UIColor.white.cgColor
flash.duration = flash.settlingDuration
blue.layer.add(flash, forKey: nil)
```

#### 处理结构体的属性

在swift中结构体和class没啥区别,但是coreAnimation是建立在OC的基础上的,在OC中,结构体很不同,要包裹起来,例如CGPoint,CGRect,CATransform3D

## 特殊的图层动画

1. GradientLayer
渐变层:可以控的属性:locations,colors,

## 转场动画

1. 要过来的控制器实现代理

```swift
vc.transitioningDelegate = self
```

2. 两个代理方法,一个是要被转入的时候,一个是被推出的时候;
返回的是一个实现了`UIViewControllerAnimatedTransitioning`协议的对象

```swift
public func animationController(forPresented presented: UIViewController, presenting: UIViewController, source:
UIViewController) -> UIViewControllerAnimatedTransitioning

func animationController(forDismissed dismissed: UIViewController) -> UIViewControllerAnimatedTransitioning? 
```

3. `UIViewControllerAnimatedTransitioning`协议中的方法,transition开始的时候,当下的view加到一个container中,新view被创建但是不显示

```swift
//过场时间
transitionDuration
//过场时的动作
animateTransition{
let containerView = transitionContext.containerView
let toView = transitionContext.view(forKey: .to)!
}
```