

<p align="center">
<img src="https://img.shields.io/github/license/andriiklymiuk/react-native-publisher" alt="github/license" />
<img src="https://img.shields.io/github/issues/andriiklymiuk/react-native-publisher" alt="github/issues" />
<img src="https://img.shields.io/github/issues-pr/andriiklymiuk/react-native-publisher" alt="github/issues-pr" />
<img alt="npm" src="https://img.shields.io/npm/v/react-native-publisher">
<img src="https://img.shields.io/github/followers/andriiklymiuk?style=social" alt="github/followers" />
<img src="https://img.shields.io/github/stars/andriiklymiuk/react-native-publisher?style=social" alt="github/stars" />
<img src="https://img.shields.io/github/forks/andriiklymiuk/react-native-publisher?style=social" alt="github/forks" />
</p>

📹 Live stream RTMP publisher for React Native with built in camera support!

THIS IS ORIGINALLY FORK OF [THE AWESOME LIBRARY](https://github.com/Andriiklymiuk/react-native-publisher). p.s Created this fork, because i needed some fixes to be in the app (like video settings setup, reconnection, etc.)

### ⚠️🛠️ Support for [the new architecture](https://reactnative.dev/docs/the-new-architecture/landing-page) is under development

## Installation

```sh
npm install react-native-publisher
```

or

```sh
yarn add react-native-publisher
```

and for iOS
```sh
cd ios && pod install
```
## Android

Add Android Permission for camera and audio to `AndroidManifest.xml`

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
```


## iOS

Add iOS Permission for camera and audio to `Info.plist`

```xml
<key>NSCameraUsageDescription</key>
 <string>CAMERA PERMISSION DESCRIPTION</string>
<key>NSMicrophoneUsageDescription</key>
 <string>AUDIO PERMISSION DESCRIPTION</string>
```

## Example Project

Clone the repo and run

```sh
yarn
```
and

```sh
cd example && yarn ios (or yarn android)
```

You can use Youtube for live stream server. You can check [Live on Youtube](https://support.google.com/youtube/answer/2474026?hl=tr&ref_topic=9257984)

## Usage
```js
import RTMPPublisher from 'react-native-publisher';

// ...

async function publisherActions() {
  await publisherRef.current.startStream();
  await publisherRef.current.stopStream();
  await publisherRef.current.mute();
  await publisherRef.current.unmute();
  await publisherRef.current.switchCamera();
  await publisherRef.current.getPublishURL();
  await publisherRef.current.isMuted();
  await publisherRef.current.isStreaming();
  await publisherRef.current.toggleFlash();
  await publisherRef.current.hasCongestion();
  await publisherRef.current.isAudioPrepared();
  await publisherRef.current.isVideoPrepared();
  await publisherRef.current.isCameraOnPreview();
  await publisherRef.current.setAudioInput(audioInput: AudioInputType);
}

<RTMPPublisher
  ref={publisherRef}
  streamURL="rtmp://your-publish-url"
  streamName="stream-name"
  videoSettings={{
    width: 1080,
    height: 1920,
    bitrate: 5000 * 1024,
    audioBitrate: 192 * 1000
  }}
  allowedVideoOrientations={[
    "portrait",
    "landscapeLeft",
    "landscapeRight",
    "portraitUpsideDown"
  ]}
  videoOrientation="portrait"
  onConnectionFailedRtmp={() => ...}
  onConnectionStartedRtmp={() => ...}
  onConnectionSuccessRtmp={() => ...}
  onDisconnectRtmp={() => ...}
  onNewBitrateRtmp={() => ...}
  onStreamStateChanged={(status: streamState) => ...}
/>
```

## Props

|     Name     |   Type   | Required |              Description              |
| :----------: | :------: | :------: | :-----------------------------------: |
| `streamURL`  | `string` |  `true`  | Publish URL address with RTMP Protocol |
| `streamName` | `string` |  `true`  |          Stream name or key           |
| `videoSettings` | `VideoSettingsType` |  `false`  |   Video settings for video        |
| `allowedVideoOrientations` | `VideoOrientation[]` |  `false`  |   Allowed video orientation        |
| `videoOrientation` | `VideoOrientation` |  `false`  |   Initial video orientation        |

### Youtube Example

For live stream, Youtube gives you stream url and stream key, you can place the key on `streamName` parameter

**Youtube Stream URL:** `rtmp://a.rtmp.youtube.com/live2`

**Youtube Stream Key:** `****-****-****-****-****`

```js
<RTMPPublisher
  streamURL="rtmp://a.rtmp.youtube.com/live2"
  streamName="****-****-****-****-****"
  ...
  ...
```

## Events

|          Name                    |        Returns            |                                        Description                                        |  Android | iOS |
| :------------------------------: | :-----------------------: | :---------------------------------------------------------------------------------------: | :------: |:---:|
|      `onConnectionFailed`        |         `null`            |                        Invokes on connection fails to publish URL                         |     ✅    |  ✅ |
|     `onConnectionStarted`        |         `null`            |                        Invokes on connection start to publish URL                         |     ✅    |  ✅ |
|     `onConnectionSuccess`        |         `null`            |                       Invokes on connection success to publish URL                        |     ✅    |  ✅ |
|         `onDisconnect`           |         `null`            |                          Invokes on disconnect from publish URL                           |     ✅    |  ✅ |
|     `onNewBitrateReceived`       |         `null`            |                         Invokes on new bitrate received from URL                          |     ✅    |  ❌ |
|     `onStreamStateChanged`       |      `StreamState`        | Invokes on stream state changes. It can be use alternatively for above connection events. |     ✅    |  ✅ |
| `onBluetoothDeviceStatusChanged` | `BluetoothDeviceStatuses` |                        Invokes on bluetooth headset state changes.                        |     ✅    |  ❌ |

## Methods

|        Name         |          Returns          |         Description         |   Android | iOS |
| :-----------------: | :------------------------:| :-------------------------: |  :------: |:---:|
|    `startStream`    |       `Promise<void>`     |      Starts the stream      |     ✅    |  ✅ |
|    `stopStream`     |       `Promise<void>`     |      Stops the stream       |     ✅    |  ✅ |
|       `mute`        |       `Promise<void>`     |    Mutes the microphone     |     ✅    |  ✅ |
|      `unmute`       |       `Promise<void>`     |   Unmutes the microphone    |     ✅    |  ✅ |
|   `switchCamera`    |       `Promise<void>`     |     Switches the camera     |     ✅    |  ✅ |
|   `toggleFlash`     |       `Promise<void>`     |    Toggles the flash        |     ✅    |  ✅ |
|   `getPublishURL`   |      `Promise<string>`    |    Gets the publish URL     |     ✅    |  ✅ |
|      `isMuted`      |      `Promise<boolean>`   |  Returns microphone state   |     ✅    |  ✅ |
|    `isStreaming`    |      `Promise<boolean>`   |   Returns streaming state   |     ✅    |  ✅ |
|   `hasCongestion`   |      `Promise<boolean>`   |    Returns if congestion    |     ✅    |  ❌ |
|  `isAudioPrepared`  |      `Promise<boolean>`   | Returns audio prepare state |     ✅    |  ✅ |
|  `isVideoPrepared`  |      `Promise<boolean>`   | Returns video prepare state |     ✅    |  ✅ |
| `isCameraOnPreview` |      `Promise<boolean>`   |    Returns camera is on     |     ✅    |  ❌ |
|   `setAudioInput`   |  `Promise<AudioInputType>`|    Sets microphone input    |     ✅    |  ✅ |
|`setVideoSettings`   | `Promise<VideoSettingsType>`| Sets camera quality settings|      ✅  |  ✅ |

## Types

| Name                      |                      Value                          |
| ------------------------- | :--------------------------------------------------:|
| `streamState`             | `CONNECTING`, `CONNECTED`, `DISCONNECTED`, `FAILED` |
| `BluetoothDeviceStatuses` | `CONNECTING`, `CONNECTED`, `DISCONNECTED`           |
| `AudioInputType`          | `BLUETOOTH_HEADSET`, `SPEAKER`, `WIRED_HEADSET`     |
| `VideoSettingsType`       | `{width: number; height: number; bitrate: number; audioBitrate: number}`    |
| `VideoOrientation`       | `portrait`, `landscapeLeft`, `landscapeRight`, `portraitUpsideDown`   |

* AudioInputType: WIRED_HEADSET type supporting in only iOS. On Android it affects nothing. If a wired headset connected to Android device, device uses it as default.
## Used Native Packages

- Android: [rtmp-rtsp-stream-client-java](https://github.com/pedroSG94/rtmp-rtsp-stream-client-java) [2.2.2]
- iOS: [HaishinKit.swift](https://github.com/shogo4405/HaishinKit.swift) [1.5.3]

## Contributing

See the [contributing guide](CONTRIBUTING.md) to learn how to contribute to the repository and the development workflow.

## License

MIT
