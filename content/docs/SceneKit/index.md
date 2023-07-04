---
title: "First implementation of SceneKit"
date: 2020-08-14
draft: false
description: "All the configuration variables available in Blowfish."
slug: "SceneKitT"

---

## What is SceneKit?

SceneKit is a powerful framework for rendering 3D graphics in Swift apps. In this tutorial, we'll guide you through the process of integrating SceneKit into your SwiftUI project. Let's get started!


> SceneKit combines a high-performance rendering engine with a descriptive API for import, manipulation, and rendering of 3D assets. Unlike lower-level APIs such as Metal and OpenGL that require you to implement in precise detail the rendering algorithms that display a scene, SceneKit requires only descriptions of your scene’s contents and the actions or animations you want it to perform.

So what we need to implement SceneKit in our project is basically a 3D model that we want to display inside our app and that's it!

## Creating the SceneKit View

First, create a new SwiftUI file. You can name it whatever you like; for this tutorial, we'll name it "CustomSceneView".

Now, import the SceneKit framework into the file:

```swift
Import SceneKit

```

Next, define the structure for the SceneKit view using the UIViewRepresentable protocol:

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


## The scene variable

Within the CustomSceneView structure, declare a @Binding variable for the scene:

```swift
struct CustomSceneView: UIViewRepresentable {
     @Binding var scene: SCNScene?
}
```

Let's discuss this line of code:

1. @Binding establishes a connection to another variable called scene, which is declared in another view.
2. The ? indicates an optional SCNScene object, which means the variable can either have a value or be nil.
3. As mentioned earlier, SCNScene is the type of object that helps us render 3D models.






## Configuring the SceneKit View

Now, let's make things happen! We'll define the makeUIView function to configure our SceneKit view:

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

Let's analyze what this function does:

1. It returns a SCNView object
2. We create a constant called view, which is an instance of SCNView().
3. We configure the view by enabling camera control, enabling default lighting, setting the antialiasing mode, assigning the scene variable to the view's scene property, and setting the background color to clear.


## Summary

To summarize the concepts covered so far, let's use an analogy: think of a film projector with a projector itself and a film reel.

In this case, our CustomSceneView structure is the projector. The @Binding variable represents the film reel, and the makeUIView function defines how the projector works.

Now, your file should look like this:

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

Now, let's use the CustomSceneView structure in a SwiftUI file.

Declare a @State variable of type scene:



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


## Conclusion

Congratulations! You've successfully integrated SceneKit into your SwiftUI project. We covered the basic steps for rendering a 3D model in your app using SceneKit. Feel free to explore further and enhance your project with SceneKit's powerful capabilities.

Remember, SceneKit provides a versatile and user-friendly way to incorporate 3D graphics into your apps, and SwiftUI simplifies the process of building user interfaces. Combine them effectively to create amazing experiences!