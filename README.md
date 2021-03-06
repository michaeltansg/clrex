# clrex
Simple script to generate colors factory methods from clr files

> Probably colors are not the only resources that you would like to operate on in a strongly typed safe manner. For that checkout [SwiftGen](https://github.com/AliSoftware/SwiftGen) or [R.Swift](https://github.com/mac-cain13/R.swift) which both support color pallets and many other cool stuf.

This basic script will help to generate UIColor factory methods from you system colors palettes.
Inspired by [this tip](http://natashatherobot.com/xcode-color-palette/) by [@natashatherobot](https://twitter.com/natashatherobot) and [ColorTools](https://github.com/ramonpoca/ColorTools).

# Usage

To use this script in your project download `clrex.swift`, put it somewhere in your project folder and add new "Run Script" phase _before_ "Compile Source":

```
xcrun swift -sdk $(xcrun --show-sdk-path --sdk macosx) PATH_TO_SCRIPT/clrex.swift
```

If you prefer to use binary then build it with `swift build` and run it in "Run Script" phase using `./PATH_TO_BINARY/clrex`

### Input arguments

`-i PATH` - the path to folder to lookup `*.clr` files. By defaul will lookup `~/Library/Colors`.

`-o PATH` - the path to generated file. By default will generate file `Colors.generated.swift` in current folder.

`-p ios|osx` - the platform to generate file for. By default will generate file for iOS.

`--setup` - if specified will install colors list to `~/Library/Colors/`

`add <color list name> <color name> <hex color>` - add color to color list in input folder, or create new list.

`delete <color list name> <color name>` - delete color from color list

_Example_:

```
./clrex -i ./Palletes/ -o ./Colors.swift -p ios
```

This command will lookup `./Palletes` folder and will generate `./Colors.swift` file for iOS.

```
./clrex -i ./Palletes/ add MyColors Orange ffa500 --setup 
```

This command will lookup `./Palletes` folder for `MyColors.clr` and will add `Orange` color (`#ffa500` in _Generic RGB color space_) and then will setup color list in user library folder.

```
./clrex -i ./Palletes/ delete MyColors Orange
```

This command will remove a color added with previous command.

If you use clrex in "Run Script" phase of your project you can use input and output files instead of command line arguments. Script will use first input file as a folder for lookup and first output file as path to generated file.

Then build your project once and add generated file to the project.

_Tip: If you change your source *.clr file but don't see changes to be reflected in generated file when you build then clean your project and build again._ 


# Generated code example

crlex will generate enum for each `*.clr` file in lookup folder named by the name of the file. Factory methods will be named by the names of colors in those palettes with removed spaces and suffixed with `Color` if needed.

For iOS:

```swift
import UIKit

enum MyColors {

  static func blackColor() -> UIColor {
    return UIColor(red: 0.000000, green: 0.000000, blue: 0.000000, alpha: 1.000000)
  }

  static func grayColor() -> UIColor {
    return UIColor(red: 0.265146, green: 0.265153, blue: 0.265149, alpha: 1.000000)
  }

}

```

For OSX:

```swift
import AppKit

enum MyColors {

  static func blackColor() -> NSColor {
    return NSColor(calibratedRed: 0.000000, green: 0.000000, blue: 0.000000, alpha: 1.000000)
  }

  static func grayColor() -> NSColor {
    return NSColor(calibratedRed: 0.265146, green: 0.265153, blue: 0.265149, alpha: 1.000000)
  }

}
```

You can check [this repo](https://github.com/ilyapuchka/ViewControllerThinning) for example of project using clrex.

# License
clrex is created by [Ilya Puchka](https://twitter.com/ilyapuchka) and released under a MIT License.
