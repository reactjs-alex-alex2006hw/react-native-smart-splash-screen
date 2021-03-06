# react-native-smart-splash-screen

[![npm](https://img.shields.io/npm/v/react-native-smart-splash-screen.svg)](https://www.npmjs.com/package/react-native-smart-splash-screen)
[![npm](https://img.shields.io/npm/dm/react-native-smart-splash-screen.svg)](https://www.npmjs.com/package/react-native-smart-splash-screen)
[![npm](https://img.shields.io/npm/dt/react-native-smart-splash-screen.svg)](https://www.npmjs.com/package/react-native-smart-splash-screen)
[![npm](https://img.shields.io/npm/l/react-native-smart-splash-screen.svg)](https://github.com/react-native-component/react-native-smart-splash-screen/blob/master/LICENSE)

A smart splash screen for React Native apps, written in JS, oc and java for cross-platform support.
It works on iOS and Android.

This component is compatible with React Native 0.25 and newer.

## Preview

![react-native-smart-splash-screen-ios-preview][1]
![react-native-smart-splash-screen-android-preview][2]

## Installation

```
npm install react-native-smart-splash-screen --save
```

## Installation (iOS)

* Drag RCTSplashScreen.xcodeproj to your project on Xcode.

* Click on your main project file (the one that represents the .xcodeproj) select Build Phases and drag libRCTSplashScreen.a from the Products folder inside the RCTSplashScreen.xcodeproj.

* Look for Header Search Paths and make sure it contains $(SRCROOT)/../../../react-native/React as recursive.

* In your project, Look for Header Search Paths and make sure it contains $(SRCROOT)/../node_modules/react-native-smart-splash-screen/ios/RCTSplashScreen/RCTSplashScreen

* delete your project's LaunchScreen.xib

* Dray SplashScreenResource to your project *if you want change image, replace splash.png*

* In AppDelegate.m

```

...
#import "RCTSplashScreen.h" //import interface
...
RCTRootView *rootView = [[RCTRootView alloc] initWithBundleURL:jsCodeLocation
                                                  moduleName:@"ReactNativeComponents"
                                           initialProperties:nil
                                               launchOptions:launchOptions];

[RCTSplashScreen open:rootView]; //activate splashscreen

rootView.backgroundColor = [[UIColor alloc] initWithRed:1.0f green:1.0f blue:1.0f alpha:1];

self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
UIViewController *rootViewController = [UIViewController new];
rootViewController.view = rootView;
self.window.rootViewController = rootViewController;
[self.window makeKeyAndVisible];
return YES;

```


## Installation (Android)

* In `android/settings.gradle`

```
...
include ':react-native-smart-splashscreen'
project(':react-native-smart-splashscreen').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-smart-splash-screen/android')
```

* In `android/app/build.gradle`

```
...
dependencies {
    ...
    // From node_modules
    compile project(':react-native-smart-splashscreen')
}
```

* Drag `drawable/splash.png` to `android/app/src/main/res/`

* If you want change image, replace `res/drawable/splash.png`

* In MainActivity.java (This step is available for react-native 0.25~0.29, if you're using react-native 0.30+, ignore this step and see next step)

```
...
import com.reactnativecomponent.splashscreen.RCTSplashScreenPackage;    //import package
...
/**
 * A list of packages used by the app. If the app uses additional views
 * or modules besides the default ones, add more packages here.
 */
@Override
protected List<ReactPackage> getPackages() {
    return Arrays.<ReactPackage>asList(
        new MainReactPackage(),
        new RCTSplashScreenPackage(this)    //register Module
    );
}
...

```

* If you're using react-native 0.30+, follow these steps

    * remove ReactNativeHost `final` defination, and add the following codes in MainApplication.java

    ```js
    private ReactNativeHost reactNativeHost = new ReactNativeHost(this) {
    //private final ReactNativeHost reactNativeHost = new ReactNativeHost(this) {
        ...
    }

    public void setReactNativeHost(ReactNativeHost reactNativeHost) {
        this.reactNativeHost = reactNativeHost;
    }
    ```

    * add the following codes in MainActivity.java

    ```js
    ...
    import com.reactnativecomponent.splashscreen.RCTSplashScreenPackage;    //import package
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        MainApplication mainApplication=(MainApplication)this.getApplication();
        mainApplication.setReactNativeHost( new ReactNativeHost(mainApplication) {
            @Override
            protected boolean getUseDeveloperSupport() {
                return BuildConfig.DEBUG;
            }

            @Override
            protected List<ReactPackage> getPackages() {
                return Arrays.<ReactPackage>asList(
                        new MainReactPackage(),
                        new RCTSplashScreenPackage(MainActivity.this)  //register Module
                );
            }

        });
        super.onCreate(savedInstanceState);
    }
    ```

* In `android/app/**/styles.xml`

```
...
<!-- add LaunchScreen style -->
<style name="LaunchScreen" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">false</item>
    <item name="android:windowContentOverlay">@null</item>
</style>
...
```

* In `android/app/**/AndroidManifest.xml`

```
...
<application
      android:allowBackup="true"
      android:label="@string/app_name"
      android:icon="@mipmap/ic_launcher"
      android:theme="@style/AppTheme">
      <activity
        android:name=".MainActivity"
        android:label="@string/app_name"
          android:configChanges="orientation|keyboardHidden"
          android:theme="@style/LaunchScreen"> <!-- use LaunchScreen style -->
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
      </activity>
      <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
</application>
...
```

## Full Demo

see [ReactNativeComponentDemos][0]


## Usage

```js
...
import SplashScreen from 'react-native-smart-splash-screen'
...
componentDidMount () {
     SplashScreen.close(SplashScreen.animationType.scale, 850, 500)
}
...

```

## Method

* close(animationType, duration, delay)
  close splash screen with custom animation

  * animationType: determine the type of animation. enum(animationType.none, animationType.fade, animationType.scale)
  * duration: determine the duration of animation
  * delay: determine the delay of animation


[0]: https://github.com/cyqresig/ReactNativeComponentDemos
[1]: http://cyqresig.github.io/img/react-native-smart-splash-screen-preview-ios-v1.0.0.gif
[2]: http://cyqresig.github.io/img/react-native-smart-splash-screen-preview-android-v1.0.3.gif