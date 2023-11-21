# Google Sign-In with Jetpack Compose Documentation

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

### 2. Google Services Configuration

Add the google-services.json file to your app module. This file contains configuration details for your Firebase project.

### 3. Web Client ID

Ensure that you include your web client ID as well. You can obtain it from the Firebase Console by navigating to Authentication > Sign-in Method > Google > Web SDK Implementation.

