# SearchController的使用详解及问题总结

1. 初始化,用展现结果的控制器,这里用 searchResultVc

搜索框所在位置的界面
> searchController.swift
```
       let searchController = UISearchController(searchResultsController: searchResultVc)
       //更新结果显示的位置,需要实现UISearchResultsUpdating协议
       searchController.searchResultsUpdater = self.searchResultVc
        //搜索框激活时,添加透明视图,
       searchController.dimsBackgroundDuringPresentation = false
       //下文注解
       self.definesPresentationContext = true
       //搜索结果控制器中的导航条和搜索框,使用同搜索控制器
        searchResultVc.searchBar = self.searchController.searchBar
        searchResultVc.nav = self.navigationController
```
显示搜索结果的界面
> searchResultController.swift
```
extension WTSearchResultController:UISearchResultsUpdating{
    func updateSearchResults(for searchController: UISearchController) {
        //搜索框激活时,显示resultcontroller的界面
        searchController.searchResultsController!.view.isHidden = false        
    }
}
```

* 一般情况,searchResultsUpdater和dimsBackgroundDuringPresentation配合使用:
      * 如果更新结果的位置是当前界面,则在协议实现中,清空当前界面,然后可以选择是否添加透明视图,
      并且,在一个控制器显示,需要用到isActive属性,判断搜索框是否激活,进而选择不同的数据源;
      * 如果更新结果的位置是另一个controller,,则在协议中,显示controller的view,此时添加透明视图无效;

* definesPresentationContext属性注解
      搜索框展开的过程,相当于是searchbar触发了"presentViewController"的过程,而modalPresentationStyle的动画方式有几种效果,searchController能使用的有**OverCurrentContext**、**Popover**,默认是**OverCurrentContext**,而这个方式presentController时,会向上寻找一个definesPresentationContext为真的控制器,并在上面显示.所以此处应该设置为真.


2. searchBar的样式修改

  * 搜索框整体背景颜色
  ```
        searchController.searchBar.setBackgroundImage(imageFromColor(color: COLOR_BG), for: .any, barMetrics: .default)
  ```

  * 搜索条背景颜色,边框效果,字体等
  ```
        let searchField = searchController.searchBar.value(forKey: "_searchField") as! UITextField
        searchField.borderStyle = .none
        searchField.backgroundColor = .white
        searchField.layer.cornerRadius = 4
        searchField.layer.masksToBounds = true
        searchField.font = FONT15
  ```

  * 位置居中 
  ```
        searchController.searchBar.setPositionAdjustment(UIOffsetMake((SCREEN_WIDTH - width - 30-5)/2, 0), for: UISearchBarIcon.search)
  ```

  * 调整图标与文字的距离
  ```
        searchController.searchBar.searchTextPositionAdjustment = UIOffsetMake(5, -1)
  ```

  * placeHoler居中显示,输入时左对齐显示:
  在searchbar的代理方法,开始编辑时将位置复原,并且在点击取消的时候将位置居中

  ```
    func searchBarShouldBeginEditing(_ searchBar: UISearchBar) -> Bool {
        searchBar.setPositionAdjustment(UIOffsetMake(0, 0), for: UISearchBarIcon.search)
        return true
    }
    func searchBarCancelButtonClicked(_ searchBar: UISearchBar) {
        let width = searchBar.placeholder!.widthWithFont(font: FONT15)
        searchBar.setPositionAdjustment(UIOffsetMake((SCREEN_WIDTH - width - 30-5)/2, 0), for: UISearchBarIcon.search)
    }
  ```
  3. 注意事项:

  * UISearchController 在使用的时候, 需要设置为全局的变量或者控制器属性, 使其生命周期与控制器相同; 如果设置为局部变量, 会提前销毁, 导致无法使用.

  4. 爬坑

      1. 搜索框激活时,向上偏移了状态栏高度的距离.
      解决: 在searchController所在的控制器,添加如下属性.原因: 待续
      ```
        self.edgesForExtendedLayout =  UIRectEdge(rawValue: UIRectEdge.left.rawValue | UIRectEdge.bottom.rawValue | UIRectEdge.right.rawValue)
      ``` 

