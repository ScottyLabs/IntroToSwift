---
title: Introduction to iOS Development
layout: page
---

## iOS Basics

When iOS 2 first allowed developers to add third party apps to be added to the app store, it completely changed the way people interacted with their phones. While apps seem commonplace these days, they remain one of the most effective and powerful ways to interact with users, and iOS is the best platform to build them for (no really).

## Application Structure

### Model-View-Controller (MVC)

MVC is the primary design pattern employed for iOS development. It divides the application into three parts:

  1. Model - The actual data and information stored in the app. Could be Tasks, Game State, Restaurant Ratings, etc. These classes may have no superclass, or simply a superclass of NSObject. If they need to be persistent, they usually are part of CoreData.
  2. View - What the user sees. View objects are usually subclasses of UIView, and should _only_ have methods that modify visual appearance. Try to use Storyboard to edit your views.
  3. Controller - This is where all of the logic happens. The controller reads and writes data to the model, performs logical tasks, and updates the Views when necessary. Controllers are usually subclasses of UIViewController, and are where you will do the meat of your coding.

### Delegation

Another design pattern common in iOS is delegation. One common use for delegation is for View elements, which are responsible for recieving user input, want to tell a Controller that their state has changed. This helps keep tasks separate, as suggested by MVC, by allowing the Controller to manage computation, even when it is based off a view event.

`Delegate` is a commone postfix on `procotol` objects. We will examine protocols later, but essentially this means that a controller must provide a specific interface in order to act as a view delegate. 

### UIKit

UIKit is Apple's developer library for iOS technology. _Always_ use it where possible. One great thing about developing for iOS is the number of reliable tools and APIs that Apple supports. Apple generally supports and improves them well (except for Apple Watch), so really try not to reinvent the wheel.

The classes mentioned above, UIView and UIViewController, are the backbone of iOS apps. Those classes (and their subclasses) will make up the majority of your application.

### App Delegate

The _app delegate_ is the central class that controls the app on startup and shutdown, and handle external events like push notifications. For our tutorial, we won't modify it much, but if you do anything with third party libraries, you usually need to configure that here.

### Storyboard

Storyboard is Xcode's built-in layout builder. It is versatile, and will connect directly to your code via annotations. It's a visual tool, so you really have to see it in action to understand what it does.

### View Controller

As I mentioned, view controllers are extremely popular items to subclass. There are three methods to highlight, they all correspond to the life cycle of the view:

  - `viewDidLoad`: Do all initial configuration, like background color and initial values for text labels, here. This is called only once.
  - `viewDidAppear`: Do all things that must occur _every time the view appears here_. It is best to use `viewDidLoad` if you can, but if some UI or animation needs updating whenever the user sees that, do it here.
  - `viewDidDisappear`: This method is useful for stopping animations or UI processes that are just wasting resources when the user isn't looking at the view.

__Always__ call the `super` method before doing your own implementation. 


# Today's App

We are going to make a shopping list app that uses some common UI elements and techniques.

## Setting Up Your Project

  1. Open Xcode
  2. Create New Project
  3. Product Name: Shopping List
  4. Organization Name: Your Name
  5. Organization Identifier: edu.cmu.andrewid
  6. Language Swift
  7. Devices Universal
  8. Don't check any boxes


## Defining a Model

_[Diff](https://github.com/scottylabs/IntroToSwiftExample/commit/f7134599c2ba620ec7373855ff5d3bc60835ae67) for this step_

We are only creating one model object, an `Item` class with two fields: `name` and `count`.

The entire class can be defined like this:

```
class Item {
    var name: String
    var count: Int

    init(name: String, count: Int) {
        self.name = name
        self.count = count
    }
}
```


## Subclassing UITableViewController

We will subclass `UITableViewController`, because it is very applicable to a lot of different applications, and requires way less work than `UIScrollView`.


### Adding Infomation

_[Diff](https://github.com/scottylabs/IntroToSwiftExample/commit/b8ea6b54deb9197efabcf2318ff1395f14ea257a) for this step_

We need to set up our `ViewController` to be a subclass of `UITableViewController`. Change the class declaration to the following:

```
class ViewController: UITableViewController {
```

Xcode probably added method skeletons to your class, delete the one that says `didRecieveMemoryWarning`, but keep the `viewDidLoad`.

Okay, now lets add our list of objects. Let's add a field called `itemList`, and initialize it to an empty list of objects:

```
var itemList = [Item]()
```

In `viewDidLoad`, lets add `Item` objects to it. It is customary to do all setup in this method, rather than overriding constructors.

``` 
override func viewDidLoad() {
    super.viewDidLoad()
    itemList.append(Item(name: "Milk", count: 1))
    itemList.append(Item(name: "Eggs", count: 12))
    itemList.append(Item(name: "Tub of cookie dough", count: 3))
}
```

Now that we have information, we need to configure the table to show it.

### Table Size

_[Diff](https://github.com/scottylabs/IntroToSwiftExample/commit/0fb88c14dc52c66f390bf20ed2e5464cffef8919) for this step_

These methods correspond to protocols called `UITableViewDelegate` and `UITableViewDataSource`, which are implemented by `UITableViewController`.

First, we need to tell the table how many rows to display:

```
override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
    return 1
}

override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return itemList.count
}
```

### Populate Table

_[Diff](https://github.com/scottylabs/IntroToSwiftExample/commit/00fcbc7a9675fa8ea5f617fed80b13dd02d97f30) for this step_

Next, for a given row in the table, we have to tell the table what information to show:

```
override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
    var cell = tableView.dequeueReusableCellWithIdentifier("ItemCell")
    if cell == nil {
        cell = UITableViewCell(style: UITableViewCellStyle.Value1, reuseIdentifier: "ItemCell")
    }

    if let cell = cell where indexPath.row < itemList.count {
            let item = itemList[indexPath.row]
            cell.textLabel?.text = item.name
            cell.detailTextLabel?.text = "\(item.count)"
    }

    return cell!
}
```

All of the `if let` crap is becuase `dequeue` is a notoriously messy function, so this is essentially a bunch of null checking. The `dequeue` is actually a beautiful memory management tool that allows for large scrolling tables, but it is a bitch to deal with, so just copy paste this code when you need to.

## Designing the Interface

First add a "Label" item to the view, and run your app, just to see the simulator working.

Now for our interface:

  1. Select `Main.storyboard` from the file browser
  2. Click on the white bar that says view controller. Delete it. Drag a `UITableViewController` into the storyboard area from the object browser in the bottom right. The icon looks like a yellow circle with lined paper on it. In its Identity Inspector (Newspaper icon), set the class to `ViewController` which is our custom class the project created by default.
  3. In the menu bar, go to `Editor->Embed in..->Navigation Controller`
  4. Click on the Navigation bar on the table VC (View Controller). In the attributes inspector on the right (it looks like a line with a downward arrow), enter a title like "Shopping List".
  5. In the outline on the left, select the "Table View Cell", or click on the cell in the image. In the attributes inspector where it says "Style", select `Right Detail`. Set it's identifier to `ItemCell`.

That's it for part one of the lab! We may add more UI later if we have time!


__Hit run!!!__


## Extra Features

### Swipe to Delete

_[Diff](https://github.com/scottylabs/IntroToSwiftExample/commit/f49a0b27933b85b346e7b52852faaff2a0bda87d) for this step_

When we have bought an item, we want to easily delete it from the list, so we don't see it anymore. It is tasks like this that make you appreciate iOS's built-in functionality. Just add the following delegate method to add swipe-to-delete functionality to your shopping list.

```
override func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
    if editingStyle == .Delete && indexPath.row < itemList.count {
        itemList.removeAtIndex(indexPath.row)
        tableView.deleteRowsAtIndexPaths([indexPath], withRowAnimation: .Automatic)
    }
}
```


