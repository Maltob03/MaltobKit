---
title: "How to implement FaceID"
date: 2020-08-14
draft: false
description: "All the configuration variables available in Blowfish."
slug: "FaceID"

---


## How to Implement FaceID in Your App

To implement Face ID authentication in your app, you can leverage the LocalAuthentication framework. The process is straightforward, so follow the steps below to enable Face ID authentication and enhance your app's security.

## Import the Framework
First, import the LocalAuthentication framework into your SwiftUI file:

```swift
import LocalAutentication
```

## Boolean Variable

In your view, declare a boolean variable using the @State property wrapper. This variable will track the authentication status:

```swift
 @State private var isUnlocked = false
```

The @State property wrapper allows the variable to change its value during the execution

## How the view works

Next, create the view that will display different content based on the authentication status. You can use an if statement and a VStack to achieve this:

```swift
VStack{
        if isUnlocked {
            ZStack{
                Text("Unlocked)
                    
                } else {
                    Text("Locked")
                }
            }.onAppear(perform: authenticate)
        }
```

The .onAppear(perform: authenticate) modifier ensures that the authenticate function is called when the view appears.

The authenticate function is responsible for initiating the Face ID authentication process. Here's an example implementation:

```swift
func authenticate() {
        let context = LAContext()
        var error: NSError?

        // check whether biometric authentication is possible
        if context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: &error) {
            // it's possible, so go ahead and use it
            let reason = "We need to unlock your data."

            context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, localizedReason: reason) { success, authenticationError in
                // authentication has now completed
                if success {
                    isUnlocked = true
                } else {
                    // there was a problem
                }
            }
        } else {
            // no biometrics
        }
    }

```

This function creates an instance of LAContext and checks if biometric authentication is available on the device. If it is, it presents the Face ID prompt to the user. Upon completion, the success parameter indicates whether the authentication was successful. If authentication succeeds, the isUnlocked variable is set to true.


## Face ID Usage Description

Before you can use Face ID in your app, you need to specify the usage description for Face ID in your app's Info.plist file. This is necessary to provide a clear explanation to the users about why your app requires access to Face ID.

So you have to add : NSFaceIDUsageDescription



## Conclusion

By following these steps, you can easily implement Face ID authentication in your app using the LocalAuthentication framework. Utilize the security and convenience of Face ID to enhance the user experience and protect sensitive data.

Feel free to customize the implementation according to your app's requirements. You can handle authentication errors, provide fallback authentication options, or incorporate additional features to optimize the user experience.

Remember to test the Face ID authentication thoroughly to ensure its smooth operation and reliability in your app.