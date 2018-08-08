# ToDo List

## What we're building

![]()

## Getting Started

* Create a new Xcode project (iOS, Single View App)
* Make sure **USE CORE DATA** is selected
* Head over to Main.storyboard and delete the existing View Controller 
* From the Document Outline, delete the ViewController.swift file associated with that View Controller (Move to Trash)

## Adding and Designing a TableView

* From the Object Library, add a Table View Controller to the Storyboard
* With the Table View Controller selected in the Storyboard, embed it in a Navigation Controller (select this option from Editor)

![inline](./assets/embedInNav.png)

This should have added a Navigation Controller to the Storyboard and added a Navigation Item to our Table View Controller. We now want to make sure that this is our point of entry for our ToDo List application. 

* With the Navigational Controller selected, make sure `Is Initial View Controller` is checked in the Attributes Inspector

If we select the Navigation Item in the Table View Controller, we can now add a Title to our app (ToDo List or whatever you prefer)

![inline](./assets/navItem.png)

We now want to add a Bar Button Item that will take us to another View to add a ToDo. You can customize this however you like.

![inline](./assets/barButtonItem.png)

Great! Let's connect this Table View to some code!

* Create a ToDoTableViewController file (File -> New -> File... -> Cocoa Touch Class -> Next). 

Don't forget to make this a subclass of UITableViewController and connect it to the TableViewController on your storyboard!

![inline](./assets/connectToDoTableVC.png)

## Create a custom ToDo class

* Create a new file (select Swift file instead of Cocoa Class) named ToDo
* import UIKIt
* Create a ToDo class that has 2 instance variables/properties... `name` and `important`
* Initialize name to an empty string and important to the boolean false
* Back in our ToDoTableViewController, we need to make a function that will create toDos and return an array of toDos (we'll hard code this for now and replace it a little later)
```swift
func createToDos() -> [ToDo] {
        
  let swift = ToDo()
  swift.name = "Learn Swift"
  swift.important = true
  
  let dog = ToDo()
  dog.name = "Walk the Dog"
  // important is set to false by default
  
  return [swift, dog]
}
```
* We also need to create a toDos property on our class
```swift
var toDos : [ToDo] = []
```

## Design the AddToDoViewController

## Adding ToDos

## Completing/Removing a ToDo

## Adding to CoreData

## Fetching from CoreData

## Deleting from CoreData

