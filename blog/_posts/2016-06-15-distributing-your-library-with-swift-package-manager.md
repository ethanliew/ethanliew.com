---
layout: blog_post
comments: true
title: "Distributing a library with Swift Package Manager"
meta_description: "This tutorial shows how to distribute a Swift library with Swift Package Manager."
tags: programming
---


[Swift Package Manager](https://swift.org/package-manager/) allows to use external code in your Swift app. In this tutorial I will show how create a library that can be used with Swift Package Manager. The method was tested with Xcode 8 Beta and Swift 3.0.

## Create a Swift library

#### 1) Create the library directory

```
mkdir MyLibrary
cd MyLibrary
```

#### 2) Create the library source file


Create the **Sources** directory

```
mkdir Sources
```

and add the library source file **my_library.swift**:

```
touch Sources/my_library.swift
```


#### 3) Write the library code

Write your library code in the **Sources/my_library.swift** file.

```Swift
public func hiThere() -> String {
  return "Hello from the library!"
}
```

Here we created a method `hiThere` that simply returns a greeting text. Notice that we marked the method as **public** in order to make it accessible to the library users.


#### 4) Create the Package.swift file for the library

Next, create an empty **Package.swift** file in the **root directory** of the library

```
touch Package.swift
```

and put the following code in it.

```Swift
import PackageDescription

let package = Package(
    name: "MyLibrary",
    exclude: ["DirectoryToExclude"]
)
```

If you have an existing library that has Swift files that you do not want to distribute to users you can exclude them in the **Package.swift** file with the **exclude** option.


#### 5) Publish the library

Now we need to publish the library so other people can download it with Swift Package Manager. This can be done by uploading the code to a Git hosting service of your choice. If you are new to this please refer to the instructions from your hosting service. Here is how to [create a repository](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/) on GitHub.

#### 6) Release a new library version


After the code is published we create a new tag version **1.0.0** and push it to the hosting service:

```
git tag 1.0.0
git push origin --tags
```

In the future you can release updates to your library by creating new version tags, for example, *1.0.1* or *1.0.2*. If the library contains breaking changes [the convention](http://semver.org/) is to increment the first *major* digit of the version, for example, *2.0.0* or *3.0.0*. This will ensure the apps of existing library users will not be broken.

Congratulations! The library is published and ready to be used with Swift Package Manager.

## Using your library with Swift Package Manager

Now we can test if the library is working by creating a simple Swift app that includes it with Swift Package Manager.

#### 1) Create app directory.

```
mkdir MyApp
cd MyApp
```

#### 2) Create an empty *Package.swift* file

```
touch Package.swift
```

#### 3) Write Swift program code

Next we create the **Sources** directory and add the `main.swift` file that prints "Hello World" message.

```
mkdir Sources
echo 'print("Hello world!")' > Sources/main.swift
```


#### 4) Build and run

Now we build

```
swift build
```

and run the app.

```
.build/debug/MyApp
```

If everything went well you will see "Hello world!" message in the terminal.

#### 5) Include your library to the Package.swift file.

Now it is time to use our library. Add the following text to the **Package.swift** file.

```Swift
import PackageDescription

let package = Package(
    name: "MyApp",
    targets: [],
    dependencies: [
        .Package(url: "https://github.com/evgenyneu/LibraryWithSwiftPackageManager.git",
                 versions: Version(1,0,0)..<Version(2,0,0))
    ]
)
```

This dependency includes a sample project that I created. Your app, of course, will point to your own repository.

You may want to show the Package.swift file contents in your library's README file as a setup instruction for the users. In this example your users will download the most recent library version that starts with 1. For example, it will use *1.3.6* or *1.0.0* version but not *2.0.1*.

#### 6) Build your dependency

Run `swift build` and it will download and build the library into the *Packages* directory.


#### 7) Use your library

Finally, we can test the library by using it in the `Source/main.swift` file:

```Swift
import MyLibrary
print(hiThere())
```

Build/run the app again and it you will see "Hello from the library!" text.

```
swift build
.build/debug/MyApp
```



## Examples

* [LibraryWithSwiftPackageManager](https://github.com/evgenyneu/LibraryWithSwiftPackageManager) is the demo library used in the tutorial.

* [SigmaSwiftStatistics](https://github.com/evgenyneu/SigmaSwiftStatistics) is an example of a real library that supports Swift Package Manager.

