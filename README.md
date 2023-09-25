# React Native JioMeet Core Template SDK Plugin

## Table of Contents -

1. [Introduction](#introduction)
2. [Features](#features)
3. [Prerequisites](#prerequisites)
   - [Install package](#install-the-package)
   - [Hilt](#hilt)
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

3. **Virtual Background**: Customize the background of your video calls, adding a touch of professionalism or fun to your communication.

4. **Screen Sharing and Whiteboard Sharing**: Empower collaboration by sharing your screen or using a virtual whiteboard during meetings or video conferences.

5. **Group Conversation**: Easily engage in text-based conversations with multiple participants in one chat group.
6. **Inspect Call Health**: Monitor the quality and performance of your audio and video calls to ensure a seamless communication experience.


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

#### Hilt:

To set up Hilt in your flutter project, follow these steps:

1. First, add this below line of code to your project’s root build.gradle file (**android/build.gradle**)

```gradle
  dependencies {
    ...
    classpath "com.google.dagger:hilt-android-gradle-plugin:2.44"
    classpath("org.jetbrains.kotlin:kotlin-gradle-plugin:1.7.20")
  }
```

2. Add the Hilt dependencies to the app-level build.gradle(**android/app/build.gradle**)

```gradle
  ...
  apply plugin: 'kotlin-kapt'
  apply plugin: 'kotlin-android'
  apply plugin: 'com.google.dagger.hilt.android'

  ...
  // Allow references to generated code
  kapt {
    correctErrorTypes true
  }

  dependencies {
    ...
    implementation "com.google.dagger:hilt-android:2.44"
    kapt "com.google.dagger:hilt-android-compiler:2.44"
  }
```

3.  Enable Hilt dependency in your Android's MainApplication File. Add this below code in your React Application's android/app/src/main/java/com/'Your Projects name'/MainApplication.java file
```java
... other imports
+ import dagger.hilt.android.HiltAndroidApp;

+ @HiltAndroidApp
public class MainApplication extends Application implements ReactApplication {
... Application's code
```
---

## Setup

##### Register on JioMeet Platform:

You need to first register on Jiomeet platform.[Click here to sign up](https://platform.jiomeet.com/login/signUp)

##### Get your application keys:

Create a new app. Please follow the steps provided in the [Documentation guide](https://dev.jiomeet.com/docs/quick-start/introduction) to create apps before you proceed.

###### Get you Jiomeet meeting id and pin

Use the [create meeting api](https://dev.jiomeet.com/docs/JioMeet%20Platform%20Server%20APIs/create-a-dynamic-meeting) to get your room id and password

### Usage: Launch Core Template Screen

```tsx
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


### Receive a callback from SDK
```tsx
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

### Example
```tsx
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
} from 'react-native';
import {launchMeetingCoreTemplateUI} from '@jiomeet/core_sdk_plugin';
import { Colors } from 'react-native/Libraries/NewAppScreen';
import { useEffect, useState } from 'react';

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
    DeviceEventEmitter.addListener('call_ended', (message) => {
      console.log('Received message from Kotlin:', message);
      setCallStatus(message);
      // Handle the message as needed
    });
    return ()=>{
      DeviceEventEmitter.removeAllListeners('call_ended')
    }
  }, []);
  return (
    <SafeAreaView style={backgroundStyle.container}>
      <StatusBar
        barStyle={isDarkMode ? 'light-content' : 'dark-content'}
        backgroundColor={backgroundStyle.container.backgroundColor}
      />
      <TextInput
        value={meetingId}
        onChange={()=>setCallStatus('Not Started')}
        style={styles.textInput}
        placeholder={'Meeting ID'}
        onChangeText={setMeetingId}
        inputMode={'numeric'}
      />
      <TextInput
        value={password}
        onChange={()=>setCallStatus('Not Started')}
        style={styles.textInput}
        placeholder={'Password'}
        onChangeText={setPassword}
      />
      <TextInput
        value={name}
        onChange={()=>setCallStatus('Not Started')}
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
          launchMeetingCoreTemplateUI(meetingId, password, name);
        }}
      />
      <Text style={{margin: 16}}>Call status: {callStatus}</Text>
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
  },
});

```

---

## Troubleshooting

- Facing any issues while integrating or installing the JioMeet Template UI Kit please connect with us via real time support present in jiomeet.support@jio.com or https://jiomeetpro.jio.com/contact-us

---
