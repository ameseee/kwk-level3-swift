# ToDo List

## What we're building

This is a really cool ToDo List application that has never been built before 🙄😛🤗. You can create a new ToDo, indicate if it is important, and delete it from your list once you have completed it. Genius! 

![inline](./assets/todolist.gif)

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

* Create a new file (File -> New -> File... -> Swift File -> Next) and name it `ToDo` (this will create a ToDo.swift file in our project)
* Delete `import Foundation` and add `import UIKIt`
* Create a ToDo class that has 2 instance variables/properties... `name` and `important` - if you need a refresher on how to create a class and add instance variables, take a look back at [this lesson](https://github.com/ameseee/kwk-level3-swift/blob/master/sessions/classes_objects_slides.markdown)

It probably makes sense to initialize `name` to an *empty string* and `important` to the boolean *false*. And that's all we need to do for our ToDo class!!!

## Create some ToDos

* Back in our ToDoTableViewController, we need to make a function that will create toDos and return an array of toDos (we'll hard code this for now and replace it a little later when we hook up CoreData)

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
* We also need to create a toDos property on our ToDoTableViewController class (above our viewDidLoad func)

```swift
var toDos : [ToDo] = []
```

* Inside `func viewDidLoad()`, delete all the commented out code and reassign toDos to our createToDos function (now toDos will be the array of toDos we returned from the function)

```swift
override func viewDidLoad() {
  super.viewDidLoad()

  toDos = createToDos()
}
```

Let's clean this file up a little more. You can delete all the other functions in this file **except** the `tableView` function with `numberOfRowsInSection`, the `tableView` function with `cellForRowAt`, and the last `prepare` function that has to do with segue navigation (you can leave this commented out for now).

* In the `tableView` function with the argument `numberOfRowsInSection`, we want to return toDos.count

```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return toDos.count
}
```

* In the `tableView` function with the argument `cellForRowAt`, we want to copy the string **reuseIdentifier**
* Now go back to the Storyboard and click right under Prototype Cells to highlight the Table View Cell
* In the Attributes Inspector, paste **reuseIdentifier** into the `Identifier` field
* Now inside that same `tableView` function, we need to access a single toDo

```swift 
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
  let cell = tableView.dequeueReusableCell(withIdentifier: "reuseIdentifier", for: indexPath)

  let toDo = toDos[indexPath.row]

  return cell
}
```

* Now let's add some code to get our toDos to show up and to indicate if the toDo has been marked important

```swift
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
  let cell = tableView.dequeueReusableCell(withIdentifier: "reuseIdentifier", for: indexPath)
  
  let toDo = toDos[indexPath.row]
  
  if toDo.important {
    cell.textLabel?.text = "❗️" + toDo.name
  } else {
    cell.textLabel?.text = toDo.name
  }
  
  return cell
}
```

**BOOM!** You should now be able see our hard-coded toDos in the Table View when you run the application!

## Design the AddToDoViewController

Now we're going to build our AddToDoViewController! Let's first create it visually, then we'll add some code to go with it.

* Drag a new View Controller from the Object Library to the Storyboard
* Create a segue from the add button (+) on our Table View Controller to our new View Controller (Action Segue -> Show) - this is going to allow us to move back and forth between views

Let's think about what objects we will need to add to this view. Looking back at the finished app at the top of the page, we need...
  - a **label** for `Title:`
  - a **text field** where the user can add a ToDo
  - another **label** for `Important:`
  - a **switch** to toggle important/not important
  - a **button** to add a ToDo

Don't forget to add constraints so that it looks good on all screens!!!

Ideally, we probably would like our switch to be in the `off` position when we come to the page. If we highlight the switch in our storyboard and open the Attributes Inspector, we can change the state of the switch to off.

Now is probably a good time to run this in your simulator and make sure everything looks how you want it to.

* Now let's add the code file that will be associated with this View Controller (File -> New -> File... -> Cocoa Touch Class -> Next) - this will be a subclass of UIViewController (I called mine AddToDoViewController)
* In your storyboard, select this View Controller and then open the Identity Inspector and add the new class you just created to connect the code to the View Controller
* Next, let's just make the necessary Outlet and Action connections we need

```swift
import UIKit

class AddToDoViewController: UIViewController {
    
  @IBOutlet weak var titleTextField: UITextField!
  @IBOutlet weak var importantSwitch: UISwitch!
  
  override func viewDidLoad() {
      super.viewDidLoad()

  }

  @IBAction func addTapped(_ sender: Any) {
  }
    
}
```

## Adding ToDos

Now we want to work on adding a new ToDo from the AddToDoViewController, then popping back to the ToDoTableViewController and being able to see that new ToDo in our Table View

* First, we need to make a new ToDo item inside our `addTapped` function and grab the value of the input field as well as the importance status of the ToDo

```swift
@IBAction func addTapped(_ sender: Any) {
  let toDo = ToDo()
  
  if let titleText = titleTextField.text {
      toDo.name = titleText
      toDo.important = importantSwitch.isOn
  }
}
```

Now we need to add this ToDo to our ToDo array. We have a small problem though... that ToDo array lives on another class (ToDoTableViewController). No worries! We can fix this by adding a reference to that class in our AddToDoViewController. All we need to do is add the following line of code above our Outlets.

```swift
var previousVC = ToDoTableViewController()
```

Now in the `ToDoTableViewController`, we need create a `prepare` for segue function (this gets called right before a segue happens)

* Un-comment out the `prepare` function at the bottom of `ToDoTableVeiwController.swift` and add the following code to create a reference to the AddToDoViewController

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
  let addVC = segue.destination as! AddToDoViewController
  addVC.previousVC = self
}
```

Now back in the AddToDoViewController, we can access the ToDo array that lives in the ToDoTableViewController.

* Add the new ToDo to the array and update/reload the Table View

```swift
@IBAction func addTapped(_ sender: Any) {
  let toDo = ToDo()
  
  if let titleText = titleTextField.text {
      toDo.name = titleText
      toDo.important = importantSwitch.isOn
  }
  previousVC.toDos.append(toDo)
  previousVC.tableView.reloadData()
}
```

So now if we run our application, we can add a new ToDo, click `< ToDo List`, and our new ToDo is there in our Table View!!! Only thing left to do now for this part is have it automatically pop back to the Table View when the user taps `Add`. We just need to call a single function after we update/reload the Table View.

* Pop back to the previous view when the user taps the `Add` button

```swift
@IBAction func addTapped(_ sender: Any) {
  let toDo = ToDo()
  
  if let titleText = titleTextField.text {
      toDo.name = titleText
      toDo.important = importantSwitch.isOn
  }
  previousVC.toDos.append(toDo)
  previousVC.tableView.reloadData()
  navigationController?.popViewController(animated: true)
}
```

Just for a sanity check, run your application to make sure everything is working correctly.

## Completing/Removing a ToDo

Now we want to focus on being able to select a ToDo from the Table View and being taken to another view where we can mark a ToDo complete and remove it from our list.

* Add another View Controller to your Storyboard (I added mine below the ToDoTableViewController since it will segue from there)
* Select the ToDoTableViewController and create a segue from ToDo List (top, left icon in Table VC) to the new View Controller (Manual Segue -> Show) - this automatically gives us a nav item that can take us back to the ToDo List
* Add a **label** for our ToDo and a complete **button** to the Storyboard
* Create a code version of this View Controller (File -> New -> File... -> Cocoa Touch Class -> Next) - make this a subclass of UIViewController
* Go back to the Storyboard, select the view that you just created and connect it with the code file you just created

![inline](./assets/connectComplete.png)

* Create the necessary Outlet and Action in the code file

```swift
import UIKit

class CompleteToDoViewController: UIViewController {
    
  @IBOutlet weak var titleLabel: UILabel!
  
  override func viewDidLoad() {
      super.viewDidLoad()

  }

  @IBAction func completeTapped(_ sender: Any) {
  }
}
```

When the user taps on a single ToDo, we want to initiate the segue from the ToDo List to the CompleteToDoViewController. In order for that to happen, we have to give the segue a name.

* Highlight the segue between the ToDoTableViewController and the CompleteToDoViewController on the Storyboard and open the Attributes Inspector. Let's give this segue a name of `moveToComplete` in the `Identifier` field

<<<<<<< HEAD
![inline](./assets/moveToComplete.png)

Now we need to go back to the ToDoTableViewController and add a `tableView` function that has an argument of `didSelectRowAt`. Inside of here, we want to call performSegue (you should be able to press `Enter` to autocomplete this function with the correct arguments). Here is where we are going to give it that identifier of moveToComplete (this needs to be a string). We also need to grab the single row/ToDo to pass to the sender.

```swift
override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {

  // this gives us a single ToDo     
  let toDo = toDos[indexPath.row]
  
  performSegue(withIdentifier: "moveToComplete", sender: toDo)
}
```

## Adding to CoreData

## Fetching from CoreData

## Deleting from CoreData

