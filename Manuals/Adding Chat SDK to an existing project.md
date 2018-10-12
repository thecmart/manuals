## Adding Chat SDK to an existing Application using Firebase
###### Quick start guide - this guide is written assuming that you already have a functioning Application that you would like to integrate Chat SDK into, that you have a basic knowledge of how Android Studio works, and that the application is already connected to firebase. If this is not the case, please view the new project manual here NEED LINK!

1. In the **Project Level** `build.gradle` file, Find the section of `repositories` in `allprojects`, and add the following code inside of it:

   ```
    maven { url "http://dl.bintray.com/chat-sdk/chat-sdk-android" }
    maven { url "https://maven.google.com" }
    maven { url "https://jitpack.io" }
   ```

2. Then add this to your `dependencies` area of the same file, if it is not already there:  

    ```
    classpath 'com.google.gms:google-services:4.0.1'
    ```

3. Add the following code to the **App Level** build.gradle file, in the section  `dependencies`:

   ```
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-push:4.1.35'
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-adapter:4.1.35'
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-file-storage:4.1.35'
    implementation 'co.chatsdk.chatsdk:chat-sdk-core:4.1.35'
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-push:4.1.35'
    implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-ui:4.1.35'
   ```

5. Find the `android {    }` section of the file. Add this code inside of it, but not inside any of the other items inside of it:

   ```
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
   ```

6. Add this to the very end of the **App Level** `build.gradle` file, if you haven't already:

   ```
    apply plugin: 'com.google.gms.google-services'
   ```

6. Now you need to create a new class. Call the class "AndroidApp", or any other name you desire, and under the label Superclass, write "Application". In the body of the class, erase all text **except for the package line at the top.** Then copy this code into it:

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
        Configuration.Builder builder = new         Configuration.Builder(context);

   // Perform any configuration steps (optional)
        builder.firebaseRootPath("prod");

   // Initialize the Chat SDK
        try {
            ChatSDK.initialize(builder.build(), new    BaseInterfaceAdapter(context), new FirebaseNetworkAdapter());
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

8. Open your `AndroidManifest.xml` file, and add this line to the`<application` section: `android:name=".AndroidApp"`. If you gave the AndroidApp class a different name, enter that name instead.

9. Now, depending on the configuration of your project, there are three options to login with the Chat SDK, pick one of them and proceed with the instructions below:

  A. **Running Chat SDK from your app when you are already logged in**                                                      B. **Login using Chat SDK Login Screen**
  C. **Login using Firebase UI**
  D. **Login using custom UI i.e. if you have an existing Firebase project**

  For the option of logging in with either Facebook, Twitter or Google, please see the section for this below.

#### Run Chat SDK

If you would like to launch Chat SDK from your app, run this line: ```ChatSDK.ui().startMainActivity```

####Login using Chat SDK Login Screen

If you wish to log in using the Chat SDK Login Screen, add this code to your `AndroidManifest.xml` file:

   ```
<activity android:name="co.chatsdk.ui.login.LoginActivity">
   <intent-filter>
​       <action android:name="android.intent.action.MAIN" />
​       <category android:name="android.intent.category.LAUNCHER" />
   </intent-filter>
</activity>
   ```
   Alternatively, the Chat SDK login screen can be triggered by this line:

       InterfaceManager.shared().a.startLoginActivity(context, true);


####Login using Firebase UI

##### Add the library

Add the following to the end of your `onCreate` method in your main class (previously termed "AndroidApp"):

```
FirebaseUIModule.activate(context, EmailAuthProvider.PROVIDER_ID, PhoneAuthProvider.PROVIDER_ID);
```

Add this in the list of import lines at the top of the AndroidApp (or equivalent) class.

   ```
import com.google.firebase.auth.EmailAuthProvider;
import com.google.firebase.auth.PhoneAuthProvider;
   ```

Add this to your `AndroidManifest.xml` in place of whichever sign in activity you were previously using.

   ```
<activity android:name="co.chatsdk.firebase.ui.SplashScreenActivity">
​    <intent-filter>
​        <action android:name="android.intent.action.MAIN" />
​        <category android:name="android.intent.category.LAUNCHER" />
​    </intent-filter>
</activity>
   ```

**VERY IMPORTANT!**

**Make sure that when you are asked by google if you wish to save the password, you select the option for it to save the password! Otherwise the firebase UI Login will not work!**

You can provide a list of providers as outlined in the [Firebase documentation](https://github.com/firebase/FirebaseUI-Android/blob/master/auth/README.md#sign-in-examples). 

> **Note**
> You will need to remove the `com.facebook.sdk.ApplicationId` meta data from the app manifest or you will get a Gradle build error. 

####Login using custom UI i.e.. if you have an existing Firebase project

Add the following to the end of your `onCreate` method in your main class (previously termed "AndroidApp")

   ```
  ChatSDK.auth().authenticateWithCachedToken().subscribe(() -> {
​      // Success
  }, throwable -> {
​      // Error
  });
   ```
####Firebase

10. Now you can go to to the [Firebase console](https://console.firebase.google.com/)  in your web browser, and you should find your project. It should be a large white tile with the name of your app. Click on your project, then go to the firebase dashboard, and go to Authentication -> Sign-In-Method, and click on whichever sign in options you like. We recommend clicking only on the "Sign in with Email and Password" option, or further steps will become more complicated.

11. Get the push token. Go to the [Firebase Console](https://console.firebase.google.com) click **your project** and then the **Settings** button. Click the **Cloud Messaging** tab. Copy the **Server Key**.

12. Add the following to the end of your `onCreate` method in your main class (previously termed "AndroidApp"):

   ```
builder.firebaseCloudMessagingServerKey("YOUR FIREBASE CLOUD MESSAGING KEY");
   ```

13. Now click on the button called "Sync Project with Gradle Files". It should be at the top left hand corner, 5 buttons from the google account button. When the gradle sync completes, your App is ready to go!

### Conclusion

Congratulations! 🎉🎉 You've just turned your app into a fully featured instant messenger! Keep reading below to learn how to further customize the Chat SDK as well as add various other modules as needed.

## Social Login

In case you would like to have the option of logging in to your app with Facebook, Twitter or Google, follow these steps. Add the following to your app level `build.gradle` file.

##### Add the library

*Gradle*

   ```
implementation 'co.chatsdk.chatsdk:chat-sdk-firebase-social-login:4.1.35'
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

Now find the heading of whichever login option(s) you would like and follow the instructions.

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

The Chat SDK has a number of additional modules that can easily be installed including:

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

   Replace ´ContactBook` with the name of the specific module you would like to activate.

   ```

   ```