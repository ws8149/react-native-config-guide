## Setup IOS Part I: Basics

Install the package:

```
$ yarn add react-native-config
```

Run pod install

```
(cd ios; pod install)
```

Create a new file `.env` in the root of your React Native app:

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

Run your app with this command
```
TODO
```

#### IOS Setup Part II: Flavoring

1. click on the file tree and create new file of type XCConfig
   ![img](./readme-pics/1.ios_new_file.png)
   ![img](./readme-pics/2.ios_file_type.png)
2. save it under `ios` folder as "Config.xcconfig" with the following content:

```
#include? "tmp.xcconfig"
```

3. add the following to your ".gitignore":

```
# react-native-config codegen
ios/tmp.xcconfig

```

4. go to project settings
5. apply config to your configurations
   ![img](./readme-pics/3.ios_apply_config.png)
6. Go to _Edit scheme..._ -> _Build_ -> _Pre-actions_, click _+_ and select _New Run Script Action_. Paste below code which will generate "tmp.xcconfig" before each build exposing values to Build Settings and Info.plist. Make sure to select your target under _Provide build settings from_, so `$SRCROOT` environment variables is available to the script. (Note that this snippet has to be placed after "cp ... \${PROJECT_DIR}/../.env" if [approach explained below](#ios-multi-scheme) is used).

   ```
   "${SRCROOT}/../node_modules/react-native-config/ios/ReactNativeConfig/BuildXCConfig.rb" "${SRCROOT}/.." "${SRCROOT}/tmp.xcconfig"
   ```

   ![img](./readme-pics/4.ios_pre_actions.png)

7. You can now access your env variables in the Info.plist, for example `$(MY_ENV_VARIABLE)`. If you face issues accessing variables, please open a new issue and provide as much details as possible so above steps can be improved.


Create a new scheme:

- In the Xcode menu, go to Product > Scheme > Edit Scheme
- Click Duplicate Scheme on the bottom
- Give it a proper name on the top left. For instance: "Myapp (staging)"
- Make sure the "Shared" checkbox is checked so the scheme is added to your version control system

Then edit the newly created scheme to make it use a different env file. From the same "manage scheme" window:

- Expand the "Build" settings on left
- Click "Pre-actions", and under the plus sign select "New Run Script Action"
- Where it says "Type a script or drag a script file", type:
  ```
  cp "${PROJECT_DIR}/../.env.staging" "${PROJECT_DIR}/../.env"  # replace .env.staging for your file
  ```
Also ensure that "Provide build settings from", just above the script, has a value selected so that PROJECT_DIR is set.

Paste this into package.json
```
TODO
```

Run your app with this command
```
TODO
```
