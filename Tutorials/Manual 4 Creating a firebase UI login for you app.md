### Manual 4: Adding The firebase UI login to your project

###### This is the fourth instruction manual in our series. It assumes that you have followed our Manuals 1 to 3, and have create a functioning app with Chat SDK integrated. If not, you can follow the instructions [Manual 1](https://github.com/thecmart/manuals/blob/master/Tutorials/Manual%201%20Creating%20a%20new%20app%20with%20an%20empty%20activity%20and%20AppObj.md), [Manual 2](https://github.com/thecmart/manuals/blob/master/Tutorials/Manual%202%20Linking%20an%20app%20to%20firebase.md) and Manual 3 and [Manual 3](https://github.com/thecmart/manuals/blob/master/Tutorials/Manual%203%20Integrating%20ChatSDK%20into%20the%20new%20project.md)

1. Open Android Studio and open your project.

2. Open the `activity_main.xml` file. You do this by going into the folder `app` on the left, then `src`, then `main`, then `res`, then `layout`, and finally click on `activity_main.xml`.

3. Click on the button `Text` in the middle of the screen, then erase all the code you see, and copy the following code into it:

   ```
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:app="http://schemas.android.com/apk/res-auto"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      tools:context=".MainActivity">
    
      <Button
          android:id="@+id/button"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_marginStart="8dp"
          android:layout_marginTop="8dp"
          android:layout_marginEnd="8dp"
          android:layout_marginBottom="8dp"
          android:onClick="login_click"
          android:text="@string/login_button_text"
          app:layout_constraintBottom_toBottomOf="parent"
          app:layout_constraintEnd_toEndOf="parent"
          app:layout_constraintStart_toStartOf="parent"
          app:layout_constraintTop_toTopOf="parent" />
    </LinearLayout>
   ```
   
   This code will add a button to the Activity. When we click the button, we will start the Firebase UI login process. 

4. Open the folder that says `values`, open the `strings.xml` file and add this to the resources: `<string name="login_button_text">Login With Firebase UI</string>`

5. Add this code in the `import` section of the `MainActivity` class:

   ```
   import com.firebase.ui.auth.AuthUI;
   import com.firebase.ui.auth.ErrorCodes;
   import com.firebase.ui.auth.IdpResponse;
   import com.google.firebase.FirebaseApp;
   import android.view.View;
   ```

6. Next, add this line below the line: `public class MainActivity extends AppCompatActivity {`

   ```
   Button signInButton;
   public static final int RC_SIGN_IN = 900;
   protected ProgressDialog progressDialog;
   ```
   
   Here we setup the properties we will need to use later on. 

7. Then add these two lines inside of the `onCreate` method: 

   ```
   FirebaseApp.initializeApp(this);
   signInButton = (Button) findViewById(R.id.button);
   ```
   
   In the first line we are starting up Firebase and in the second line we are finding the button we defined in the `xml` file and assigning that button to the `signInButton` variable we created earlier. 

8. Now we're going to actually use Firebase UI to authenticate with Firebase. Add the following function: 

   ```
   public void startAuthenticationActivity () {
   
      ArrayList<AuthUI.IdpConfig> idps = new ArrayList<>();
            
      idps.add(new AuthUI.IdpConfig.EmailBuilder().build());
            
      startActivityForResult(
             AuthUI.getInstance()
                     .createSignInIntentBuilder()
                     .setAvailableProviders(idps)
                     .build(),
             RC_SIGN_IN);
    }
    ```
   
    In this function we are first defining a list of the sign in methods we want Firebase UI to provide. Then we pass this list to the Firebase Auth UI object to build an activity that will allow us to sign in. Then we launch that activity. 
   
9. Now we need to handle that activity when it returns:

   ```
   protected void onActivityResult(int requestCode, int resultCode, Intent data) {
      super.onActivityResult(requestCode, resultCode, data);
     
      // RC_SIGN_IN is the request code you passed into  startActivityForResult(...) when starting the sign in flow.
      if (requestCode == RC_SIGN_IN) {
         IdpResponse response = IdpResponse.fromResultIntent(data);
   
         // Successfully signed in
         if (resultCode == RESULT_OK) {
            // Success
         }
         else {
            // Handle Error
         }
      }
   }
   ```
  
10. Now we're going to call the authentication method when the user clicks the button:
  
   ```
   public void login_click (View v) {
      startAuthenticationActivity();
   }
   ```
  
11. Conceptually two steps are involved with starting the Chat SDK. The first step is to authenticate with Firebase. The second step is for the Chat SDK to connect to Firebase and initialize the messenger. To do this, we can call the `authenticate` method. 

   ```
   @Override
   protected void onResume() {
      super.onResume();
      authenticateWithCachedToken();
   }
    
   protected void authenticateWithCachedToken () {
      signInButton.setEnabled(false);
      
      ChatSDK.auth().authenticate()
              .observeOn(AndroidSchedulers.mainThread())
              .doFinally(() -> {
                  signInButton.setEnabled(true);
              })
              .subscribe(() -> {
                  ChatSDK.ui().startMainActivity(MainActivity.this);
              }, throwable -> {
                  // Setup failed
              });
      }
   ```
  
   This method will be called whenever the `MainActivity` resumes. It will check to see if we're authenticated with Firebase and if so, it will try to connect the Chat SDK to Firebase. If not, the fail block will be called. If it succeeds, it will launch the Chat SDK main activity. 

12. Click on the Gradle Sync button.

### Conclusion

Congratulations! 🎉🎉 You've just created you first App with Firebase UI Login. Please read our other manuals to learn how to further customize the Chat SDK as well as add various other modules as needed here: [Modules](https://github.com/chat-sdk/chat-sdk-android#module-setup)
