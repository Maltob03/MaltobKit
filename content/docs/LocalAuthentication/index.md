---
title: "FaceID"
date: 2020-08-14
draft: false
description: "All the configuration variables available in Blowfish."
slug: "FaceID"

---


## How to implement FaceID in your app

In order to perform an authentication using FaceID you can use the LocalAuthentication framework. It's very easy so follow the steps and enjoy !

## Import the framework

The first thing that you have to do is import the LocalAuthentication framework as:

```
import LocalAutentication
```

## Boolean Variable

In your view you have to declare a boolean variable in this way

```
 @State private var isUnlocked = false
```

The state wrapper allow the variable to change his value when the view is esecuted

## How the view works

We have to display two view item, one when the authentication doesn't confirm the user and another one when the authenticaiton works, like this:

```
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

as you can see there is **.onAppear(perform: authenticate)**

This mean that when the view is loaded the first thing that is done is function **authenticate** that we define like this:

```
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