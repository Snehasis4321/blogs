# ⚠ Fix for Flutter Web  AbortError: Failed to execute 'subscribe' on 'PushManager': Subscription failed - no active Service Worker

## Show the steps how to fix this error for flutter web

### Follow this steps 
- inside web folder where index.html file is located create a new file `firebase-messaging-sw.js`
- add this code in `firebase-messaging-sw.js`

  - First import firebase app and messaging 
  
    ```js
    // import the scripts
    importScripts("https://www.gstatic.com/firebasejs/8.9.1/firebase-app.js");
    importScripts("https://www.gstatic.com/firebasejs/8.9.1/firebase-messaging.js");
    ```

  - initialize with you own app (copy this from the `firebase_options.dart` that is generated while registering the app)
    ```js
    // initialize firebase app
    firebase.initializeApp({
      apiKey: 'AIzaSyCH-Ms0H8c1a6enXXXXXXXX',                 // replace with you app
      appId: '1:280472782557:web:1067d65e8ba54XXXXXXXX',      // replace with you app
      messagingSenderId: '2804727XXXXXXXX',                   // replace with you app
      projectId: 'mon-XXXXXXXX',                              // replace with you app
      authDomain: 'mon-XXXXXXXXX.firebaseapp.com',            // replace with you app
      databaseURL: 'https://mon-XXXXXXX.firebaseio.com',      // replace with you app
      storageBucket: 'mon-XXXXXXX.appspot.com',               // replace with you app
      measurementId: 'M-YXXXXXXXXXX',                         // replace with you app
    });
   
  - add this following codes to initialize messaging and onBackgroundMessage and active service worker
    ```js
    // initialize firebase messaging
    const messaging = firebase.messaging();

    // for background message to show in console
    messaging.onBackgroundMessage((message) => {
      console.log("onBackgroundMessage", message);
    });
      
    // event on service worker activate
    self.addEventListener('activate', event => {
      event.waitUntil(self.clients.claim());
    });
      ```
- add this in `index.html` file inside `<script>` tag after  appRunner.runApp();
  ```js
  var serviceWorkerUrl = 'flutter_service_worker.js?v=' + serviceWorkerVersion;
  navigator.serviceWorker.register('/firebase-messaging-sw.js').then((registration) => {
    registration.onactivate = function(event) {
      if (event.target.state === 'activated') {
        // Your push notification code here
      }
    };
  });
  }
  ```
  **it will be like this**
  
  ![image](https://github.com/Snehasis4321/blogs/assets/96995340/f206214c-b584-4ccc-b163-a232db1e8d84)

- initializing service worker might take some more time so for that wait for some more time using before asking notification request and getting the fcm token like this
  ```dart
    // wait for sometime in web to ask for notification
  if (kIsWeb) {
    Future.delayed(Duration(seconds: 5), () {
      PushNotifications.init();
    });
  } else {
    PushNotifications.init();
  }
  ```
