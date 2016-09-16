# Watches
A simple stopwatches use to logging out the elapsed time between 2 timestamp, easily find out execution time of any function, a block of code or async callback.

## Installation
### Cocoapods
Add below line to your project Podfile
```
pod 'Watches'
```
Then run `pod install` from Terminal

### Manual
Drag and drop `Watches.swift` source file into your project

## Usage
By default, Watches will automatically print out the debugging log in format

```"Elapsed interval for \(identifier) is \(interval)"```

If you want to disable this auto log feature, set 

```Watches.printElapsedTimeAutomatically = false```

Track elapsed time of any async callback (e.g : network request)
call tick to tickoff the time with specific identifier right before calling async task
```swift
Watches.tick("LoadProfile")
```
Then call tock with the corresponding identifier inside async callback closure
```swift
Watches.tock("LoadProfile")
```

For example with Alamofire
```swift
Watches.tick("LoadProfile")
Alamofire.request(loadProfileUrl).responseJSON { response in
    Watches.tock("LoadProfile")
}
```

It will print out the elapsed intervals in default format

Result: ```Elapsed interval for LoadProfile is 1.20113898515701```

Feel free to provide custom format that you desire

```Watches.tock("LoadProfile") { debugPrint("Time needed for \($0) is \($1)") }```

If you want the elapsed interval for your own purpose, get it from the tock return value

```
//You can disable auto print feature if you want
Watches.printElapsedTimeAutomatically = false

//Then get the result from tock function
let elapsedTime = Watches.tock("LoadProfile")
````

Determine the execution time of any block of code.
```swift
Watches.create("1 million times loop").tick {
	//Block of code to measure
    for _ in 0...10000000 { }

}.tock()
```
Result: ```Elapsed interval for 1 million times loop is 0.214788019657135```

If you want custom print format, just need to provide the tock callback closure.
```swift
Watches.create("2 million times loop").tick {
	//Block of code to measure
    for _ in 0...20000000 { }

}.tock { debugPrint("Time for finishing \($0) is \($1)")}
```
Result: ```Time for finishing 2 million times loop is 0.411646008491516```

If you just want to get the result
```swift
let executionTime = Watches.create("MyClosure").tick { 
	//Block of code to measure
	sleep(3)
}.tock()

debugPrint(executionTime)
```
Result: ```3.0011550188064575```

## License
Watches is released under the MIT License. See LICENSE file for more detail. Copyright © Vuong Huu Tai
