# See how ML Kit and ARKit play together

> Joaquin Briceno - Software Engineer

I have enjoyed playing with Hanley Weng's "CoreML-in-ARKit" project. It displays 3D labels on top of images it detects in the scene. While the on-device detection provides a fast response, I wanted to build a solution that gave you the speed of the on-device model with the accuracy you can get from a cloud-based solution. Well, that's exactly what I built with our MLKit-ARKit project. Read on to find out more about how I did it!

## How it all works

**ML Kit** for Firebase is a mobile SDK that extends Google Cloud's machine learning (ML) expertise into Android and iOS apps in a powerful yet easy-to-use package. It includes easy-to-use Base APIs and also offers the ability to bring your own custom TFLite models.

**ARKit** is Apple's framework that combines device motion tracking, camera scene capture, advanced scene processing, and display conveniences to simplify the task of building an AR experience. You can use these technologies to create many kinds of AR experiences using either the back camera or front camera of an iOS device.

In this project I'm pushing ARKit frames from the back camera into a queue. ML Kit processes these to find out the objects in that frame.

When the user taps the screen, ML Kit returns the detected label with the highest confidence. I then create a 3D bubble text and add it into the user's scene.

## How ML Kit works

ML Kit makes ML easy for all mobile developers, whether you have experience in ML or are new to the space. For those with more advanced use cases, ML Kit allows you to bring your own TFLite models, but for more common use cases, you can implement one of the easy-to-use Base APIs. These APIs cover use cases such as text recognition, image labeling, face detection and more, and are backed by models trained by Google Cloud. We'll be using image labeling in our example.

Base APIs are available in two flavors: On-device and cloud-based. The on-device APIs are free to use and run locally, while the cloud-based ones provide higher accuracy and more precise responses. Cloud-based Vision APIs are free for the first 1000/API calls and paid after that. They provide the power of full-sized models from Google's Cloud Vision APIs.

## Hybrid approach

I'm using the ML Kit on-device image labeling API to get a live feed of results while keeping our frame rate steady at 60fps. When the user taps the screen I fire up an async call to the Cloud image labeling API with the current image. When I get a response from this higher accuracy model, I update the 3D label on the fly. So while I'm continuously running the on-device API and using its result as the initial source of information, the higher accuracy Cloud API is called on-demand and its results replaces on-device label eventually.

## Which result to show?

While the on-device API is real-time with all the processing happening locally, the Cloud Vision API makes a network request to the Google Cloud backend, leveraging a larger, higher accuracy model. Given that I consider this the more precise response, in our app I replace the label provided by the on-device API with the result from Cloud Vision API when it arrives.

## Try it yourself!

1. Clone the project

```
$ git clone https://github.com/FirebaseExtended/MLKit-ARKit.git
```

2. Install the pods and open the .xcworkspace file to see the project in Xcode.

```
$ cd MLKit-ARKit
$ pod install --repo-update
$ open MLKit-ARKit.xcworkspace
```

3. To set up the Firebase ML Kit in the sample app:

Follow these instructions for adding Firebase to your app.
Make sure to specify "com.google.firebaseextended.MLKit-ARKit" as the iOS project bundle ID.
Download the GoogleService-Info.plist file generated as part of adding Firebase to your app.
In Xcode, add the GoogleService-Info.plist file to your app, next to Info.plist.
At this point, the app should work using the on-device recognition.

4. (Optional) To set up Cloud Vision API in the sample app:

Switch your Firebase project to the Blaze plan
Only Blaze-level projects can use the Cloud Vision APIs. Follow these steps to switch your project to the Blaze plan and enable pay-as-you-go billing.
Open your project in the Firebase console.
Click on the MODIFY link in the lower left corner next to the currently selected Spark plan.
Select the Blaze plan and follow the instructions in the Firebase Console to add a billing account.
★ The cloud label detection feature is still free for first 1000 uses per month. Click here to see additional pricing details.

Go to the ML Kit section of the Firebase console and enable the "Cloud Based APIs" toggle at the top.
At this point, the app should update labels with more precise results from the Cloud Vision API.