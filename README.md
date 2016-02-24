This is an example project for implementing the Unity Ads SDK in a SpriteKit game using Swift

<i>Planet illustration provided by NASA - Original image by ESA/Hubble (M. Kornmesser) - https://www.spacetelescope.org/</i>

# How to integrate Unity Ads into your Swift project

### Import the Unity Ads Framework

Download the Unity SDK from https://github.com/Applifier/unity-ads-sdk
  - [Download the SDK zip file](https://github.com/Applifier/unity-ads-sdk/archive/master.zip)
  - Unzip the project, and locate **UnityAds.framework** and **UnityAds.bundle**

Import **UnityAds.framework** and **UnityAds.bundle** into your project
  - Drag and drop the files into your project's file manager
  - Select the box next to **"Copy items if needed"**

Make sure the following dependancies are enabled in your project  
  
  `CoreMedia.framework`,  `CoreTelephony.framework`,  
  
  `SystemConfiguration.framework`, `AdSupport.framework`,  
  
  `CFNetwork.framework`, `StoreKit.framework`  
  
  1. Click your project settings
  2. select **Build Phases** > **Link Binary With Library**
  3. Click the **+** button > select the Framework > Click **Add**

Add a bridging header for **UnityAds.framework**
  - Create a new file in your project called **UnityAds-Bridging-Header.h**
  - In the file, add the following line:  
  
**`#import <UnityAds/UnityAds.h>`**

### Initialize Unity Ads

Add UnityAds to your **AppDelegate**
- Open **AppDelegate.swift**
- Create a shared instance of Unity ads by adding the following code to your **AppDelegate** class  
```Swift
class AppDelegate: UIResponder, UIApplicationDelegate {
    static let unityAds = UnityAds() //Create a shared instance of Unity Ads
```

Initialize Unity Ads in your *root ViewController*
- Open your root ViewController
- In **viewDidLoad()**, add the following code to initialize the SDK
```Swift
override func viewDidLoad() {
  super.viewDidLoad()

  UnityAds.sharedInstance().delegate = self
  UnityAds.sharedInstance().setTestMode(true) //enable client-side test mode
  UnityAds.sharedInstance().startWithGameId("1003843", andViewController: self)
```
> NOTE: The game ID in the example project is **1003843**, you need to replace this number with your own game ID

Add the callback to your root ViewController  
- In your root ViewController, add the following function

```Swift
func unityAdsVideoCompleted(rewardItemKey: String!, skipped: Bool {
  if (!skipped) {
    //Provide ingame reward, give coins, report to analytics, etc...
  }
}
```
> Note: **rewardItemKey** is deprecated in the SDK; You can use custom zones to track rewards.

### Show a video ad

In the root View Controller, the following function will play a video ad

```swift
func playAd(placement: String) {
  if (UnityAds.sharedInstance().canShowZone("video")) {
    UnityAds.sharedInstance().show()
  }
}
```

To call an ad from another ViewController (including a SpriteKit or Cocos2D scene)
```swift
let vc = self.view!.window!.rootViewController as! YourRootViewController
vc.playAd("rewardedVideo")
```
