## 普通table
1. 数据源,类型

```
BehaviorSubject(value: [RepairListModel]())

var sectionDataSouce = BehaviorSubject(value: [SectionModel<String,NoticeListModel>]())

```

2. 配置数据源

```
let collectionsDatasource = RxTableViewSectionedReloadDataSource<SectionModel<String,NoticeListModel>> (configureCell:{ (dataSource, tableView, indexPath, item) -> UITableViewCell in
    
    let cell = tableView.dequeueReusableCell(withIdentifier: NoticeListCell.cellIdentifier, for: indexPath) as! NoticeListCell
    return cell
})
```

3. 绑定数据源
```
sectionDataSouce.asObserver().bind(to: tableView.rx.items(dataSource: collectionsDatasource)).disposed(by: disposeBag)
```

4. 点击事件(同时监听点击位置和数据)

```
        Observable.zip(tableView.rx.itemSelected,tableView.rx.modelSelected(NoticeListModel.self)).subscribe(onNext: {[weak self] (indexPath,model) in
            
        }).disposed(by: disposeBag)
```

## 带动画的table

```
let source = RxTableViewSectionedAnimatedDataSource<AnimatableSectionModel<String,RepairListModel>>(
            //设置插入、删除、移动单元格的动画效果
            animationConfiguration: AnimationConfiguration(insertAnimation: .top,
                                                           reloadAnimation: .fade,
                                                           deleteAnimation: .left),
            configureCell: {
                (data, tableView, indexPath, model) in
                return self.configCell(data, tableView, indexPath, model)
        },
            canEditRowAtIndexPath: { _, _ in
                return true //单元格可删除
        }
        )
        let refreshCommand = dataArray.asObservable().map(TableEditingCommand.setItems)
        let initialVM = TableViewModel()
        
        //绑定单元格数据
        Observable.of(refreshCommand,delete)
            .merge()
            .scan(initialVM) { (vm: TableViewModel, command: TableEditingCommand)
                -> TableViewModel in
                return vm.execute(command: command)
            }
            .startWith(initialVM)
            .map {
                [AnimatableSectionModel<String, RepairListModel>(model: "", items: $0.items)]
            }
            .share(replay: 1)
            .bind(to: self.tableView.rx.items(dataSource: source))
            .disposed(by: disposeBag)
```