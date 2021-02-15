## Android Setup Part I: Gradle Setup

Install the package:
```
$ yarn add react-native-config
```
 -  

	**android/settings.gradle**
	Add this below rootProject variable:
	```diff
	+ include ':react-native-config'
	+ project(':react-native-config').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-config/android')
	```
	**android/app/build.gradle**
	At 2nd line, add this line:
    ```diff
    + apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"
    ```
    At dependencies, add this line:
	```diff
	dependencies {
		implementation "com.facebook.react:react-native:+"  // From node_modules
	+	implementation project(':react-native-config')
	}
	```

## Basic Usage
Create a new file `.env` in the root of your React Native app, add anything you like eg:

```
API_URL=https://myapi.com
GOOGLE_MAPS_API_KEY=abcdefgh
```

Then access variables defined there from your app:

```js
import Config from "react-native-config";

Config.API_URL; // 'https://myapi.com'
Config.GOOGLE_MAPS_API_KEY; // 'abcdefgh'
```

Now run your app with the following command to see if the .env file can be read
```
SET ENVFILE=.env && npx react-native run-android
```

If any issues occur, try cleaning/rebuilding with gradle and run the command again.


## Android Setup Part II: Flavoring Setup

Add to **android/app/build.gradle**
```
defaultConfig {
    ...
    resValue "string", "build_config_package", "YOUR_PACKAGE_NAME_IN_ANDROIDMANIFEST.XML"
}
```

Also, in **android/app/build.gradle**, add this below buildTypes
```
buildTypes {
    ...
}

flavorDimensions "default"  

    productFlavors {
        dev {
            //configure applicationId for app published to Play store
            applicationId project.env.get("ANDROID_APP_ID")
        }
        sandbox {
            //configure applicationId for app published to Play store
            applicationId project.env.get("ANDROID_APP_ID")
        }
        prod {
            // applicationId project.env.get("ANDROID_APP_ID")
        }
    }
```
Modify **strings.xml**
```diff
    - <string name="app_name">your_app_name</string>
    + <string name="app_name">@string/ANDROID_APP_NAME</string>
```
Add the following commands to **package.json**
```
"scripts": {
    "android-dev": "ENVFILE=.env.development react-native run-android --variant=devDebug",
    "android-sb": "ENVFILE=.env.sandbox react-native run-android --variant=sandboxDebug",
    "android-prod": "ENVFILE=.env.production react-native run-android --variant=prodDebug",
    "win-android-dev": "SET ENVFILE=.env.development && react-native run-android --variant=devDebug",
    "win-android-sb": "SET ENVFILE=.env.sandbox && react-native run-android --variant=sandboxDebug",
    "win-android-prod": "SET ENVFILE=.env.production && react-native run-android --variant=prodDebug",
    "apk-dev": "cd android && ENVFILE=.env.development ./gradlew assembledevRelease",
    "apk-sb": "cd android && ENVFILE=.env.sandbox ./gradlew assemblesandboxRelease",
    "apk-prod": "cd android && ENVFILE=.env.production ./gradlew assembleprodRelease",
    "aab-dev": "cd android && ENVFILE=.env.development ./gradlew bundledevRelease",
    "aab-sb": "cd android && ENVFILE=.env.sandbox ./gradlew bundlesandboxRelease",
    "aab-prod": "cd android && ENVFILE=.env.production ./gradlew bundleprodRelease",
    ...
  },
```

## Basic Usage
Create three .env files, adding whatever variables you like inside them.

`.env.development`
`.env.sandbox`
`.env.production`

Now run your app with the following command to see if those .env file can be read
```
 yarn win-android-dev 
```

If any issues occur, try cleaning/rebuilding with gradle and run the command again.





