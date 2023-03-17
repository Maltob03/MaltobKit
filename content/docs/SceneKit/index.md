---
title: "First implementation of SceneKit"
date: 2020-08-14
draft: false
description: "All the configuration variables available in Blowfish."
slug: "SceneKit"

---

## What is SceneKit?

SceneKit is a framework for swift that perform 3D rendering inside your app.

> SceneKit combines a high-performance rendering engine with a descriptive API for import, manipulation, and rendering of 3D assets. Unlike lower-level APIs such as Metal and OpenGL that require you to implement in precise detail the rendering algorithms that display a scene, SceneKit requires only descriptions of your scene’s contents and the actions or animations you want it to perform.

So what we need to implement SceneKit in our project is basically a 3D model that we want to display inside our app and that's it!

There will be a complete article on how to import a custom 3D model inside our project, but for now let's assume that we already have the model in our project.

## New SwiftUI file

Add new SwiftUI file, name it as you want, I called mine CustomSceneView

## Create the SceneKit View

First of all we need to import the framework in the file that we have created before, like this:

```swift
Import SceneKit

```

Now we have to create the structure for this view that will render our fantastic 3D model. SceneKit is a framework that works with the old UIKit so if you have always see only SwiftUI component it could look a little bit different, but let's see:

```swift
struct CustomSceneView: UIViewRepresentable {
     ...
}
```


As you can see it looks different from the classical:

```swift
struct ContentView: View{
    var body: some View{
        ...
    }
}
```

And for this reason we need to delete the preview from the SceneKit file.

## The scene variable

After we have created the struct we need to declare a variable binding.

```swift
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

```swift
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

```swift
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



```swift
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

