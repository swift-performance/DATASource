# DATASource

Using `NSFetchedResultsController` and `NSFetchedResultsControllerDelegate` is awesome, but sadly it involves a lot of boilerplate. Well, luckily with DATASource not anymore.

- [x] Encapsulates NSFetchedResultsController and NSFetchedResultsControllerDelegate boilerplate
- [x] Supports indexed tables out of the box
- [x] Supports sectioned collections out of the box
- [x] Swift 2.0
- [x] Objective-C compatibility

## UITableView

### Basic Usage

Hooking up your table view to your `Task` model and making your UITableView react to insertions, updates and deletions is as simple as this.

**Swift:**
```swift
let request: NSFetchRequest = NSFetchRequest(entityName: "Task")
request.sortDescriptors = [NSSortDescriptor(key: "title", ascending: true)]

let dataSource = DATASource(tableView: self.tableView, cellIdentifier: "Cell", fetchRequest: request, mainContext: self.dataStack.mainContext, configuration: { cell, item, indexPath in
    cell.textLabel?.text = item.valueForKey("title") as? String
})

tableView.dataSource = dataSource;
```

**Objective-C:**
```objc
NSFetchRequest *request = [[NSFetchRequest alloc] initWithEntityName:@"Task"];
request.sortDescriptors = @[[NSSortDescriptor sortDescriptorWithKey:@"title" ascending:YES]];

DATASource *dataSource = [[DATASource alloc] initWithTableView:self.tableView
                                                cellIdentifier:@"Cell"
                                                  fetchRequest:request
                                                   mainContext:self.dataStack.mainContext
                                                   sectionName:nil
                                                 configuration:^(UITableViewCell * _Nonnull cell, NSManagedObject * _Nonnull item, NSIndexPath * _Nonnull indexPath) {
                                                     cell.textLabel.text = [item valueForKey:@"name"];
                                                 }];

self.tableView.dataSource = dataSource;
```

### Indexed UITableView

`DATASource` provides an easy way to show an indexed UITableView, you just need to specify the attribute we should use to group your items. This attribute is located in the `dataSource` initializer as a parameter called `sectionName`.

Check the [Swift ](https://github.com/3lvis/DATASource/blob/master/TableSwift/ViewController.swift) for an example of this, were we have an indexed UITableView of names, just like the Contacts.app!

<p align="center">
  <img src="https://raw.githubusercontent.com/3lvis/DATASource/master/GitHub/table.gif" />
</p>

### UITableViewDataSource

`DATASource` takes ownership of your `UITableViewDataSource` providing boilerplate functionality for the most common tasks, but if you need to override any of the `UITableViewDataSource` methods you can use the `DATASourceDelegate`.

## UICollectionView

### Basic Usage

Hooking up a UICollectionView is as simple as doing it with a UITableView, just use this method.

**Swift**:
```swift
let request: NSFetchRequest = NSFetchRequest(entityName: "Task")
request.sortDescriptors = [NSSortDescriptor(key: "title", ascending: true)]

let dataSource = DATASource(collectionView: self.collectionView, cellIdentifier: "Cell", fetchRequest: request, mainContext: self.dataStack.mainContext, configuration: { cell, item, indexPath in
    cell.textLabel.text = item.valueForKey("title") as? String
})

collectionView.dataSource = dataSource
```

**Objective-C:**
```objc
NSFetchRequest *request = [[NSFetchRequest alloc] initWithEntityName:@"Task"];
request.sortDescriptors = @[[NSSortDescriptor sortDescriptorWithKey:@"title" ascending:YES]];

DATASource *dataSource = [[DATASource alloc] initWithCollectionView:self.collectionView
                                                     cellIdentifier:CollectionCellIdentifier
                                                       fetchRequest:request
                                                        mainContext:self.dataStack.mainContext
                                                        sectionName:nil
                                                      configuration:^(UICollectionViewCell * _Nonnull cell, NSManagedObject * _Nonnull item, NSIndexPath * _Nonnull indexPath) {
                                                          CollectionCell *collectionCell = (CollectionCell *)cell;
                                                          [collectionCell updateWithText:[item valueForKey:@"name"]];
                                                      }];

self.collectionView.dataSource = dataSource;
```

### Indexed UITableView

`DATASource` provides an easy way to show an grouped UICollectionView, you just need to specify the attribute we should use to group your items. This attribute is located in the `dataSource` initializer as a parameter called `sectionName`. This will create a collectionView reusable header.

Check the [CollectionView Demo](https://github.com/3lvis/DATASource/tree/master/CollectionSwift) for an example of this, were we have a grouped UICollectionView using the first letter of a name as a header, just like the Contacts.app!

<p align="center">
  <img src="https://raw.githubusercontent.com/3lvis/DATASource/master/GitHub/collection.gif" />
</p>

### UICollectionViewDataSource

`DATASource` takes ownership of your `UICollectionViewDataSource` providing boilerplate functionality for the most common tasks, but if you need to override any of the `UICollectionViewDataSource` methods you can use the `DATASourceDelegate`. Check the [CollectionView Demo](https://github.com/3lvis/DATASource/tree/master/CollectionViewSwift) where we show how to add a footer view to your `DATASource` backed UICollectionView.

## Installation

**DATASource** is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'DATASource'
```

## Author

Elvis Nuñez, [@3lvis](https://twitter.com/3lvis)

## License

**DATASource** is available under the MIT license. See the LICENSE file for more info.
