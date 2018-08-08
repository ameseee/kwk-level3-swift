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
* Embed in Navigational Controller (select this option from Editor)
![inline](./assets/embedInNav.png)
* With the Navigational Controller selected, make sure `Is Initial View Controller` is checked in the Attributes Inspector
* Add a title to the NavItem (ToDo List or whatever you prefer)
* Add a Bar Button Item to the top right of the Nav Bar (you can customize this however you like... it will eventually take you to add a new ToDo)
* Create a ToDoTableViewController file (remember to inherit from a UITableViewController) and connect it to the TableViewController on your storyboard

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

