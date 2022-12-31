---
title: "CoreData"
date: 2020-08-14
draft: false
description: "All the configuration variables available in Blowfish."
slug: "CoreData"

---


## What is CoreData

CoreData is maybe one of the basic framework that you need in order to get data persistance in your app.

The implementation could seem very complicated but if you understand how it works maybe it could be a little bit more clear. 
For this project we need different type of swift file, but we will see this and other stuff don't worry. Let's start


> Use Core Data to save your application’s permanent data for offline use, to cache temporary data, and to add undo functionality to your app on a single device. To sync data across multiple devices in a single iCloud account, Core Data automatically mirrors your schema to a CloudKit container. Through Core Data’s Data Model editor, you define your data’s types and relationships, and generate respective class definitions. Core Data can then manage object instances at runtime to provide the following features.


## Remember, import the library !

Open a new swift file (not swiftUI be careful), you can name it as you want. I called main **Persistance**
As usual the first step when we use a certain framework is import the library.

```
import CoreData
```

## The Controller

So in order to work with our stored data we need a controller that work as a bridge between CoreData and our personal data structure.



```
import Foundation

import CoreData

struct PersistenceController {
    static let shared = PersistenceController()
    
    let container: NSPersistentContainer
    init() {
        container = NSPersistentContainer(name: "Accounts")
        
        container.loadPersistentStores { (storeDescription, error) in
            if let error = error as NSError? {
                fatalError("Container load failed: \(error)")
            }
        }
    }
}

```

Let's see what is this "Controller" 

1. We have creaated a struct called PersistanceController, inside we define a constant that is an object of type PersistenceController()
2. Then we define the container (our box that will contain our database) and we initialize it defining the constant container with the Container Object
3. Then there are some control error


## The DataModel file

Inside the DataModel file we define the structure of the database. In other words how the data will be stored inside the database

Now if you never work with a database it's a little bit complex to work with data flow.

In the DataModel file we define an **entity** of the real world that could be an **Account** as for the example. Now we have to imagine what charaterize the entiy so its **attributes**.

**The entity account has as attributes Account_Name, mail, password for example**

So imagine that an user could have more than account, how these will be stored.


Account_Name | Mail | Password |
-------------|------|----------|
Apple | mail@domain.com| Secret |
Google | mail@domain.com| Secret |
McDonald's | mail@domain.com| Secret |
Microsoft | mail@domain.com| Secret |

So it works like a numbers file with rows and column




## Create the SceneKit View

First of all we need to import the framework in the file that we have created before, like this:

```
Import SceneKit

```

Now we have to create the structure for this view that will render our fantastic 3D model. SceneKit is a framework that works with the old UIKit so if you have always see only SwiftUI component it could look a little bit different, but let's see:

```
struct CustomSceneView: UIViewRepresentable {
     ...
}

```


As you can see it looks different from the classical:

```
struct ContentView: View{
    var body: some View{
        ...
    }
}
```

And for this reason we need to delete the preview from the SceneKit file.

## The scene variable

After we have created the struct we need to declare a variable binding.

```
struct CustomSceneView: UIViewRepresentable {
     @Binding var scene: SCNScene?
}
```

Ok now we have to discuss a lot about this single line of code.

1. The "@Binding" it's a way to connect to another variable **scene** that is declared into another view.
2. The ? means an optional SCNScene object so the variable will either have a value or no value. Since no value is assigned to the variable this means that is nil.
3. As we said before SCNScene it's the type of object that will help us to render our 3D models






## Make things happen!

Now that we have our var scene we need to tell to swift how to show this var and what specifc it has so let's look the code

```
func makeUIView(context: Context) -> SCNView {
        let view = SCNView()
        view.allowsCameraControl = true
        view.autoenablesDefaultLighting = true
        view.antialiasingMode = .multisampling2X
        view.scene = scene
        view.backgroundColor = .clear
        return view
    }
```

Try to analyze what this function does:

1. It returns a SCNView value
2. Declare a constant "view" that will be a SCNView() object
3. To this object we need to add some specific like cameraControl and other Stuff
4. The most important: to this view constant (that remember is a SCNView object) we say what scene it has to display


## Summary

In order to be sure that all this information are clear enough i want to make a little discorsive summary without code:

Remember our goal is to render a 3D model inside our app

So think about a film projector, It has the projector itself and the film reel.

The struct that we have defined is the projector.
The @Binding var is the place where the film reel goes and the function is how the projector works

And the film reel ?
The film reel is another view where we define our model and we call the film projector to work

So this is how your file would look:

```
import SwiftUI
import SceneKit

struct CustomSceneView: UIViewRepresentable {
    
    @Binding var scene: SCNScene?
    
    func makeUIView(context: Context) -> SCNView {
        let view = SCNView()
        view.allowsCameraControl = true
        view.autoenablesDefaultLighting = true
        view.antialiasingMode = .multisampling2X
        view.scene = scene
        view.backgroundColor = .clear
        return view
    }
    
    func updateUIView(_ uiView: SCNView, context: Context) {
    }
}

struct CustomSceneView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

## The last steps

So now we can use the struct that we have defined into a swiftUI file.

What we need is to declare a @State var of type scene



```
import SwiftUI
import SceneKit


struct Home: View {
    @State var scene: SCNScene? = .init(named: "name_of_your_model.scn")
    var body: some View {
        VStack{
            CustomSceneView(scene: $scene)
                . frame (height: 350)
                .padding ()
        }
    }
}
struct Home_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
    
}
```

