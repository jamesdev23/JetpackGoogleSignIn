# Google Sign-In in Jetpack Compose Documentation

## Overview

This documentation provides guidance on implementing Google Sign-In in a Jetpack Compose application. The provided code demonstrates the integration of Google Sign-In using the Google Sign-In API and Firebase Authentication.

## Prerequisites

Before implementing Google Sign-In, make sure you have the following:

1. A Google Cloud Platform (GCP) project with the necessary credentials.
2. Firebase project set up with Firebase Authentication.

## Implementation Steps

### 1. Add Dependencies

Ensure you have the required dependencies in your `build.gradle` file:

```gradle
implementation "androidx.activity:activity-compose:1.3.0-alpha08"
implementation "com.google.android.gms:play-services-auth:19.0.0"
implementation "com.google.firebase:firebase-auth:23.0.0"
```

### 2. Add Google Services Configuration

Add the google-services.json file to your app module. This file contains configuration details for your Firebase project.

### 3. Get Web Client ID

Ensure that you include your web client ID as well. You can obtain it from the Firebase Console by navigating to Authentication > Sign-in Method > Google > Web SDK Implementation.

### 4. Add Google Sign-In Launcher

Add Google Sign-in Launcher inside a composable:

```
@Composable
fun SignInScreen() {
    // Initialize the Google Sign-In launcher using the ActivityResultContracts
    val googleSignInLauncher =
        rememberLauncherForActivityResult(contract = ActivityResultContracts.StartActivityForResult()
    ) { result ->
        if (result.resultCode == Activity.RESULT_OK) {
            val task = GoogleSignIn.getSignedInAccountFromIntent(result.data)
            handleSignInResult(task, context)
        }
    }

    // ... (Your existing code)
}
```

### 5. Add Google Sign-in Function
Implement a function to initiate Google Sign-in:

```
fun signInWithGoogle(
    googleSignInLauncher: ManagedActivityResultLauncher<Intent, ActivityResult>,
    context: Context
) {
    val webClientID = "YOUR_WEB_CLIENT_ID"
    val googleSignInOptions = GoogleSignInOptions
        .Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
        .requestIdToken(webClientID)
        .requestEmail()
        .build()
    val googleSignInClient = GoogleSignIn.getClient(context, googleSignInOptions)
    // hack-ish way to re-select google account during sign-in
    googleSignInClient.signOut()
    val signInIntent = googleSignInClient.signInIntent
    googleSignInLauncher.launch(signInIntent)
}
```

### 6. Add Handle Sign-in Result Function
Implement the function to handle the result of Google Sign-In:

```
fun handleSignInResult(completedTask: Task<GoogleSignInAccount>, context: Context) {
    try {
        val account = completedTask.getResult(ApiException::class.java)
        firebaseAuthWithGoogle(account.idToken!!, context)
    } catch (e: ApiException) {
        toast("Error: ${e.statusCode} ${e.message}", context)
    }
}
```

### 7. Add Firebase Authentication
mplement the function for Firebase Authentication using the obtained Google Sign-In account:

```
private fun firebaseAuthWithGoogle(idToken: String?, context: Context) {
    val auth: FirebaseAuth = FirebaseAuth.getInstance()
    val credential = GoogleAuthProvider.getCredential(idToken, null)

    auth.signInWithCredential(credential)
        .addOnCompleteListener { task ->
            if (task.isSuccessful) {
                // get email from currently signed-in user
                val email = auth.currentUser?.email ?: ""
                toast("Your email: $email", context)
            } else {
                toast("Failed to sign-in", context)
            }
        }
}
```

## Screenshot
![Google Sign-In Screenshot 01](https://github.com/jamesdev23/JetpackGoogleSignIn/blob/master/googlesignin1.jpg)
![Google Sign-In Screenshot 02](https://github.com/jamesdev23/JetpackGoogleSignIn/blob/master/googlesignin02.jpg)

