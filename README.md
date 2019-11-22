# iOS Modal 

This sample comes from this StackOverflow question: [Control height of iOS 13 modal presentation](https://stackoverflow.com/questions/58507935/control-height-of-ios-13-modal-presentation)

![](modal-ios13-size.gif)

## How

- create your ChildViewController
- add a Vertical Stack View, put everything you need inside (or any other container view)
- calculate that container's view's height
- change height of ViewController's View to that
- set corner radius of View


```lang-swift
import UIKit

class ChildViewController: UIViewController {

    @IBOutlet weak var stackView: UIStackView!
    
    override func updateViewConstraints() {
        // distance to top introduced in iOS 13 for modal controllers
        // they're now "cards"
        let TOP_CARD_DISTANCE: CGFloat = 40.0
        
        // calculate height of everything inside that stackview
        var height: CGFloat = 0.0
        for v in self.stackView.subviews {
            height = height + v.frame.size.height
        }
        
        // change size of Viewcontroller's view to that height
        self.view.frame.size.height = height
        // reposition the view (if not it will be near the top)
        self.view.frame.origin.y = UIScreen.main.bounds.height - height - TOP_CARD_DISTANCE
        // apply corner radius only to top corners
        self.view.roundCorners(corners: [.topLeft, .topRight], radius: 10.0)
        super.updateViewConstraints()
    }
}

// https://stackoverflow.com/a/41197790/225503
extension UIView {
   func roundCorners(corners: UIRectCorner, radius: CGFloat) {
        let path = UIBezierPath(roundedRect: bounds, byRoundingCorners: corners, cornerRadii: CGSize(width: radius, height: radius))
        let mask = CAShapeLayer()
        mask.path = path.cgPath
        layer.mask = mask
    }
}
```
