### Manual 2: Linking an existing project with Firebase

###### This is the second instruction manual in our series. It assumes that you have followed our Manual 1  and have create a functioning app, or have a functioning app in any case. If not, you can follow the instructions in Manual 1 here; NEED LINK!

1. Open Android Studio and open your project. If Android Studio is not connected to Google, click on the Google sign in button located at the top left hand corner and sign in to Google. ![Buttons](https://www.dropbox.com/s/kf97cfn3tk2u05c/Buttons2.png?dl=0)

2. Go to the `dependencies ` area of the top level `build.gradle` file and add this line:

   ```
    classpath 'com.google.gms:google-services:4.2.0'
   ```

      Move your mouse over that line lines slowly, if android studio tells you that the version is outdated, enter the number of the latest version in place of the 4.2.0.

3. Add this to the very end of the app level `build.gradle` file:

   ```
    apply plugin: 'com.google.gms.google-services'
   ```

4. Click on the gradle sync button as seen in step 1.

5. Go to **Tools** -> **Firebase**, then on the Firebase tab that pops up on the right, click on **Analytics**, then on **Log an Analytics Event**, then click on the button **Connect to Firebase**. Select your region and then again click on **Connect to Firebase**. Ignore any messages claiming that it failed and click **sync** until the button turns into the word "Connected". Then remove the Firebase tab by clicking the **-** in its top righthand corner.

6. Now you can go to to the [Firebase Console](https://console.firebase.google.com/) in your web browser, (log in to google first if you must) and you should find your project. It should be a large white tile with the name of your app. Click on your project, then go to the firebase dashboard, and go to **Authentication** -> **Sign-In-Method**, and click on whichever sign in options you like. We recommend clicking only on the **Sign in with Email and Password** option, or further steps will become more complicated. Switch both Sign in switches to "On" and click **Save**.

7. Now we need to configure the app to be able to handle push notifications.

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

### Enabling location based messages

1. If you would like for your app to be able to receive messages based on the location of the user's device, then you need to activate location based messages. The Chat SDK needs two google services to support location messages. The [Google Places API](https://developers.google.com/places/) to select the location and the [Google Static Maps API](https://developers.google.com/maps/documentation/static-maps/) to display the location.

2. Go to the [Google Places API](https://developers.google.com/places/) page, click **Get Started**, then click **Places**, and then click **Continue**.

3. After this you select your Project from the drop down lit and click **Next**. Then **QUICKLY** click on **Create a billing Account** when the dialog box pops up. If you miss it, simply repeat steps 1 and 2. In order to do this you will need a billing account. If you want to do that, then continue, otherwise disable location messages by placing this text into the AndroidApp's `Oncreate` method: ```config.locationMessagesEnabled(false);``` and skip to the conclusion, otherwise follow the next steps.

4. Select your country, and accept the Terms of Service, then click **Agree and Continue**. Click **Set up payments profile** and enter your billing information. Click **Start my free trial**, then click **Next** to enable to google maps platform. Copy your API key and click **Done**.

5. Although you need to setup billing, Google give you 200 USD per month for free. So you can load 10 million free location messages for free per month.

6. Go back to Android Studio, Add this line to the `oncreate` method of the AndroidApp: `config.googleMaps("YOUR GOOGLE PLACES API KEY");`

7. Now go the `AndroidManifest.xml` file and add this line directly above `</application>`line in the file: `<meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR COPIED GOOGLE PLACES API KEY"/>`

8. Now go to [Google Static Maps API](https://developers.google.com/maps/documentation/static-maps/) and click on **Get Started**. Click on **Maps** and click **Continue**. Select the appropriate project and click **Next**, copy the API Key then click **Done**. Save this API Key in some other file as you may need it later.


### Conclusion

Congratulations! 🎉🎉 You've just linked your app to firebase. Please see our next manual for instructions on how to add Chat SDK to it.
