### Manual 3: Adding Chat SDK to your project

###### This is the third instruction manual in our series. It assumes that you have followed our [Manual 1](https://github.com/thecmart/manuals/blob/master/Tutorials/Manual%201%20Creating%20a%20new%20app%20with%20an%20empty%20activity%20and%20AppObj.md) and [Manual 2](https://github.com/thecmart/manuals/blob/master/Tutorials/Manual%202%20Linking%20an%20app%20to%20firebase.md), and have create a functioning app, or have a functioning app in any case. 

1. Open Android Studio and open your project.

2. Find the section of `repositories` in `allprojects`, and add the following code inside of it:

   ```
    maven { url "http://dl.bintray.com/chat-sdk/chat-sdk-android" }
   ```

3. Add this to the very end of the app level `build.gradle` file in the `dependencies` section:

   ```
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-adapter:4.6.0'
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-file-storage:4.6.0'
    implementation 'co.chatsdk.chatsdk:chat-sdk-core:4.6.0'
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-push:4.6.0'
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-ui:4.6.0'
    implementation 'co.chatsdk.chatsdk:chat-sdk-ui:4.6.0'
   ```

4. Move your mouse over these lines slowly, if android studio tells you that these versions are outdated, enter the number of the latest version in the appropriate line in place of the number of the latest version.

5. Now you need to create a new `Application` class if you do not already have it. Under the **app** folder on the left, click on **src**, then on **main**, and then on **java**. Under **java** there should  be a folder with the package name. Right click on it, then go to **new** and click on **Java Class**. Call the class "AppObject" and under the label `superclass`, write "Application". In the body of the class, erase all text **except for the first line.** This would normally be `package PACKAGE NAME;` 

  First we're going to override the `onCreate` method. To do that, add the following to your class:

  ```
  @Override
  public void onCreate() {
     super.onCreate();
     
  }
  ```

  The `onCreate` method is called when the app first launches and is the best place to setup the Chat SDK. 

  Then add the following code to this method:

  ```
  // The Chat SDK needs access to the application's context
  Context context = getApplicationContext();
   
  // Initialize the Chat SDK
  // Pass in 
  try {

     // The configuration object contains all the Chat SDK settings. If you want to see a full list of the settings
     // you should look inside the `Configuration` object (CMD+Click it in Android Studio) then you can see every
     // setting and the accompanying comment
     Configuration.Builder config = new Configuration.Builder(context);
   
     // Perform any configuration steps
     // The root path is an optional setting that allows you to run multiple Chat SDK instances on one Realtime database. 
     // For example, you could have one root path for "test" and another for "production"
     config.firebaseRootPath("prod");

     // Start the Chat SDK and pass in the interface adapter and network adapter. By subclassing either
     // of these classes you could modify deep functionality withing the Chat SDK 
     ChatSDK.initialize(config.build(), new BaseInterfaceAdapter(context), new FirebaseNetworkAdapter());
  }
     catch (ChatSDKException e) {
  }
   
  // File storage is needed for profile image upload and image messages
  FirebaseFileStorageModule.activate();
  ```

6. If you have not done this already, Open your `AndroidManifest.xml` file, it should be in the "main" folder. Add this code to the `<application` section: `android:name="AppObject"`. If you gave the AppObject class a different name, enter that name instead. This will tell Android which class to load up when the app starts. 

7. Add the following code to the `AndroidManifest.xml` file to launch Chat SDK upon the starting of the app:

   ```
    <activity android:name="co.chatsdk.ui.login.LoginActivity">
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>
   ```

9. Alternatively, the Chat SDK login screen can be triggered by adding this line to the end of your `onCreate` method:

   ```
   InterfaceManager.shared().a.startLoginActivity(context, true);
   ```

10. Go back to your [Firebase Console](https://console.firebase.google.com/) , click on your app, click on **Database**. Scroll down to where it says **Realtime Database** and click on **Create database**. Start in locked mode and click **Enable**. Click the **Rules** tab. Delete everything in the box, then go to this [rules.json](https://github.com/chat-sdk/chat-sdk-android/blob/master/firebase-rules.json) file, copy everything in the box (approximately 355 lines), and paste it into the box in the firebase console. Click on **Publish**.

11. Go back to the Firebase Console and click **Storage** on the left hand side. Then click **Get Started** click **Got it**. Now file storage is activated and the Chat SDK will be able to save user profile pictures and send image messages. 

12. This concludes initial setup of your project. If you would like for your app to have the ability to handle push notifications or handle location messages, please follow the instructions below.

13. Now we need to configure the app to be able to handle push notifications.

### Push Notifications

1. To handle push notifications, we use [Firebase Cloud Functions](https://firebase.google.com/docs/functions/).  This service allows you to upload a script to Firebase hosting. This  script monitors the Realtime database and whenever a new message is  detected, it sends a push notification to the recipient. Below is a summary of the steps that are required to setup push using  the Firebase Cloud Functions script. For further instructions you can  look at the [Firebase Documentation](https://firebase.google.com/docs/functions/get-started).

2. In your main class `onCreate` method add this line if it is not already present:

   ```
    FirebasePushModule.activate();
   ```

3. Install [Node.js](https://nodejs.org/). Then, in case you are using windows, run windows PowerShell, and in case you are using a mac, run the terminal app.

4. Run `firebase login` and login using the browser.

5. Make a new directory or choose an existing one to store your push functions in.

6. Navigate to that directory using the terminal.

7. Run `firebase init functions`.

8. Choose `y` to proceed.

9. Choose the correct project from the list, or select a new one if your current project is not present in the list.

10. Choose `JavaScript`.

11. Choose `y` for ESLint.

12. Choose `y` to install the node dependencies.

13. Run `firebase use --add` and pick the correct project from the list. It should be visible in the list now.

14. The alias can be any name that you like.

15. Find the `functions` directory you've just created and copy the `index.js` file from [Github](https://github.com/chat-sdk/chat-sdk-android/tree/master/FirebasePushNotifications) into the directory. Click on the `raw` version of the file, and save the page as node, then replace the node.js file that is already in the in the folder.

16. Run `firebase deploy`. You are now done using windows power shell or the terminal app. The script is now active and push notifications will be set out automatically.

17. Now click on the button called **Sync Project with Gradle Files**. It should be at the top left hand corner, 5 buttons from the google account button. When the gradle sync completes, this section will be completed. Now we need to configure your app to handle location based messages.

### Enabling location messages

1. If you would like for your app to be able to receive messages based on the location of the user's device, then you need to activate location messages. The Chat SDK needs two google services to support location messages. The [Google Places API](https://developers.google.com/places/) to select the location and the [Google Static Maps API](https://developers.google.com/maps/documentation/static-maps/) to display the location.

2. Go to the [Google Places API](https://developers.google.com/places/) page, click **Get Started**, then click **Places**, and then click **Continue**.

3. After this you select your Project from the drop down lit and click **Next**. Then **QUICKLY** click on **Create a billing Account** when the dialog box pops up. If you miss it, simply repeat steps 1 and 2. In order to do this you will need a billing account. If you want to do that, then continue, otherwise disable location messages by placing this text into the AndroidApp's `onCreate` method:

   ```
    config.locationMessagesEnabled(false);
   ```

   Then skip to the conclusion, otherwise follow the next steps.

4. Select your country, and accept the Terms of Service, then click **Agree and Continue**. Click **Set up payments profile** and enter your billing information. Click **Start my free trial**, then click **Next** to enable to google maps platform. Copy your API key and click **Done**.

5. Although you need to setup billing, Google give you 200 USD per month for free. So you can load 10 million free location messages for free per month.

6. Go back to Android Studio, Add this line to the `onCreate` method of the AndroidApp:

   ```
    config.googleMaps("YOUR GOOGLE PLACES API KEY");
   ```

7. Now go the `AndroidManifest.xml` file and add this line directly above `</application>`line in the file:

   ```
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR COPIED GOOGLE PLACES API KEY"/>
   ```

8. Now go to [Google Static Maps API](https://developers.google.com/maps/documentation/static-maps/) and click on **Get Started**. Click on **Maps** and click **Continue**. Select the appropriate project and click **Next**, copy the API Key then click **Done**. Save this API Key in some other file as you may need it later.

### Conclusion

Congratulations! 🎉🎉 You've just turned your app into a fully featured instant messenger! Keep reading our other manuals to learn how to further customize the Chat SDK as well as add various other modules as needed. Please see our next manual to see how to add a firebase UI login function to your app here: [Manual 4](https://github.com/thecmart/manuals/blob/master/Tutorials/Manual%204%20Creating%20a%20firebase%20UI%20login%20for%20you%20app.md). You can also download the project as described above here: [Project 3](https://github.com/thecmart/BlankApp/tree/Manual3)
