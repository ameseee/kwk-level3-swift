# Timer App

StoryBoard needs to be created with appropriate actions and outlets.

Code in ViewController:

```swift
import UIKit

class ViewController: UIViewController {

    //create instance of Timer class, called timer
    var timer = Timer()
    
    //set default beginning time to 210 second
    var time = 210

    //outlet to the time left label
    @IBOutlet weak var timeLeft: UILabel!

    // function that I wrote (@objc needed to be written before declaration because we need to expose it 
    // to Objective C - probably because Timer/NSTimer is older and built for Objective C)
    @objc func decreaseTimer() {
        if time > 0 {
            time -= 1
            timeLeft.text = String(time)
        } else {
            timer.invalidate()
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    // action tied to pause button
    @IBAction func pauseTapped(_ sender: Any) {
    // call invalidate on the timer instance - this methods stops the timer
        timer.invalidate()
    }

    // action tied to play button
    @IBAction func playTapped(_ sender: Any) {
    // set timer instance to a scheduled timer, interval of 1 second
    // the selector ties to a function, so calls this function every second
        timer = Timer.scheduledTimer(timeInterval: 1, target: self, selector: #selector(ViewController.decreaseTimer), userInfo: nil, repeats: true)
    }

    // action tied to reset button
    @IBAction func resetTapped(_ sender: Any) {
    // resets timer to 210, update text
        time = 210
        timeLeft.text = String(time)
    }

    @IBAction func decreaseTapped(_ sender: Any) {
        time -= 10
        timeLeft.text = String(time)
    }

    @IBAction func increaseTapped(_ sender: Any) {
        time += 10
        timeLeft.text = String(time)
    }

}

```
