# Tree Datastructure implementation in Swift
```swift
import Cocoa

class Node{
    var value: String = ""
    var children: [Node] = []
    weak var parent : Node?
    
    init(value:String) {
        self.value = value
    }
    
    func addChild(child:Node){
        self.children.append(child)
        child.parent = self
    }
    
    func search(value:String) -> Node?{
        if value == self.value{
            return self
        }
        for child in self.children{
            if let foundObj = child.search(value: value){
                return foundObj
            }
        }
        return nil
    }
    
}

extension Node: CustomStringConvertible{
    var description: String{
        var text = "\(value)"
        if !children.isEmpty{
            text = text + "\n\tChildren = {"
            for child in children{
                text = text + child.description + ","
            }
            text = text + "} "
        }
        return text
    }
}

```

**Lets create some nodes and create a family tree structure**
```swift

let starks = Node(value: "Rickard Stark")
let s1 = Node(value: "Eddard Stark")
let s2 = Node(value: "Brandon Stark")
let s3 = Node(value: "Benjen Stark")

let c1 = Node(value: "Robb Stark")
let c2 = Node(value: "Sansa Stark")
let c3 = Node(value: "Arya Stark")
let c4 = Node(value: "Brandon Stark")
let c5 = Node(value: "Rickon Stark")

s1.children = [c1,c2,c3,c4,c5]
starks.addChild(child: s1)
starks.addChild(child: s2)
starks.addChild(child: s3)
```

**Lets print the entire family tree for Rickard Stark**
``` swift
print(starks)
 Rickard Stark
     Children = {Eddard Stark
                    Children = {Robb Stark,Sansa Stark,Arya Stark,Brandon Stark,Rickon Stark,}
                ,Brandon Stark,
                Benjen Stark,}

```

**Lets print the entire family tree for Eddard Stark**
``` swift
print(s1)
 Eddard Stark
     Children = {Robb Stark,Sansa Stark,Arya Stark,Brandon Stark,Rickon Stark,}

```

**Below we are checking if Arya Stark exists in the family tree of starks **
``` swift

if let aryaStark = starks.search(value: "Arya Stark") {
    print("Found \(aryaStark)")
} else{
    print("Not able to find Arya Stark")
}
/*
 Found Arya Stark
 */


```

**Lets check if John Snow belongs to the Stark family **
```Swift
if let johnsnow = starks.search(value: "John Snow") {
    print("Found \(johnsnow)")
} else{
    print("Not able to find John Snow")
}
//Not able to find John Snow
```
