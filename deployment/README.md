## Deployment

In this section we'll see how to deploy your PhoneGap app to the Apple App Store and Google Play Store.

But before that let's add custom icons and splashscreens.

You need to provide the **icon** and **splashscreen** in different sizes.

The easiest way to do it is to look at the size of icons/splashscreens for the default **PhoneGap** application.

For the ```ios``` platform they are located in:

```
platforms/ios/HeyApp/Resources/icons
platforms/ios/HeyApp/Resources/splash
```

and for ```android```, they are in:

```
platforms/android/res
```

**Challenge**: Replace all 3 folders with the HeyApp icons and splashscreens located here [CFG branding](http://google.com)

### Deploy for iOS

You need an Apple Developer Account to submit your App.

If you don't have one that's fine we'll show how to do it.

Step to submit your app are:

1. Log On the [Apple Developer Portal](https://developer.apple.com/)
2. Register a new **Explicit App ID** that matches the one we gave to our application ```com.codefirstgirl.phonegap```. Make sure to tick the **Push Notifications** checkbox.
3. Create a **Production Certificate** and download it.
4. Create a **Distribution Provisioning Profile**. Select the **App ID** and **Production Certificate** you just created. Name it ```HeyApp Production```.
5. Finally download it.


1. Open the **Production Certificate** in Keychain.
2. Export it from the **Keychain** app.

Next we need to create the New App on iTunes Connect:

1. Log On **iTunes Connect**.
2. Create a new app. Make sure to select the correct **Bundle ID**.
3. Choose the **availability date** and **pricing**.
4. Fill out all the options. Give it a description.

Now we have registered a new App with **iTunes Connect** we can jump back in **Xcode** to upload our **.ipa** file.

1. Select **iOS device** in the target dropdown.
2. Select **Product > Archive**.

Great! Now let's wait and see if **Apple** accepts our application.

### Deploy for Android

1. Make sure you've set the **versionName** and **versionCode** in ```platforms/android/AndroidManifest.xml```.
2. Create a **keystore** file. Check [here](http://developer.android.com/tools/publishing/app-signing.html#cert) to generate a new one.
    ```
    $ keytool -genkey -v -keystore hey-release-key.keystore
       -alias hey -keyalg RSA -keysize 2048 -validity 10000
    ```

3. Then we need to tell **ant** where your keystore file is located for this app. To do so create a file called **ant.properties** ```platforms/android``` and copy in the following snippet:
    ```
    key.store=/Users/<user_name>/Developer/cfg/phonegap/platforms/android/hey-release-key.keystore
    key.alias=hey
    ```

Now that we have a signed key we can build our product **.pkg** file.

1. Open the command prompt and run:
    ```
    $ phonegap build android
    ```
    In ```platforms/android/bin``` you have:
    ```
    HeyApp.ap_
    HeyApp.ap_.d
    HeyApp-debug.apk
    HeyApp-debug-unaligned.apk
    HeyApp-debug-unaligned.apk.d
    ```

2. Navigate to ```platforms/android``` and run:
    ```
    $ ant release
    ```
    Enter the keystore password you provided in phase 1.

3. Now move in the ```bin``` directory and run these **jarsigner** commands:
    ```
    $ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore /Users/<user_name>/Developer/cfg/confidential/HeyApp-release-key.keystore HeyApp-release-unsigned.apk HeyApp
    ```

4. Run this **zipalign** command to create the final **apk** file:
    ```
    $ zipalign -v 4 HeyApp-release-unsigned.apk HeyApp.apk
    ```

Your final ```HeyApp.apk``` will be created in the ```bin``` directory.

Now let's upload it to the Google Play Store:

1. Log On the [Google Play Developer Console](https://play.google.com/apps/publish/) and create a new account.
2. Click on **Upload a new application**. Choose a language and title.
3. Upload the ```.apk``` file.

Finaly don't forget to:

1. Fill in all the required fields such as a **description** and **contact details.
2. Upload the screenshots.
