[README_EN.md](https://github.com/user-attachments/files/32867783/README_EN.md)

Shortcut:
-download BT-Max-1.0.apk on your phone
-find it in your download folder
-Enable unknown sources
-click install
-Done, now you can use the app, type a name of the device while you are connected, and chose if you want in on or off

# BT Max

BT Max is a simple Android app that keeps media volume at the maximum level for a selected connected Bluetooth audio device. The user enters the device name or selects it from the list of currently connected devices, enables protection, and the app restores 100% volume whenever Android reports that the media volume has been lowered.

The app is written natively in Java. It does not require root, an account, internet access, or any external server.

## Main features

- select a Bluetooth device by name,
- choose from currently connected Bluetooth audio devices,
- automatically restore maximum media volume,
- keep protection running in the background through a foreground service,
- show a persistent notification with a **STOP** action,
- Polish app interface,
- no internet access,
- support for Bluetooth A2DP and LE Audio headphones or speakers,
- route checking to avoid raising phone volume when the selected Bluetooth device is not the active media output.

## Requirements

- Android 13 or newer,
- a phone with Bluetooth,
- Bluetooth speaker or headphones connected to the phone,
- Nearby devices permission,
- Notifications permission so the app can show its foreground service notification.

## APK installation

1. Download `BT-Max-1.0.apk` to your phone.
2. Open the downloaded APK file.
3. If Android asks for permission to install apps from this source, allow it.
4. Tap **Install**.
5. After installation, open **BT Max**.

If you received the app as a ZIP archive, extract the ZIP first in the **Files** app, then open `BT-Max-1.0.apk`.

## How to use the app

1. Connect your phone to the Bluetooth device in Android settings.
2. Make sure media audio is enabled for that device.
3. Open **BT Max**.
4. Enter the exact device name or tap **Choose connected device**.
5. Tap **Enable protection · 100%**.
6. Grant the required permissions.
7. Start playing music or any other media audio.
8. When the volume visible to Android is lowered, the app will try to restore the maximum level.

You can stop protection from the app screen or by tapping **STOP** in the notification.

## How volume detection works

The app checks the system media volume repeatedly. If the volume drops below the maximum level and the selected Bluetooth device is still the expected media output, the app sets the volume back to the highest available level.

Before changing volume, the app checks Android's predicted media output route. This prevents it from raising the phone speaker volume when Bluetooth is disconnected or when audio has moved to a different output.

## Limitations

The app reacts only to volume changes visible to Android. It does not know who changed the volume. The change may come from the phone buttons, the Bluetooth device, Android itself, or another app.

Buttons or knobs on a Bluetooth speaker are detectable only when the device synchronizes its volume with the phone. In practice, this means Bluetooth Absolute Volume support. If the speaker changes only its own internal amplifier volume and does not report that change to the phone, the app cannot detect or undo it.

The app does not bypass Android safety limits, manufacturer restrictions, fixed-volume devices, or system volume caps. If Android does not allow full volume, the app can show that state, but it cannot override the system restriction.

## Privacy

BT Max does not use the internet and does not send any data anywhere. It only uses Android features needed to detect connected Bluetooth devices, run in the background, and control media volume.

## Project structure

```text
BTMax/
├── app/
│   ├── build.gradle
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/pl/trackgomania/btmax/
│       │   ├── Devices.java
│       │   ├── MainActivity.java
│       │   └── VolumeGuardService.java
│       └── res/
├── gradle/
├── tests/
│   └── test_devices.py
├── build.gradle
├── gradle.properties
├── gradlew
├── gradlew.bat
├── README.md
└── settings.gradle
```

## Building in Android Studio

1. Open the `BTMax` folder in Android Studio.
2. Wait for Gradle sync to finish.
3. Make sure Android SDK Platform 35 and Build Tools 35.0.0 are installed.
4. Select **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
5. The generated APK will be available in `app/build/outputs/apk/debug/`.

## Building from the terminal

Linux or macOS:

```sh
./gradlew assembleDebug lintDebug
```

Windows:

```bat
gradlew.bat assembleDebug lintDebug
```

Generated debug APK:

```text
app/build/outputs/apk/debug/app-debug.apk
```

## Testing

The project includes a simple host-side test for device matching and audio route logic:

```sh
python3 tests/test_devices.py
```

The test requires JDK 17. Host-side tests do not replace testing on a real phone with a real Bluetooth device.

## Suggested phone test

1. Install the APK.
2. Connect the phone to a Bluetooth speaker or headphones.
3. Select the device in the app.
4. Enable protection.
5. Start playing music.
6. Lower the volume with the phone volume button.
7. Check whether the app restores maximum volume.
8. Lower the volume using the Bluetooth device button or knob.
9. Check whether the Android volume slider changes too.
10. Disconnect Bluetooth and confirm that the phone speaker is not raised.
11. Stop protection with **STOP**.

If the Bluetooth device has independent local volume control, Android may not see that change. In that case, the app cannot reverse it.

## Technologies

- Java,
- Android SDK 35,
- Android Gradle Plugin 8.9.1,
- Gradle Wrapper 8.11.1,
- minimum Android version: API 33.

## Project status

The project contains a working debug APK and source code. The app has been compiled, checked with Android Lint, and verified with host-side tests for audio route matching logic. Testing on a physical phone with a specific Bluetooth device still needs to be done separately, because Bluetooth behavior depends on the phone, Android version, and audio device.
