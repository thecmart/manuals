### Manual 4: Adding The firebase UI login to your project

###### This is the fourth instruction manual in our series. It assumes that you have followed our Manuals 1 to 3, and have create a functioning app with Chat SDK integrated. If not, you can follow the instructions in Manual 1 here; NEED LINK! Manual 2 here; NEED LINK! and Manual 3 here; NEED LINK!

1. Open Android Studio and open your project.

2. Open the `activity_main.xml` file. You do this by going into the folder `app` on the left, then `src`, then `main`, then `res`, then `layout`, and finally click on `activity_main.xml`.

3. Click on the button `Text` in the middle of the screen, then erase all the code you see, and copy the following code into it:

      ```
       <?xml version="1.0" encoding="utf-8"?>
       <android.support.constraint.ConstraintLayout
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
       </android.support.constraint.ConstraintLayout>
      ```

4. Open the folder that says ```values```, open the ```strings.xml``` file and add this to the resources:    ```<string name="login_button_text">Login With Firebase UI</string>```

5. Add this code in the `import` section of the `MainActivity` class:

         ```
          import com.firebase.ui.auth.AuthUI;
          import com.firebase.ui.auth.ErrorCodes;
          import com.firebase.ui.auth.IdpResponse;
          import com.google.firebase.FirebaseApp;
         ```

6. Next, add this line below the line: `public class MainActivity extends AppCompatActivity {`

         ```
          Button signInButton;
          public static final int RC_SIGN_IN = 900;
          protected ProgressDialog progressDialog;.
         ```

7. Then add these two lines inside of the `onCreate` method: 

         ```
          FirebaseApp.initializeApp(this);
          signInButton = (Button) findViewById(R.id.button);
         ```

8. Now enter this code outside of the `onCreate` method:

         ```
              @Override
              protected void onResume() {
                  super.onResume();
                  authenticateWithCachedToken();
              }
          
              protected void authenticateWithCachedToken () {
                      showProgressDialog(getString(chatsdk.co.chat_sdk_firebase_ui.R.string.a uthenticating));
                  signInButton.setEnabled(false);
                  ChatSDK.auth().authenticateWithCachedToken()
                          .observeOn(AndroidSchedulers.mainThread())
                          .doFinally(() -> {
                              signInButton.setEnabled(true);
                              //dismissProgressDialog();
                          })
                          .subscribe(() -> {
                              ChatSDK.ui().startMainActivity(MainActivity.this);
                          }, throwable -> {
          //                    startAuthenticationActivity();
                          });
              }
          
              protected void onActivityResult(int requestCode, int resultCode, Intent data) {
                  super.onActivityResult(requestCode, resultCode, data);
                  // RC_SIGN_IN is the request code you passed into  startActivityForResult(...) when starting the sign in flow.
                  if (requestCode == RC_SIGN_IN) {
                      IdpResponse response = IdpResponse.fromResultIntent(data);
          
                      // Successfully signed in
                      if (resultCode == RESULT_OK) {
                          return;
                      }
                      else {
          
                          // Sign in failed
                          if (response == null) {
                              // User pressed back button
                              ToastHelper.show(this, chatsdk.co.chat_sdk_firebase_ui.R.string.sign_in_cancelled);
                              return;
                          }
          
                          if (response.getError().getErrorCode() == ErrorCodes.NO_NETWORK) {
                              ToastHelper.show(this, chatsdk.co.chat_sdk_firebase_ui.R.string.no_internet_connection);
                              return;
                          }
          
                          if (response.getError().getErrorCode() == ErrorCodes.UNKNOWN_ERROR) {
                              ToastHelper.show(this, chatsdk.co.chat_sdk_firebase_ui.R.string.unknown_error);
                              return;
                          }
                      }
                      ToastHelper.show(this, chatsdk.co.chat_sdk_firebase_ui.R.string.unknown_sign_in_response);
          //            finish();
                  }
              }
          
              /** Show a SuperToast with the given text. */
              protected void showToast(String text){
                  if (StringUtils.isEmpty(text))
                      return;
          
                  ToastHelper.show(this, text);
              }
          
              protected void showProgressDialog(String message) {
                  if (progressDialog == null) {
                      progressDialog = new ProgressDialog(this);
                  }
          
                  if (!progressDialog.isShowing()) {
                      progressDialog = new ProgressDialog(this);
                      progressDialog.setMessage(message);
                      progressDialog.show();
                  }
              }
          
              protected void showOrUpdateProgressDialog(String message) {
                  if (progressDialog == null) {
                      progressDialog = new ProgressDialog(this);
                  }
          
                  if (!progressDialog.isShowing()) {
                      progressDialog = new ProgressDialog(this);
                      progressDialog.setMessage(message);
                      progressDialog.show();
                  } else progressDialog.setMessage(message);
              }
          
              protected void dismissProgressDialog() {
                  // For handling orientation changed.
                  try {
                      if (progressDialog != null && progressDialog.isShowing()) {
                          progressDialog.dismiss();
                      }
                  } catch (Exception e) {
                      ChatSDK.logError(e);
                  }
              }
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
         
          public void login_click (View v) {
             startAuthenticationActivity();
          }
         ```

9. Click on the Gradle Sync button.


### Conclusion

Congratulations! 🎉🎉 You've just created you first App with Firebase UI Login. Keep reading below to learn how to further customize the Chat SDK as well as add various other modules as needed.
