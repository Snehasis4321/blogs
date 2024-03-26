# Push Local Notifications in flutter firebase 


### Setup 
- #### 📱 Android 
  - set minSdkVersion 21 in `build.gradle`.


- #### 🍎 IOS
- need todo some changes in `AppDelegate.swift` file located in `ios/Runner/AppDelegate.swift`
  ``` swift
  import UIKit
  import Flutter
  // This is required for calling FlutterLocalNotificationsPlugin.setPluginRegistrantCallback method.
  import flutter_local_notifications
  
  @UIApplicationMain
  override func application(
    _ application: UIApplication,
    didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
    // This is required to make any communication available in the action isolate.
    FlutterLocalNotificationsPlugin.setPluginRegistrantCallback { (registry) in
      GeneratedPluginRegistrant.register(with: registry)
    }
  
    ...
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }
  ```
