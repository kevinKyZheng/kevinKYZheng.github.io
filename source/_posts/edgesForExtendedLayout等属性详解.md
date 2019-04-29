# edgesForExtendedLayout,contentInsetAdjustmentBehavior,automaticallyAdjustsScrollViewInset属性总结

1. edgesForExtendedLayout

此属性是指定view的边延伸的位置,默认值是All
```
typedef enum : NSUInteger {
   UIRectEdgeNone   = 0,
   UIRectEdgeTop    = 1 << 0,
   UIRectEdgeLeft   = 1 << 1,
   UIRectEdgeBottom = 1 << 2,
   UIRectEdgeRight  = 1 << 3,
   UIRectEdgeAll = UIRectEdgeTop | UIRectEdgeLeft | UIRectEdgeBottom | UIRectEdgeRight 
} UIRectEdge;
```
注意:它只有当viewController被嵌到别的container view controller中时才会起作用

2. automaticallyAdjustsScrollViewInsets

* table从navigationBar底部开始并且滚动的时候能延伸到整个屏幕
* 适用于view是ScrollView或其子View
* 无论是true或者false,table滚动都是覆盖整个屏幕,区别是否会被导航条挡住

3. extendedLayoutIncludesOpaqueBars

对前两个的补充,navigationBar不透明时是否依然延伸
edgesForExtendedLayout是UIRectEdgeAll,并且此属性为false,就不会延伸到不透明的Bar