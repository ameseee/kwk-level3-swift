footer: KWK Swift/iOS: Core Data
slidenumbers: true

# Core Data

---

# Learning Goals

* Students will explain the purpose and responsibility of Core Data
* Students will be able to use Core Data to store and retrieve photos and captions

---

# What is Core Data?

* Core Data is an Apple Framework that allows us to manage data in an application
* It can save information
* It can give us information back when we ask for it

^ Core Data, for our purposes, will act like a little database. It stores what is called "model layer objects" - the model being the structure of all the data that is important to the app. There is also a view layer, and like it sounds, that is the part the user sees. It allows the user to VIEW the MODEL, or data/information that they care about.

---

# Creating an Entity


![inline](slide_images/add_entity.png)

^ Think of an entity as a bucket of information. We just need one big bucket, it will hold a bunch of photo/caption pairs.

---

# Allow External Storage


![inline](slide_images/allow_external_storage.png)

^ Checking this box makes sure that we have enough space - pictures can take up a LOT of space!

---

# Connections

* Establish two connections between StoryBoard and code:
  - Action for `Add Photo` button
  - Outlet for caption text field

^ This is the set up to begin lab on their own - accessing context and creating an instance of the CoreData Object.

---

# Plan out our Code

* Let's think through what all needs to happen each time we save/add a photo

^ RECOMMENDATION: Have one teacher writing on whiteboard or typing on slide while the other facilitates.
Students might generate - store the picture and caption in Core Data, ask Core Data for all the pictures when we go back to the table view, tell the program to go back to the table view once we tap the save/add button.
Teacher will probably need to suggest (I'd do this after students share ideas, and put them in order): get Core Data context (basically tell the program, hey - we need to the Core Data info, please). Create an instance of the Core Data Object and store it in a variable. Update the text with whatever the used typed in. Update the photo with whatever photo the user selected. Convert the photo to a format the computer can store. Save all of these updates. Move the user back to the table view! Once we are in the table view, we will also have to ask Core Data for all the pictures so we can display them.
Tell the students they will have a step by step lab for this part; this discussion was just to do some thinking about what our code will do!

---

```
INSIDE SUBMIT BUTTON TAPPED ACTION:
       if let context = (UIApplication.shared.delegate as? AppDelegate)?.persistentContainer.viewContext {
            
            let photoToSave = Photos(entity: Photos.entity(), insertInto: context)
            photoToSave.caption = captionText.text
            photoToSave.emojiIcon = emojiIcon.text
            
            if let userImage = newImageView.image {
                if let userImageData = UIImagePNGRepresentation(userImage) {
                    photoToSave.imageData = userImageData
                }
            }

            (UIApplication.shared.delegate as? AppDelegate)?.saveContext()
            
            navigationController?.popViewController(animated: true)
        }
        
    }
```
INSIDE TABLE VIEW:
import UIKit

class PhotoTableViewController: UITableViewController {

    var photos : [Photos] = []
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
    
    override func viewWillAppear(_ animated: Bool) {
        getPhotos()
    }

    func getPhotos() {
        if let context = (UIApplication.shared.delegate as? AppDelegate)?.persistentContainer.viewContext {
            
            if let coreDataPhotos = try? context.fetch(Photos.fetchRequest()) as? [Photos] {
                if let unwrappedPhotos = coreDataPhotos {
                    photos = unwrappedPhotos
                    tableView.reloadData()
                }
            }
        }

    }
    
    override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        performSegue(withIdentifier: "moveToDetail", sender: photos[indexPath.row])
    }
    
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "moveToDetail" {
            if let photoDetailView = segue.destination as? PhotoDetailViewController {
                if let photoToSend = sender as? Photos {
                    photoDetailView.photo = photoToSend
                }
            }
        }
    }

    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return photos.count
    }

    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell()
        
        let cellPhoto = photos[indexPath.row]
            
        cell.textLabel?.text = cellPhoto.caption
        
        if let cellPhotoImageData = cellPhoto.imageData {
            if let cellPhotoImage = UIImage(data: cellPhotoImageData) {
                cell.imageView?.image = cellPhotoImage
            }
        }
        
        return cell
    }

    override func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool {
        return true
    }
    
    override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
        if editingStyle == .delete {
          if let context = (UIApplication.shared.delegate as? AppDelegate)?.persistentContainer.viewContext {
            let photoToDelete = photos[indexPath.row]
            context.delete(photoToDelete)
            (UIApplication.shared.delegate as? AppDelegate)?.saveContext()
            getPhotos()
          }
        }
    }
}

```

```
