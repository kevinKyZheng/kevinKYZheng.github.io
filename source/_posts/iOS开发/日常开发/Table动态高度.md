---
title: Table相关
date: 2019-06-19 10:45:14
tags:
categories:
---
## 动态高度

```swift
    tableView.addObserver(self, forKeyPath: "contentSize", options: [], context: nil)

    override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
        let height = self.tableView.contentSize.height
        tableView.snp.updateConstraints { (make) in
            make.height.equalTo(height)///
        }
    }
    deinit {
        tableView.removeObserver(self, forKeyPath: "contentSize")
    }
```

## table单选

在table处

```swift
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {

        tableView.selectRow(at: indexPath, animated: true, scrollPosition: .none)
    }
```

在cell处

```swift
    override func setSelected(_ selected: Bool, animated: Bool) {

    }
```

