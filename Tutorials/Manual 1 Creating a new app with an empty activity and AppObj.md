### Creating a new empty Project

###### Quick start guide - it takes about 10 minutes! This instruction manual assumes that you are a beginner at using Android Studio and assumes that you want to create a new empty project. If you are an advanced user and want to add Chat SDK to an existing project, please view our manual 3 here: [Manual 3](https://github.com/thecmart/manuals/blob/master/Tutorials/Manual%203%20Integrating%20ChatSDK%20into%20the%20new%20project.md). The corresponding video to this manual is here: [Tutorial 1](https://www.youtube.com/watch?v=AwhxFx8CXCg)

1. In Android Studio, go to **File** -> **New** -> **Project**.

2. Click on the **Phone and Tablet** tab. Click on the **Add Empty Activity** tab and click **Next**. Enter a name for the **Application** as you see fit. Change the **Name** of the application as you see fit. Be sure to note the **Application name** and the **Package name**. You will need this information later. Set the minimum API level to 16. Check the box of the Android X option, then click **Finish**.

3. Open the top level `build.gradle` file. You can do this by clicking on the vertical **Project** tab in the upper left hand corner, then clicking on the horizontal **Project** option in the drop down menu beside it. ![Project and Project](https://github.com/thecmart/manuals/blob/master/Images/Project%20and%20Project.png)

4. Click on the folder with the **name of your App**, then click on the `build.gradle` file. When you open it, the tab should have the name of your App. That’s how you know it’s the project level `build.gradle` file. It should have the name of your App when you open it. ![Top Level Build Gradle File](https://github.com/thecmart/manuals/blob/master/Images/Top%20Level%20Build%20Gradle%20File.png)

5. Find the section of `repositories` in `allprojects`, and add the following code inside of it:

   ```
   maven { url "https://maven.google.com" }
   maven { url "https://jitpack.io" }
   ```

   These are links to the necessary libraries for the app to work. The result should look like this:

   ```
   allprojects {
       repositories {
           google ()
           jcenter ()
           maven { url "https://maven.google.com" }
           maven { url "https://jitpack.io" }
       }
   }
   ```

6. Now go to your app level `build.gradle` file. Click on the **app** folder above the `build.gradle` file on the right, and then open the `build.gradle` file in it. The file should have the title "app" when you open it.

7. There are implementations which are needed for android X to work. Find the following lines of code in the `dependencies` section:

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

8. Find the `android {    }` section of the file. Add this code inside of it, but not inside any of the other items inside of it. This will make the app more flexible and allow it to run on a larger variety of devices:

   ```
   compileOptions {
       sourceCompatibility JavaVersion.VERSION_1_8
       targetCompatibility JavaVersion.VERSION_1_8
   }
   ```

   It should then look like this:

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
               testInstrumentationRunner
               "android.support.test.runner.AndroidJUnitRunner"
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
   }
   ```

9. Now you need to create a new class. Under the **app** folder on the left, click on **src**, then on "main, and then on **java**. Under **java** there should  be a folder with the package name. Right click on it, then go to **new** and click on **Java Class**. Call the class "AppObject" and under the label Superclass, write "Application". In the body of the class, erase all text **except for the first line.** This would normally be `package PACKAGE NAME;`and copy the following code into it. This is basic code for the functioning of the class.

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

11. Now you need to declare the existence of this class in the manifest. Open your `AndroidManifest.xml` file, it should be in the "main" folder. Add this code to the `<application` section:

    ```
    android:name=".AppObject"
    ```

    If you gave the `AppObject` class a different name, enter that name instead.

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


13. Now click on the button called **Sync Project with Gradle Files**. It should be at the top right hand corner, 5 buttons from the google account button. Ignore any messages telling you that the build failed.![Buttons](https://github.com/thecmart/manuals/blob/master/Images/Buttons2.png)

14. You can now run your app. You can either plug your smartphone into your computer, or choose to use an emulator. Click on **Run** -> **Run App** and choose either an emulator or your plugged in phone. If you have neither you can download an emulator by going to **Tools** -> **AVD Manager**. Pick a device and click **OK**. Ignore any messages you get containing warnings. When the screen shown below appears, your app is now running. ![Running App](https://github.com/thecmart/manuals/blob/master/Images/Running%20App.png)

### Conclusion

Congratulations! 🎉🎉 You've just created you first App. Please see our next manual for instructions on how to link it to Firebase here: [Manual 2](https://github.com/thecmart/manuals/blob/master/Tutorials/Manual%202%20Linking%20an%20app%20to%20firebase.md). You can also download a blank project as described above here: [Project 1](https://github.com/thecmart/BlankApp)

