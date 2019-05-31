1. UIVisualEffectView
使用:

```
// 1
view.backgroundColor = .clear
// 2
let blurEffect = UIBlurEffect(style: .light)
// 3
let blurView = UIVisualEffectView(effect: blurEffect)
// 4
blurView.translatesAutoresizingMaskIntoConstraints = false
view.insertSubview(blurView, at: 0)
```

2. 运用了反射

通知中心,收到屏幕旋转的通知.用NSClassFromString获得Application

3. intrinsicContentSize
   约束优先级／content Hugging／content Compression Resistance