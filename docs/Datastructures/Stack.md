* Stack is one of the simplest data structures.
* Stack has LIFO - Last In first out order
*  Stack is like an array but with limited functionality
	- Push to add a new object to the top
	- Pop to remove the last object
	- top to look at the top most/last element without removing it


``` swift
import Cocoa

//Here the stack is created as a wrapper around a swift array that allows push an object to the stack, 
//pop the last object and look at the top element of the stack
public struct Stack<T> {
    fileprivate var array = [T]()
    
    public var isEmpty: Bool{
        return array.isEmpty
    }
    
    public var count: Int{
        return array.count
    }
    
    public mutating func push(_ element:T){
        array.append(element)
    }
    
    public mutating func pop() -> T?{
        return array.popLast()
    }
    
    public var top: T? {
        return array.last
    }
}
```
**Testing the stack implementation**

```swift
var stack = Stack<Int>()
stack.push(10)
print(stack) 
//Stack<Int>(array: [10])

stack.push(20)
print(stack) 
//Stack<Int>(array: [10, 20])

stack.push(30)
print(stack) //Stack<Int>(array: [10, 20, 30])
if let val = stack.pop(){
    print("The popped item = \(val)") 
    //The popped item = 30
}
print(stack) //Stack<Int>(array: [10, 20])

if let topElement = stack.top{
    print("The Top Element is = \(topElement)")
}
//The Top Element is = 20

print(stack) 
//Stack<Int>(array: [10, 20])


//The pop removes the last element however the top only looks at the last element without removing it.
```