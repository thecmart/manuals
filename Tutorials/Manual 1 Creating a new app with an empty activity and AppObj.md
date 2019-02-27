### Creating a new empty Project

###### Quick start guide - it takes about 10 minutes! This instruction manual assumes that you are a beginner at using Android Studio and assumes that you want to add Chat SDK to a blank Android Studio project. If you are an advanced user or if you want to add the Chat SDK to an existing project, please use the manual here; NEED LINK!

1. In Android Studio, Go to **File** -> **New** -> **Project**.

2. Click on the **Phone and Tablet** tab. Click on the **Add Empty Activity** tab and click **Next**. Enter a name for the **Application** as you see fit. Change the **Name** of the application as you see fit. Be sure to note the **Application name** and the **Package name**. You will need this information later. Set the minimum API level to 16. Check the box of the Android X option, then click **Finish**.

3. Open the top level `build.gradle` file. You can do this by clicking on the vertical **Project** tab in the upper left hand corner, then clicking on the horizontal **Project** option in the drop down menu beside it. ![Project and Project](https://ucf4ee6a431be24ebd743771d2a3.previews.dropboxusercontent.com/p/thumb/AAVFGLFmMG8sGM7s5ko6n2TtpUJIcEWglBHeJk4l7RZnR0v78uKHgSKZBIklKcZITXpe-D_OheeoCkiYAfWeSUqENJzvDWYv0X3ZvKH10rTh8RpHFCKxJs5zRGzKlBasCZQknK7foa-ivCAwJkOfRcLWgMwzmeqbL66hr2T4SOrAqxOgN91ftPRM1nKe43f6A5yyBs8aVCZD9E3_7-4ISfqx3TickAcVf3nmZM6gueNuVYct-v2vGJMvMr_nTWs2MWqi2hEzV1s5zVLpvi63pJYK/p.png?size_mode=5)

4. Click on the folder with the **name of your App**, then click on the `build.gradle` file. When you open it, the tab should have the name of your App. That’s how you know it’s the project level `build.gradle` file. It should have the name of your App when you open it. ![Top Level Build Gradle File](https://previews.dropbox.com/p/thumb/AAU6ESGQIV87ptt6cFUIVFS__l01b-23kwPapAkWknzRvaaj7xIg4PKJLg8KuMJuJ-hEmQvKiIvkRGmwJRjeOlHEZi2RS00nwi4413b9XNGkrrXFH6n1U1dZSugvBmUoPoeP4rJoo53LeQZH8JkpiCNwg4Vu32UCoOqvGDc8O-0KpGX6eicPK7Y6YdjA7f11NBaSI8HW0Xh_RfwoD-WzHP0si_mawn6Ma8a8JEPSY5CbWA/p.png?size_mode=5)

5. Find the section of `repositories` in `allprojects`, and add the following code inside of it:

   ```
    maven { url "https://maven.google.com" }
    maven { url "https://jitpack.io" }
   ```

   The result should look like this:

   ```
    allprojects {
      repositories {
        google()
        jcenter()
        maven { url "https://maven.google.com" }
        maven { url "https://jitpack.io" }
      }
    }
   ```

6. Now go to your app level `build.gradle` file. Click on the **app** folder above the ``build.gradle`` file on the right, and then open the `build.gradle` file in it. The file should have the title "app" when you open it.

7. Find the following lines of code in the `dependencies` section: 

   ```
    implementation 'androidx.appcompat:appcompat:1.0.0-beta01'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.2'
    androidTestImplementation 'androidx.test:runner:1.1.0-alpha4'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.1.0-alpha4'
   ```

   Move your mouse over them and update them to their most recent versions if needs be. These currently are:

   ```
    implementation 'androidx.appcompat:appcompat:1.1.0-alpha02'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    androidTestImplementation 'androidx.test:runner:1.1.2-alpha02'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.1.2-alpha01'
   ```

8. Find the `android {    }` section of the file. Add this code inside of it, but not inside any of the other items inside of it:

   ```
    compileOptions {
      sourceCompatibility JavaVersion.VERSION_1_8
      targetCompatibility JavaVersion.VERSION_1_8
    }
   ```

   It should then look like this :

   ```
    android {
    android {
    compileSdkVersion 27
    defaultConfig {
        applicationId "domain.testing.testapp2"
        minSdkVersion 21
        targetSdkVersion 27
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner             "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
     }
    }
   ```

9. Now you need to create a new class. Under the **app** folder on the left, click on **src**, then on "main, and then on **java**. Under **java** there should  be a folder with the package name. Right click on it, then go to **new** and click on **Java Class**. Call the class "AppObject" and under the label Superclass, write "Application". In the body of the class, erase all text **except for the first line.** This would normally be `package PACKAGE NAME;`and copy this code into it:

   ```
    import android.app.Application;
   
    public class AppObject extends Application {
   
     @Override
     public void onCreate() {
        super.onCreate();
     }
    }
   ```

10. If you have this class a different name than AppObject, you need to change the name of it in the line `public class AppObject extends Application` to whatever the name of the app is.

11. Open your `AndroidManifest.xml` file, it should be in the "main" folder. Add this code to the `<application` section: ```android:name=".AppObject"```

   If you gave the AppObject class a different name, enter that name instead.

12. Currently, your `Android Manifest.xml` file should look something like this: 

   ```
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.emptyapplication">
    
    <application
        android:name=".AppObject"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
    
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
    
    </manifest>
   ```

13. Now click on the button called **Sync Project with Gradle Files**. It should be at the top right hand corner, 5 buttons from the google account button. Ignore any messages telling you that the build failed.![Buttons](https://previews.dropbox.com/p/thumb/AAWsTz-wgsIUocl-0_qIHSGw9E9kMva8RS43IFS2i8ntyUCEjYrYpk35V6N9B53rMsTAfGy0kJSdAp21VcBVa2lzWEskvCO9s19elzORDS9KwmCLjPbmc1oDKm_DJTm-_Ko4Klud7PcfZLd3BtBNCppCaf7-6U593L32oOVqXnqBwHRp4C6AbbsuoLD-ygkhEpq6_8O3I-BAGo-facibj_hl7zelZ2BsHkde6TQwc_kJ1A/p.png?size_mode=5)
14. You can now run your app. You can either plug your smartphone into your computer, or choose to use an emulator. Click on **Run** -> **Run App** and choose either an emulator or your plugged in phone. If you have neither you can download an emulator by going to **Tools** -> **AVD Manager**. Pick a device and click **OK**. Ignore any messages you get containing warnings. When the screen shown below appears, your app is now running.

   ![Running App](https://previews.dropbox.com/p/thumb/AAX7TrueXa6qtiL3LR7vNvWYQDU8OwX7YUMiqq8CaHmv2ABg8zU5pDhNEBNonBq_Yh9ZT9uMJgHMk76MO4xVF-B1yx0Hs5wZ2N2IuRAc54pjtRpQwXS4F_1dgS09TD_ZZe_YOhpo3BDzc-TMIo3NHptzPMdnzhQc8K9D9Y2DJ0s_j7QIetYdQ_bzNYGbPitgJg50N7S-HHRuh9J0Q3btuRtqsJmMkhchFiNJtVswlQDN6Q/p.png?size_mode=5)

### Conclusion

Congratulations! ???? You've just created you first App. Please see our next manual for instructions on how to add Chat SDK to it.