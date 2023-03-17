---
title: "Generate PDF from SwiftUI"
date: 2020-08-14
draft: false
description: "How to generate a pdf file and share it"
slug: "PDF Gen"

---

## Why we need to convert a SwiftUI view into PDF?

I was working on one of my project that basically generate something like a book page. One feature that i want to implement was to share this page but how?

I tried with swiftUI sharelink but i was able to share the string and not a well formatted document. So i need something that allow me to generate a PDF.


## What you need to implement

You are very lucky cause to implement this feature i made a lot of research and seems that no one has what i need... Until i found a great git repository !!

This repository contain a perfect extension for a swiftui view that works perfectly.


## Extension 

An extension allow you to add methods to existing types, in this case View typoe, to make them do things they weren’t originally designed to do. So a best practice when you implement some extension is to create a folder called Extension in your XCode project. In this folder you will add some necessary files.



## Time to work

Let's start creating a new swift file in the Extension folder, i called mine exportPDF.
Inside this file you can paste this code:

```swift
import SwiftUI

extension View{
    
    func sharePDF<Content: View> (@ViewBuilder content: @escaping () -> Content, fileName: String) {
        exportPDF(content: content, completion: { status , url in
            if let url = url, status {
                ShareSheet.instance.share(items: [url])
            } else {
                print("⚠️ Failed to make PDF")
            }
        }, fileName: fileName)
    }
    
    // MARK: Extracting View's Height and width with the Help of Hosting Controller and ScrollView
    fileprivate func convertToScrollView<Content: View>(@ViewBuilder content: @escaping ()->Content)->UIScrollView{
        
        let scrollView = UIScrollView()
        
        // MARK: Converting SwiftUI View to UIKit View
        let hostingController = UIHostingController(rootView: content()).view!
        hostingController.translatesAutoresizingMaskIntoConstraints = false
        
        // MARK: Constraints
        let constraints = [
        
            hostingController.leadingAnchor.constraint(equalTo: scrollView.leadingAnchor),
            hostingController.trailingAnchor.constraint(equalTo: scrollView.trailingAnchor),
            hostingController.topAnchor.constraint(equalTo: scrollView.topAnchor),
            hostingController.bottomAnchor.constraint(equalTo: scrollView.bottomAnchor),
            
            // Width Anchor
            hostingController.widthAnchor.constraint(equalToConstant: screenBounds().width)
        ]
        scrollView.addSubview(hostingController)
        scrollView.addConstraints(constraints)
        scrollView.layoutIfNeeded()
        
        return scrollView
    }
    
    // MARK: Export to PDF
    // MARK: Completion Handler will Send Status and URL
    fileprivate func exportPDF<Content: View>(@ViewBuilder content: @escaping ()->Content,completion: @escaping (Bool,URL?)->(), fileName: String){
        
        // MARK: Temp URL
        let documentDirectory = FileManager.default.urls(for: .cachesDirectory, in: .userDomainMask).first!
        // MARK: To Generate New File when ever its generated
        let outputFileURL = documentDirectory.appendingPathComponent("\(fileName)\(UUID().uuidString).pdf")
        
        // MARK: PDF View
        let pdfView = convertToScrollView {
            content()
        }
        pdfView.tag = 1009
        let size = pdfView.contentSize
        // Removing Safe Area Top Value
        pdfView.frame = CGRect(x: 0, y: getSafeArea().top, width: size.width, height: size.height)
        
        // MARK: Attaching to Root View and rendering the PDF
        getRootController().view.insertSubview(pdfView, at: 0)
        
        // MARK: Rendering PDF
        let renderer = UIGraphicsPDFRenderer(bounds: CGRect(x: 0, y: 0, width: size.width, height: size.height))
        
        do{
            
            try renderer.writePDF(to: outputFileURL, withActions: { context in
                
                context.beginPage()
                pdfView.layer.render(in: context.cgContext)
            })
            
            completion(true,outputFileURL)
        }
        catch{
            completion(false,nil)
            print(error.localizedDescription)
        }
        
        // Removing the added View
        getRootController().view.subviews.forEach { view in
            if view.tag == 1009{
                print("Removed")
                view.removeFromSuperview()
            }
        }
    }
    
    fileprivate func screenBounds()->CGRect{
        return UIScreen.main.bounds
    }
    
    fileprivate func getRootController()->UIViewController{
        guard let screen = UIApplication.shared.connectedScenes.first as? UIWindowScene else{
            return .init()
        }
        
        guard let root = screen.windows.first?.rootViewController else{
            return .init()
        }
        
        return root
    }
    
    fileprivate func getSafeArea()->UIEdgeInsets{
        guard let screen = UIApplication.shared.connectedScenes.first as? UIWindowScene else{
            return .zero
        }
        
        guard let safeArea = screen.windows.first?.safeAreaInsets else{
            return .zero
        }
        
        return safeArea
    }
}
```

## Share 

With the extension above we can generate the PDF but you need to create another file in order to share the document. Create a new file, i call mine sharePDF and paste into this file the next lines of code:

````swift
import Foundation
import SwiftUI

struct ShareSheet {
    
    static let instance = ShareSheet()
    
    private init() { }
    
    func share(items: [Any]) {
        let controller = UIActivityViewController(activityItems: items, applicationActivities: nil)
        UIApplication.shared.currentUIWindow()?.rootViewController?.present(controller, animated: true, completion: nil)
    }
    
}

public extension UIApplication {
    func currentUIWindow() -> UIWindow? {
        let connectedScenes = UIApplication.shared.connectedScenes
            .filter({
                $0.activationState == .foregroundActive})
            .compactMap({$0 as? UIWindowScene})
        
        let window = connectedScenes.first?
            .windows
            .first { $0.isKeyWindow }

        return window
        
    }
}
````


## Use the extension in a view

The last part is to use what you have created in the view that you want to convert and share.
What you need to to is to add a button to share the content. Look at this:

```swift
Button {
        sharePDF(content: { //THE VIEW THAT YOU WANT TO SHARE, FOR EXAMPLE USE self TO SHARE THE CURRENT VIEW }, fileName: //THE TITLE OF THE FILE YOU WANT GENERATE)
        } label: {
            Text("Share")
        }
```


## Conclusion

With this type of implmenetation you will able to generate PDF of your views and share it with basically one tap.

This is the [link](https://github.com/Noice-Anas/PDF-creator-SwiftUI.git) of the repository that i found if you have some problems with the code above