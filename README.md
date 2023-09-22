# @jiomeet/core_sdk_plugin

Welcome to the `@jiomeet/core_sdk_plugin` library – your gateway to seamless integration of core SDK features into your React Native project for Android. This plugin simplifies the process of launching and managing core template screens, allowing you to quickly build applications that leverage the power of the core SDK.

## What is @jiomeet/core_sdk_plugin?

The `@jiomeet/core_sdk_plugin` library is a React Native plugin designed to streamline the integration of critical core SDK functionalities into your Android application. It provides an easy-to-use interface for launching core template screens, making it effortless to create applications that involve meetings.

## Key Features

- Launch core template screens with ease.
- Join meetings seamlessly by providing meeting details.
- Receive callbacks from the SDK to handle events as needed.
- Simplify the integration of core SDK features into your Android app.

Ready to get started? Follow the steps below to integrate `@jiomeet/core_sdk_plugin` into your React Native project and unlock the potential of core SDK features.


## Getting Started

Here's how to get started with @jiomeet/core_sdk_plugin in your React Native project:

### Install the package:

Using `npm`:

```
npm install @jiomeet/core_sdk_plugin
```

Using `Yarn`:

```
yarn add @jiomeet/core_sdk_plugin
```

### Automatic Linking (if necessary)

If you're using a React Native version that doesn't automatically link native modules and assets (React Native 0.59 and later usually do this automatically), you might need to perform these additional steps:

#### For Android

1. Open a terminal and navigate to your project's root directory.

2. Run the following command to link the `@jiomeet/core_sdk_plugin` library:

   ```bash
   npx react-native link @jiomeet/core_sdk_plugin

## Usage: Launch Core Template Screen

```js
import { launchMeetingCoreTemplateUI } from '@jiomeet/core_sdk_plugin';

// ...

launchMeetingCoreTemplateUI(meetingId, meetingPin, displayName);
```

To join a meeting, enter meeting details in the function and directly call the function as mentioned above

//create a table with meetingId, meetingPin, displayName as columns

| Parameter     | Type     | Description                                                     |
|:--------------|:---------|:----------------------------------------------------------------|
| `meetingId`   | `string` | **Required**. The meeting ID of the meeting to be joined.       |
| `meetingPin`  | `string` | **Required**. The meeting PIN of the meeting to be joined.      |
| `displayName` | `string` | **Required**. The display name of the user joining the meeting. |


## Receive a callback from SDK
```js
import { DeviceEventEmitter } from 'react-native';

// ...

useEffect(() => {
  DeviceEventEmitter.addListener('call_ended', (message) => {
    // Handle the message as needed
  });
  return ()=>{
    DeviceEventEmitter.removeAllListeners('call_ended')
  }
}, []);
```
The above callback will be triggered when the Call is ended, you can handle this callback as per your need, the default message that is received is "Call Ended"

## Permissions

To ensure that the `@jiomeet/core_sdk_plugin` works seamlessly on Android devices, it requires specific permissions. These permissions are necessary to enable core SDK features like video conferencing, audio calls, and accessing device resources. Depending on the Android version your app targets, you may need to take additional steps to handle permissions properly.

### Targeting Android 11 (API level 30) or Newer

If your app targets Android 11 or a newer version, you must follow the permissions guidelines as described in the [Android documentation](https://developer.android.com/training/basics/intents/package-visibility). Specifically, you'll need to declare package visibility needs and handle permissions accordingly.

Here's a list of permissions that `@jiomeet/core_sdk_plugin` may require:

- `android.permission.CAMERA`: Required for accessing the device's camera for video calls.
- `android.permission.READ_EXTERNAL_STORAGE`: Necessary for reading files from external storage.
- `android.permission.WRITE_EXTERNAL_STORAGE`: Required for writing files to external storage.
- `android.permission.RECORD_AUDIO`: Needed for capturing audio during calls.
- `android.permission.INTERNET`: Essential for internet connectivity, especially when communicating with remote servers.
- `android.permission.ACCESS_NETWORK_STATE`: Allows monitoring of network connectivity state.
- `android.permission.ACCESS_WIFI_STATE`: Provides information about Wi-Fi connectivity.
- `android.permission.BLUETOOTH`: Required for Bluetooth functionality.
- `android.permission.WAKE_LOCK`: Needed to prevent the device from sleeping during active calls.
- `android.permission.ACCESS_NOTIFICATION_POLICY`: Allows access to notification settings, which might be needed for call management.
- `android.permission.EXPAND_STATUS_BAR`: Required for expanding the status bar, potentially for notifications.
- `android.permission.FOREGROUND_SERVICE`: Required to run a foreground service, such as for ongoing calls.
- `android.permission.DOWNLOAD_WITHOUT_NOTIFICATION`: Allows downloading without displaying a notification.

It's crucial to include these permissions in your AndroidManifest.xml file to ensure that your app functions correctly. You can add these permissions like so:

Android requires allowing permissions with https://facebook.github.io/react-native/docs/permissionsandroid.html
The below permission must be added to your main application's `AndroidManifest.xml`.
```xml
...
  <uses-feature
    android:name="android.hardware.camera"
    android:required="false" />
  <uses-permission android:name="android.permission.CAMERA" />
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.RECORD_AUDIO" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
  <uses-permission android:name="android.permission.BLUETOOTH" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <uses-permission android:name="android.permission.ACCESS_NOTIFICATION_POLICY" />
  <uses-permission android:name="android.permission.EXPAND_STATUS_BAR" />
  <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
  <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
...
```

### Build.gradle dependencies required

Add this below line of code to React Native app's Android project's build.gradle file, i.e. your_project/android/build.gradle inside buildscript

```diff
dependencies {
  ...
  classpath "com.google.dagger:hilt-android-gradle-plugin:2.44"
  classpath("org.jetbrains.kotlin:kotlin-gradle-plugin:1.7.20")
}
```

Add this below line of code to React Native app's Android app's build.gradle file, i.e. your_project/android/app/build.gradle

```diff
...
apply plugin: 'kotlin-kapt'
apply plugin: 'kotlin-android'
apply plugin: 'com.google.dagger.hilt.android'

...
// Allow references to generated code
kapt {
  correctErrorTypes true
}

repositories {
  ... Other repositories
  maven {
    credentials {
      username "<UserName>"
      password "<Personal Access Token>"
    }
    url "https://maven.pkg.github.com/JioMeet/JioMeetCoreTemplateSDK_ANDROID"
  }
}

dependencies {
  ...
  implementation "com.google.dagger:hilt-android:2.44"
  kapt "com.google.dagger:hilt-android-compiler:2.44"
}
```
