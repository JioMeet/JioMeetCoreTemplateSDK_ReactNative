# React Native JioMeet Core Template SDK Plugin

## Table of Contents -

1. [Introduction](#introduction)
2. [Features](#features)
3. [Prerequisites](#prerequisites)
   - [Android](#android)
     - [Install package](#install-the-package)
     - [Android Manifest](#android-manifest-changes)
     - [Add credentials](#adding-credentialsproperties-file-in-the-android-folder)
   - [iOS](#ios) 
     - [Require Configurations](#require-configurations)
     - [Info.plist Changes](#infoplist-changes)
     - [Enable Background Mode](#enable-background-mode)
4. [Setup](#setup)
5. [Usage](#usage-launch-core-template-screen)
6. [Example](#example)

## Introduction

In this documentation, we'll guide you through the process of installation, enabling you to enhance your React Native app with React Native core sdk plugin swiftly and efficiently. Let's get started on your journey to creating seamless communication experiences with React Native plugin!

---

## Features

In this Plugin , you'll find a range of powerful features designed to enhance your application's communication and collaboration capabilities. These features include:

1. **Voice and Video Calling**:Enjoy high-quality, real-time audio and video calls with your contacts.

2. **Participant Panel**: Manage and monitor participants in real-time meetings or video calls for a seamless user experience.

3. **Screen Sharing and Whiteboard Sharing**: Empower collaboration by sharing your screen or using a virtual whiteboard during meetings or video conferences.

4. **Group Conversation**: Easily engage in text-based conversations with multiple participants in one chat group.

5. **Inspect Call Health**: Monitor the quality and performance of your audio and video calls to ensure a seamless communication experience.


## Prerequisites

Before you begin, ensure you have met the following requirements:

#### Install the package:

Using `npm`:

```
npm install @jiomeet/core_sdk_plugin
```

Using `Yarn`:

```
yarn add @jiomeet/core_sdk_plugin
```

#### Automatic Linking (if necessary)

If you're using a React Native version that doesn't automatically link native modules and assets (React Native 0.59 and later usually do this automatically), you might need to perform these additional steps:

#### For Android

1. Open a terminal and navigate to your project's root directory.

2. Run the following command to link the `@jiomeet/core_sdk_plugin` library:

```bash
   npx react-native link @jiomeet/core_sdk_plugin
```

#### Android Manifest changes
To ensure proper functionality and compatibility of your Android application, please add the following parameters to your `AndroidManifest.xml` file.
1. **Open `AndroidManifest.xml`**:
- Navigate to the `android/app/src/main` directory of your project.
- Open the `AndroidManifest.xml` file.

2. **Add the Following Attributes to `manifest` and `<application>` Tags**:

   ```xml
   <manifest xmlns:android="http://schemas.android.com/apk/res/android"
       <!-- Other attributes -->
       xmlns:tools="http://schemas.android.com/tools">
   <application
       <!-- Other attributes -->
       android:supportsRtl="true"
       android:usesCleartextTraffic="true"
       android:requestLegacyExternalStorage="true"
       tools:replace="android:name">
       <!-- Other attributes and activities -->
   </application>

#### Adding `credentials.properties` File in the `android` Folder

1. To securely add your GitHub credentials, follow these steps to create a `credentials.properties` file in your project's root directory, the directory containing the package.json file. and update the build configuration accordingly.

2. Open the `credentials.properties` file and add your GitHub credentials:

   ```properties
   github_username=your-github-username
   github_password=your-github-token-or-password
---

#### For iOS
#### Require Configurations

Before getting started with this example app, please ensure you have the following software installed on your machine:

- Xcode 14.2 or later.
- Swift 5.0 or later.
- An iOS device or emulator running iOS 13.0 or later.

#### Info.plist Changes

Please add below permissions keys to your `Info.plist` file with proper description.

```swift
<key>NSCameraUsageDescription</key>
<string>Allow access to camera for meetings</string>
<key>NSMicrophoneUsageDescription</key>
<string>Allow access to mic for meetings</string>
```

#### Enable Background Mode

Please enable `Background Modes` in your project `Signing & Capibilities` tab. After enabling please check box with option `Audio, Airplay, and Pictures in Pictures`. If you don't enables this setting, your mic will be muted when your app goes to background.

#### Screen Share Integration


#### Add Broadcast Upload Extension

You need to create a Broadcast Upload Extension to enable the screen sharing process. To do that,

open your example project, go to **Xcode -> File -> Target... ->** 

![create_broadcast_upload_extension](https://storage.googleapis.com/cpass-sdk/assets/screenshots/iOS/screenshare_1.png)

Select **Broadcast Upload Extension** and click on **Next**

![select_broadcast_upload_extension](https://storage.googleapis.com/cpass-sdk/assets/screenshots/iOS/screenshare_2.png)

Fill the **Product name** and other info, uncheck **Include UI Extension**, and click **Finish**.

![broadcast_upload_extension_info](https://storage.googleapis.com/cpass-sdk/assets/screenshots/iOS/screenshare_3.png)

Activate the Extension

![activate_broadcast_upload_extension](https://storage.googleapis.com/cpass-sdk/assets/screenshots/iOS/screenshare_4.png)

Xcode automatically creates the Extension folder, which contains the **SampleHandler.swift** file.


**NOTE: Please set deployment target for Broadcast Upload Extension same as of your main app.**


#### Add JioMeet Screen Share SDK

Go to your Podfile. Add `JioMeetScreenShareSDK_iOS` pod for your newly created broadcast upload extension and run `pod install --repo-update --verbose` command to install the SDK.

```ruby
target 'ScreenShareExtension' do
    use_frameworks!
    pod 'JioMeetScreenShareSDK_iOS', '4.0.7-temp.1'
end
```

**NOTE: `ScreenShareExtension` is name of target you fill while creating `Broadcast Upload Extension`**


### Enable App Groups

You need to enable app groups for your main app and screenshare extension. Please follow guide from below link.
[https://developer.apple.com/documentation/xcode/configuring-app-groups](https://developer.apple.com/documentation/xcode/configuring-app-groups)

[https://www.appcoda.com/app-group-macos-ios-communication/](https://www.appcoda.com/app-group-macos-ios-communication/)


#### Edit `SampleHandler` file.

Go to your `SampleHandler.swift` file. Replace the whole file content with content below.

**NOTE: Please change `YOUR_APP_GROUP_NAME_IDENTIFIER` with app group you created in above step.**

```swift
import ReplayKit
import JioMeetScreenShareSDK

class SampleHandler: JMScreenShareHandler {

    override func getAppGroupsIdentifier() -> String {
        return "YOUR_APP_GROUP_NAME_IDENTIFIER"
    }
}
```



## Setup

##### Register on JioMeet Platform:

You need to first register on Jiomeet platform.[Click here to sign up](https://platform.jiomeet.com/login/signUp)

##### Get your application keys:

Create a new app. Please follow the steps provided in the [Documentation guide](https://dev.jiomeet.com/docs/quick-start/introduction) to create apps before you proceed.

###### Get you Jiomeet meeting id and pin

Use the [create meeting api](https://dev.jiomeet.com/docs/JioMeet%20Platform%20Server%20APIs/create-a-dynamic-meeting) to get your room id and password

### Usage: Launch Core Template Screen

```js
import { launchMeetingCoreTemplateUI } from '@jiomeet/core_sdk_plugin';
// ...
         // ...
        const colorConfig = {
            primary: '#0062FF', // Primary color
            primaryDark50: '#0041CC', // Primary Dark 50
            primary70: '#338BFF', // Primary 70
        };

        **NOTE: `screenShareConfig` is used to support screen share in iOS app. If screen share is not required we don't need to pass this**

        const screenShareConfig = {
            appGroupName: 'YOUR_APP_GROUP_NAME_IDENTIFIER', // App group name
            screenShareExtensionBundleIdentifier: 'BROADCAST_UPLOAD_EXTENSION_IDENTIFIER', // Screen share extension bundle identifier
        }

        const meetingConfig = {
            meetingId: meetingId,
            meetingPin: password,
            displayName: name,
            hostToken: '',
            initialAudio: false, // Set the initial audio state
            initialVideo: false, // Set the initial video state
            colorConfig: colorConfig, // Passing color configuration
            screenShareConfig: screenShareConfig, // Passing screen share configuration to support screen share in iOS (Only required for iOS)
            isChatCallbackEnabled: true, // Example callback enabled flag
            isParticipantCallbackEnabled: false, // Example callback enabled flag
          };

          launchMeetingCoreTemplateUI(meetingConfig);
```

To join a meeting, enter meeting details in the function and directly call the function as mentioned above

### Receive a callback from SDK
```js
import { DeviceEventEmitter } from 'react-native';

// ...

useEffect(() => {
    if (Platform.OS === 'ios') {
      CoreSDKManager.addListener('call_ended', (message) => {
        console.log('Received message from Swift:', message);
        setCallStatus(message);
        // Handle the message as needed
      });
      CoreSDKManager.addListener('chat_option_clicked', (message) => {
        console.log('Received message from Swift:', message);
        setCallStatus(message);
        // Handle the message as needed
      });
    } else {
      DeviceEventEmitter.addListener('call_ended', (message) => {
        console.log('Received message from Kotlin:', message);
        setCallStatus(message);
        // Handle the message as needed
      });

      DeviceEventEmitter.addListener('chat_option_clicked', (message) => {
        console.log('Received message from Kotlin:', message);
        // setCallStatus(message);
        // Handle the message as needed
      });
}, []);
```
The above callback will be triggered when the Call is ended, you can handle this callback as per your need

### Example
```js
import * as React from 'react';

import {
  StyleSheet,
  Button,
  SafeAreaView,
  StatusBar,
  TextInput,
  useColorScheme,
  Text,
  DeviceEventEmitter,
  Platform,
} from 'react-native';
import { launchMeetingCoreTemplateUI } from '@jiomeet/core_sdk_plugin';
import { Colors } from 'react-native/Libraries/NewAppScreen';
import { useEffect, useState } from 'react';
import CoreSDKManager from '@jiomeet/core_sdk_plugin';

export default function App() {
  const isDarkMode = useColorScheme() === 'dark';
  const [meetingId, setMeetingId] = useState('');
  const [password, setPassword] = useState('');
  const [name, setName] = useState('');
  const [callStatus, setCallStatus] = useState('Not Started');

  const backgroundStyle = StyleSheet.create({
    container: {
      backgroundColor: isDarkMode ? Colors.darker : Colors.lighter,
      justifyContent: 'center',
      alignItems: 'center',
      flex: 1,
    },
  });

  useEffect(() => {
    if (Platform.OS === 'ios') {
      CoreSDKManager.addListener('call_ended', (message) => {
        console.log('Received message from Swift:', message);
        setCallStatus(message);
        // Handle the message as needed
      });
      CoreSDKManager.addListener('chat_option_clicked', (message) => {
        console.log('Received message from Swift:', message);
        setCallStatus(message);
        // Handle the message as needed
      });
    } else {
      DeviceEventEmitter.addListener('call_ended', (message) => {
        console.log('Received message from Kotlin:', message);
        setCallStatus(message);
        // Handle the message as needed
      });

      DeviceEventEmitter.addListener('chat_option_clicked', (message) => {
        console.log('Received message from Kotlin:', message);
        // setCallStatus(message);
        // Handle the message as needed
      });
    }


    return () => {
      CoreSDKManager.removeAllListeners('call_ended');
      CoreSDKManager.removeAllListeners('chat_option_clicked');

      DeviceEventEmitter.removeAllListeners('call_ended');
      DeviceEventEmitter.removeAllListeners('chat_option_clicked');
    };
  }, []);

  const colorConfig = {
    primary: '#0062FF', // Primary color
    primaryDark50: '#0041CC', // Primary Dark 50
    primary70: '#338BFF', // Primary 70
  };

  return (
    <SafeAreaView style={backgroundStyle.container}>
      <StatusBar
        barStyle={isDarkMode ? 'light-content' : 'dark-content'}
        backgroundColor={backgroundStyle.container.backgroundColor}
      />
      <TextInput
        value={meetingId}
        onChange={() => setCallStatus('Not Started')}
        style={styles.textInput}
        placeholder={'Meeting ID'}
        onChangeText={setMeetingId}
        inputMode={'numeric'}
      />
      <TextInput
        value={password}
        onChange={() => setCallStatus('Not Started')}
        style={styles.textInput}
        placeholder={'Password'}
        onChangeText={setPassword}
      />
      <TextInput
        value={name}
        onChange={() => setCallStatus('Not Started')}
        style={styles.textInput}
        placeholder={'Name Visible'}
        onChangeText={setName}
      />
      <Button
        disabled={
          name.length < 3 || meetingId.length < 9 || password.length < 5
        }
        title={'Join Call'}
        onPress={() => {
          const meetingConfig = {
            meetingId: meetingId,
            meetingPin: password,
            displayName: name,
            hostToken: '',
            initialAudio: false, // Set the initial audio state
            initialVideo: false, // Set the initial video state
            colorConfig: colorConfig, // Passing color configuration
            screenShareConfig: screenShareConfig, // Passing screen share configuration
            isChatCallbackEnabled: true, // Example callback enabled flag
            isParticipantCallbackEnabled: false, // Example callback enabled flag
          };

          launchMeetingCoreTemplateUI(meetingConfig);
        }}
      />
      {/* eslint-disable-next-line react-native/no-inline-styles */}
      <Text style={{ margin: 16 }}>Call status: {callStatus}</Text>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  box: {
    width: 60,
    height: 60,
    marginVertical: 20,
  },
  textInput: {
    padding: 16,
    margin: 16,
    backgroundColor: '#FCE6E7',
    borderRadius: 8,
    alignSelf: 'stretch',
    color: 'black',
  },
});

```

---

## Troubleshooting

- Facing any issues while integrating or installing the JioMeet Template UI Kit please connect with us via real time support present in jiomeet.support@jio.com or https://jiomeetpro.jio.com/contact-us

---
