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
