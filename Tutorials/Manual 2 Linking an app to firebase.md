### Manual 2: Linking an existing project with Firebase

###### This is the second instruction manual in our series. It assumes that you have followed our Manual 1  and have create a functioning app, or have a functioning app in any case. If not, you can follow the instructions in Manual 1 here; NEED LINK!

1. Open Android Studio and open your project. If Android Studio is not connected to Google, click on the Google sign in button located at the top left hand corner and sign in to Google. ![Buttons](https://github.com/thecmart/manuals/blob/master/Images/Buttons2.png)

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


### Conclusion

Congratulations! 🎉🎉 You've just linked your app to firebase. Please see our next manual for instructions on how to add Chat SDK to it.
