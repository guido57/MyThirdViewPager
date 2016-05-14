# My Third View Pager
Skeleton app using: TabLayout PagerView 3 Fragments

![mythirdviewpager](https://cloud.githubusercontent.com/assets/8282404/15268422/4e47526a-19de-11e6-8526-207e9d8e03e2.JPG)

Features:

1. Can be used as a skeleton app putting something useful inside the fragments
2. Can be used to understand how to work with a TabLayout or a PagerView or with multiple fragments

## Version 1

Where [version 2][4] was essentially just a stripped-down version of the [Barcode Scanner application][2],
version 3 is a rewrite of a large part of the codebase, making it more versatile and customizable.

With the rewrite, many APIs for UI customization were removed. Instead, it is now recommended
to create a custom Activity using the lower-level components directly
(see [Customization](#customization) for details).

Other notable changes:
* The camera is now loaded in a background thread, making the activity start faster.
* The camera preview and decoding now function correctly in any orientation.

## Adding aar dependency with Gradle

From version 3 this is a single library, supporting Gingerbread and later versions of Android
(API level 9+). If you need support for earlier Android versions, use [version 2][4].

Add the following to your build.gradle file:

```groovy
repositories {
    jcenter()
}

dependencies {
    compile 'com.journeyapps:zxing-android-embedded:3.1.0@aar'
    compile 'com.google.zxing:core:3.2.0'
}
```

## Usage with IntentIntegrator

Launch the intent with the default options:
```java
new IntentIntegrator(this).initiateScan(); // `this` is the current Activity
```

Use from a Fragment:
```java
IntentIntegrator.forFragment(this).initiateScan(); // `this` is the current Fragment

// If you're using the support library, use IntentIntegrator.forSupportFragment(this) instead.
```

Customize options:
```java
IntentIntegrator integrator = new IntentIntegrator(this);
integrator.setDesiredBarcodeFormats(IntentIntegrator.ONE_D_CODE_TYPES);
integrator.setPrompt("Scan a barcode");
integrator.setCameraId(0);  // Use a specific camera of the device
integrator.setBeepEnabled(false);
integrator.setBarcodeImageEnabled(true);
integrator.initiateScan();
```

See [IntentIntegrator][5] for more options.

### Changing the orientation

To change the orientation, create a new Activity extending CaptureActivity, and specify the
orientation in your `AndroidManifest.xml`.

Sample:

```java
public class CaptureActivityAnyOrientation extends CaptureActivity {

}
```

```xml
<activity android:name=".CaptureActivityAnyOrientation"
          android:screenOrientation="fullSensor"
          android:stateNotNeeded="true"
          android:theme="@style/zxing_CaptureTheme"
          android:windowSoftInputMode="stateAlwaysHidden">

</activity>
```

```java
IntentIntegrator integrator = new IntentIntegrator(this);
integrator.setCaptureActivity(CaptureActivityAnyOrientation.class);
integrator.setOrientationLocked(false);
integrator.initiateScan();
```

The previous API for `integrator.setOrientation()` was removed. It caused the Activity to be created
in landscape orientation, then destroyed and re-created in the requested orientation, which creates
a bad user experience. The only way around this is to specify the orientation in the manifest.

### Customization

For more control over the UI or scanning behaviour, some components may be used directly:

* BarcodeView: Handles displaying the preview and decoding of the barcodes.
* CompoundBarcodeView: Combines BarcodeView with a viewfinder for feedback, as well as some status /
  prompt text.
* CaptureManager: Manages the InactivityTimer, BeepManager, orientation lock, and returning of the
  barcode result.

These components can be used from any Activity.

Samples:
* [ContinuousCaptureActivity][6]: continuously scan and display results (instead of a once-off scan).
* [ToolbarCaptureActivity][8]: Same as the normal CaptureActivity, but with a Lollipop Toolbar.

## Android Permissions

The camera permission is required for barcode scanning to function. It is automatically included as
part of the library. On Android 6 it is requested at runtime when the barcode scanner is first opened.

When using BarcodeView directly (instead of via IntentIntegrator / CaptureActivity), you have to
request the permission manually before calling `BarcodeView#resume()`, otherwise the camera will
fail to open.

## Building locally

    ./gradlew assemble

To deploy the artifacts the your local Maven repository:

    ./gradlew publishToMavenLocal

You can then use your local version by specifying in your `build.gradle` file:

    repositories {
        mavenLocal()
    }

## Sponsored by

[Journey][1] - Build enterprise mobile apps for iOS and Android. Work in the cloud, code in JavaScript and forget about back-end development.


## License

[Apache License 2.0][7]


[1]: http://journeyapps.com
[2]: https://github.com/zxing/zxing/
[3]: https://github.com/zxing/zxing/wiki/Scanning-Via-Intent
[4]: https://github.com/journeyapps/zxing-android-embedded/blob/2.x/README.md
[5]: zxing-android-embedded/src/com/google/zxing/integration/android/IntentIntegrator.java
[6]: sample/src/main/java/example/zxing/ContinuousCaptureActivity.java
[7]: http://www.apache.org/licenses/LICENSE-2.0
[8]: sample/src/main/java/example/zxing/ToolbarCaptureActivity.java
