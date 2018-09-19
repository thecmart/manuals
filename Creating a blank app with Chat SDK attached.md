## Creating a new Project with Chat SDK pre-integrated
###### Quick start guide - it takes about 10 minutes! This instruction manual assumes that you are a beginner at using Android Studio and do not already have an Application that you would like to integrate Chat SDK into. If this is not the case, please use the existing application manual here: NEED LINK!

1. Go to **File** -> **New** -> **Project**.
2. Enter a name for the **Application** as you see fit. Change the **Company domain** if you wish as well. Be sure to note the **Application name**, the **Company domain**, and the **Package name**. You will need this information later. Click **Next**. Make sure the checkbox of Phone and Tablet is checked, then click **Next**.  Click **Add No Activity** and click **Finish**.
3. Open the top level build gradle file. You can do this by clicking on the vertical **Project** tab in the upper left hand corner, then clicking on the horizontal **Project** option in the drop down menu beside it. Click on the folder with the **name of your App**, then click on the **build.gradle** file. It should have the name of your App when you open it. Find the section of `repositories` in `allprojects`, and add the following code inside of it:

```
maven { url "http://dl.bintray.com/chat-sdk/chat-sdk-android" }
maven { url "https://maven.google.com" }
maven { url "https://jitpack.io" }
```

5. Then add this to your `dependencies` area of the same file:

```
classpath 'com.google.gms:google-services:4.0.1'
```

6. Move your mouse over that line lines slowly, if android studio tells you that the version is outdated, enter the number of the latest version in place of the 4.0.1.
7. Now go to your app level build.gradle file. Click on the **app** folder above the **build.gradle** file on the right, and then open the **build.gradle** file in it. The file should have the title "app" when you open it.
8. Add the following code to the build.gradle file, in the section  `dependencies`:

```
implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-push:4.0.26'
implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-adapter:4.0.26'
implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-file-storage:4.0.26'
implementation 'co.chatsdk.chatsdk:chat-sdk-core:4.1.26'
implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-push:4.1.26'
implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-ui:4.1.26'
implementation 'com.google.firebase:firebase-auth:16.0.3'
implementation 'com.google.android.gms:play-services-auth:16.0.0'
```

8. Move your mouse over these lines slowly, if android studio tells you that these versions are outdated, enter the number of the latest version in the appropriate line in place of the number of the latest version.

9. Find the `android {    }` section of the file. Add this code inside of it, but not inside any of the other items inside of it:

```
compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
}
```

10. Add this to the very end of the app level `build.gradle` file:

```
apply plugin: 'com.google.gms.google-services'
```

11. Now you need to create a new class. Under the **app** folder on the right, click on **src**, then on "main, and then on **java**. Under **java** there should  be a folder with the package name. Right click on it, then go to **new** and click on **Java Class**. Call the class "AndroidApp", or any other name you desire, and under the label Superclass, write "Application". In the body of the class, erase all text **except for the first line.** This would normally be `package PACKAGE NAME;`and copy this code into it:

```
import android.app.Application;
import android.content.Context;

import co.chatsdk.core.error.ChatSDKException;
import co.chatsdk.core.session.ChatSDK;
import co.chatsdk.core.session.Configuration;
import co.chatsdk.firebase.FirebaseNetworkAdapter;
import co.chatsdk.firebase.file_storage.FirebaseFileStorageModule;
import co.chatsdk.firebase.push.FirebasePushModule;
import co.chatsdk.ui.manager.BaseInterfaceAdapter;

public class AndroidApp extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        Context context = getApplicationContext();

// Create a new configuration
        Configuration.Builder builder = new Configuration.Builder(context);

// Perform any configuration steps (optional)
        builder.firebaseRootPath("prod");

// Initialize the Chat SDK
        try {
            ChatSDK.initialize(builder.build(), new BaseInterfaceAdapter(context), new FirebaseNetworkAdapter());
        }
        catch (ChatSDKException e) {
        }

// File storage is needed for profile image upload and image messages
        FirebaseFileStorageModule.activate();
        FirebasePushModule.activateForFirebase();

// Activate any other modules you need.
// ...

    }
}

```

12. If you have this class a different name than AndroidApp, you need to change the name of it in the line `public class AndroidApp extends Application` to whatever the name of the app is.

13. Open your `AndroidManifest.xml` file, it should be in the "main" folder. Add this code to the `<application` section: `android:name=".AndroidApp"`. If you gave the AndroidApp class a different name, enter that name instead.

14. At this point, you can decide which activity should be the first to launch when you launch your application. As you have not yet created any activities, this should then be the Chat SDK login activity by default. Currently, your `Android Manifest.xml` file should look something like this: 

    ```
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="PACKAGE NAME">
    
        <application
            android:name=".AndroidApp"
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="APP NAME"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/AppTheme"/>
    </manifest>
    ```

15.  On the line `android:theme="@style/AppTheme"/>` delete the `/` then click after the `>`  and hit the enter button. Now write `</application>`. Copy the code below, then click to the right of the `>` in the line `android:theme="@style/AppTheme">`, hit enter and paste the code :

```
<activity android:name="co.chatsdk.ui.login.LoginActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

16. Now add the line ```<?xml version="1.0" encoding="utf-8"?>``` At the very top of the file. The result should look like this:

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="PACKAGE NAME">

    <application
        android:name=".AndroidApp"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="APP NAME"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name="co.chatsdk.ui.login.LoginActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

17. Open your Android Studio Suite. Go to the top righthand button which should be the **blue google person**, and use it to sign in to google with your google account. If you do not have a google account, you can use the button to create one.

18. Now click on the button called **Sync Project with Gradle Files**. It should be at the top left hand corner, 5 buttons from the google account button. Ignore any messages telling you that the build failed,
19. Go to **Tools** -> **Firebase**. Go to the tab on the right, click on analytics, click on  **Log an Analytics event**, and then click **Connect to Firebase** then click on **Connect to firebase**. If there are errors, click **Connect to firebase** again and click **sync**, until the button turns into the word "Connected".

19. Now you can go to to the [Firebase Console](https://console.firebase.google.com/)  in your web browser, and you should find your project. It should be a large white tile with the name of your app. Click on your project, then go to the firebase dashboard, and go to **Authentication** -> **Sign-In-Method**, and click on whichever sign in options you like. We recommend clicking only on the **Sign in with Email and Password** option, or further steps will become more complicated. Switch both Sign in switches to "On" and click **Save**.

20. Get the push token. Click on the **Gear Button** in the top left corner, and then the **Project Settings** button. In the **General** tab, find the **Web API Key**.

21. Now go back to Android Studio. Add the following to the setup code in the AndroidApp's `onCreate` method.

```
builder.firebaseCloudMessagingServerKey("YOUR FIREBASE CLOUD MESSAGING KEY");
```

22. Now click on the button called **Sync Project with Gradle Files**. It should be at the top left hand corner, 5 buttons from the google account button. When the gradle sync completes, your App is ready to go!

### Conclusion

Congratulations! 🎉🎉 You've just turned your app into a fully featured instant messenger! Keep reading below to learn how to further customize the Chat SDK  as well as add various other modules as needed.

## Free Additional Modules

######This section is about the two free modules, the use of the Firebase UI and the Sign Up by Social Media option.

### Firebase UI

##### Add the library

Add the following to your app level `build.gradle` file:

```
implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-ui:4.0.26'
```

##### Enable the module

Add the following to the end of your `onCreate` method in your main class (previously termed "AndroidApp"):

```
FirebaseUIModule.activate(context, AuthUI.EMAIL_PROVIDER, AuthUI.PHONE_VERIFICATION_PROVIDER);
```

Add this to your `AndroidManifest.xml` in place of whichever sign in activity you were previously using.

```
<activity android:name="co.chatsdk.firebase.ui.FirebaseUIActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

You can provide a list of providers as outlined in the [Firebase documentation](https://github.com/firebase/FirebaseUI-Android/blob/master/auth/README.md#sign-in-examples). 

> **Note**
> You will need to remove the `com.facebook.sdk.ApplicationId` meta data from the app manifest or you will get a Gradle build error. 

### Social Login

Add the following to your app level `build.gradle` file.

##### Add the library

*Gradle*

```
implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-social-login:4.0.26'
```

[*Manual Import*](https://github.com/chat-sdk/chat-sdk-android#adding-modules-manually)

```
compile project(path: ':chat-sdk-firebase-social-login')
```

##### Enable the module

In your main class `onCreate` method add:

```
FirebaseSocialLoginModule.activate(getApplicationContext());
```

#### Facebook

1. On the [Facebook developer](https://developers.facebook.com/) site get the **App ID** and **App Secret**

2. Go to the [Firebase Console](https://console.firebase.google.com/) and open the **Auth** section

3. On the **Sign in method** tab, enable the **Facebook** sign-in method and specify the **App ID** and **App Secret** you got from Facebook.

4. Then, make sure your **OAuth redirect URI** (e.g. `my-app-12345.firebaseapp.com/__/auth/handler`) is listed as one of your **OAuth redirect URIs** in your Facebook app's settings page on the Facebook for Developers site in the **Product Settings > Facebook Login** config

5. Add the following to your `AndroidManifest.xml`:

   ```
   <meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_identifier"/>
   ```

   Add the following to your `chat_sdk_firebase.xml` file:

   ```
   <string name="facebook_app_identifier">[FACEBOOK APP KEY]</string>
   ```

6. Go back to the Facebook site and click "Add Platform". Choose Android and enter your **Bundle ID**. Then you will need to enter add the **Key Hashes** property. To do this first generate a [key store](https://developer.android.com/studio/publish/app-signing.html) for your app. Then generate the hash by running the following on MacOS:

   ```
   keytool -exportcert -alias <RELEASE_KEY_ALIAS> -keystore <RELEASE_KEY_PATH> | openssl sha1 -  binary | openssl base64
   ```

   On Windows, use:

   ```
   keytool -exportcert -alias <RELEASE_KEY_ALIAS> -keystore <RELEASE_KEY_PATH> | openssl sha1 -binary | openssl base64
   ```

#### Twitter

1. [Register your app](https://apps.twitter.com/) as a developer application on Twitter and get your app's **API Key** and **API Secret**.

2. In the [Firebase console](https://console.firebase.google.com/), open the **Auth** section.

3. On the **Sign in method** tab, enable the **Twitter** sign-in method and specify the **API Key** and **API Secret** you got from Twitter.

4. Then, make sure your Firebase **OAuth redirect URI** (e.g. `my-app-12345.firebaseapp.com/__/auth/handler`) is set as your **Callback URL** in your app's settings page on your [Twitter app's config](https://apps.twitter.com/).

5. Add the following to your `AndroidManifest.xml`:

   ```
   <meta-data android:name="twitter_key" android:value="@string/twitter_key" />
   <meta-data android:name="twitter_secret" android:value="@string/twitter_secret" />
   ```

   Add the following to your `chat_sdk_firebase.xml` file:

   ```
   <string name="twitter_key">[TWITTER KEY]</string>
   <string name="twitter_secret">[TWITTER SECRET]</string>
   ```

#### Google

1. If you haven't yet specified your app's SHA-1 fingerprint, do so from the [Settings page](https://console.firebase.google.com/project/_/settings/general/) of the Firebase console. See [Authenticating Your Client](https://developers.google.com/android/guides/client-auth) for details on how to get your app's SHA-1 fingerprint.

   ```
   keytool -exportcert -alias [KEY ALIAS] -keystore [PATH/TO/KEYSTORE] -list -v  
   ```

> **Note:**
>
> > You may need to add multiple keys for debug and release

1. In the [Firebase console](https://console.firebase.google.com/), open the **Auth** section.

2. On the **Sign in method** tab, enable the **Google** sign-in method and click **Save**.

3. You must pass your [server's client ID](https://developers.google.com/identity/sign-in/android/start-integrating#get_your_backend_servers_oauth_20_client_id) to the requestIdToken method. To find the OAuth 2.0 client ID.

4. Open the [Credentials page](https://console.developers.google.com/apis/credentials) in the API Console.

5. The **Web application type** client ID is your backend server's OAuth 2.0 client ID.

6. Add the following to your `AndroidManifest.xml`:

   ```
   <meta-data android:name="google_web_client_id" android:value="@string/google_web_client_id" />
   ```

   Add the following to your `chat_sdk_firebase.xml` file:

   ```
   <string name="google_web_client_id">[CLIENT ID]</string>
   
   ```

Social login can also be enabled or disabled by changing the Chat SDK [configuration](https://github.com/chat-sdk/chat-sdk-android#configuration).

## Other Additional Modules

######The Chat SDK has a number of additional modules that can easily be installed including:

- [Keyboard overlay](http://chatsdk.co/downloads/keyboard-overlay/)
- [Typing indicator](http://chatsdk.co/downloads/typing-indicator/)
- [Read receipts](http://chatsdk.co/downloads/read-receipts/)
- [Location based chat](http://chatsdk.co/downloads/location-based-chat/)
- [Audio messages](http://chatsdk.co/downloads/audio-messages/)
- [Video messages](http://chatsdk.co/downloads/video-messages/)
- [Sticker messages](https://chatsdk.co/downloads/sticker-messages/)
- [Contact book integration](https://chatsdk.co/downloads/contact-book-integration/)

After you have purchased the module you will be provided with a link to the module source code. Unzip this file and import it into Android Studio.

1. Click **File** -> **New** -> **Import Module**

2. Add the module to your `build.gradle`

   ```
   compile project(path: ':chat_sdk_[module name]')
   ```

3. Sync Gradle

4. In your main class `onCreate` activate the module:

   ```
   ContactBookModule.activateForFirebase();
   ```

   or 

   ```
   ContactBookModule.activateForXMPP();
   ```

