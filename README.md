This plugin is the official SDK for [DeepAR](http://deepar.ai). Platforms supported: Android & iOS. 

The current version of plugin supports: 
- Live AR previews ✅ 
- Take screenshots ✅ 
- Record Videos ✅ 
- Flip camera ✅ 
- Toggle Flash ✅ 

| Support |Android  | iOS|
|--|--|--|
|  |SDK 23+  |  iOS 13.0+|


## Installation
Please visit our [developer website](https://developer.deepar.ai) to create a project and generate your separate license keys for both platforms. 

Once done, please add the latest `deepar_flutter` dependency to your pubspec.yaml. 

**Android**: 
 1. compileSdkVersion should be 33 or more.
 2. minSdkVersion should be 23 or more.
 3. Download the native android dependencies from our [downloads](https://developer.deepar.ai/downloads) section and paste it in your flutter project at `android/app/libs/deepar.aar`.
 4. Make sure to `pub clean` & `flutter pub upgrade` to fetch latest working code.

Also add the following permission requests in your AndroidManifest.xml
```
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"  />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.RECORD_AUDIO"/>
<uses-permission android:name="Manifest.permission.CAPTURE_AUDIO_OUTPUT"  />
<uses-permission android:name="android.permission.CAMERA" />
```


Ensure to add these rules to `proguard-rules.pro` else app might crash in release build
```
-keepclassmembers class ai.deepar.ar.DeepAR { *; }
-keepclassmembers class ai.deepar.ar.core.videotexture.VideoTextureAndroidJava { *; }
-keep class ai.deepar.ar.core.videotexture.VideoTextureAndroidJava
``` 

**iOS:**
1. Ensure your app iOS deployment version is 13.0+.
2. Do a flutter clean & install pods.
3. To handle camera and microphone permissions, please add the following strings to your info.plist.
4. Make sure to `pub clean` & `flutter pub upgrade` to fetch latest working code.

```
<key>NSCameraUsageDescription</key>
<string>---Reason----</string>
<key>NSMicrophoneUsageDescription</key>
<string>---Reason----</string>
```
6. Also add the following to your `Podfile` file:
```
post_install do |installer|
  installer.pods_project.targets.each do |target|
    ... # Here are some configurations automatically generated by flutter

    # Start of the deepar configuration
    target.build_configurations.each do |config|

	  config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
        '$(inherited)',

        ## dart: PermissionGroup.camera
         'PERMISSION_CAMERA=1',

        ## dart: PermissionGroup.microphone
         'PERMISSION_MICROPHONE=1',    
      ]

    end 
    # End of the permission_handler configuration
  end
end
```


**Flutter:**

1. Initialize  `DeepArController` by passing in your license keys for both platforms.
```
final  DeepArController _controller = DeepArController();
_controller.initialize(
	androidLicenseKey:"---android key---",
	iosLicenseKey:"---iOS key---",
	resolution: Resolution.high);
```
2. Place the DeepArPreview widget in your widget tree to display the preview. 
```
@override

Widget  build(BuildContext  context) {
return  _controller.isInitialized
		? DeepArPreview(_controller)
		: const  Center(
			child: Text("Loading Preview")
		);
}
```

To display the preview in full screen, wrap `DeepArPreview` with `Transform.scale()` and use the correct scale factor as per preview area size. More info [here](https://github.com/DeepARSDK/deepar-flutter-plugin/issues/61#issuecomment-1331683622).
       
3.  Load effect of your choice by passing the asset file to it in `switchPreview`
```
_controller.switchEffect(effect);
```
4. To take a picture, use `takeScreenshot()` which return the picture as file.
```
final File file = await _controller.takeScreenshot();
```
5. To record a video, please use : 
```
if (_controller.isRecording) {
_controller.stopVideoRecording();
} else {
final File videoFile = _controller.startVideoRecording();
}
```

For more info, please visit: [Developer Help](https://help.deepar.ai/en/).