# Bonus: on iOS

The process for pushing notifications and deploying on the iOS App Store is more complicated. So we've added the instructions in the Bonus section.

Prerequisites for Apple Push Notifications are:

1. Have a physical device. **APN** does not work in the simulator.
2. An iOS Developer Program account. We need to make a new App ID and provisioning profile for each app that uses push, as well as an **SSL** certificate for the server. We do this in the iOS Provisioning Portal.

The server should provide the payload as a JSON dictionary in the following format:

```js
{
    "aps": {
        "alert": "Hello, world!",
        "sound": "default"
    }
}
```

To enable push notifications in your app, it needs to be signed with a provisioning profile that is configured for push.

The **provisioning profile** and **SSL certificate** are closely tied together and are only valid for a single **App ID**. This is a protection that ensures only your server can send push notifications to instances of your app, and no one else.

Later, we will see that apps use different provisioning profiles for development and distribution. There are also two types of push server certificates:

- **Development:** If your app is running in Debug mode and is signed with the Development provisioning profile (Code Signing Identity is “iPhone Developer”), then your server must be using the Development certificate.

- **Production:** Apps that are distributed as Ad Hoc or on the App Store (when Code Signing Identify is “iPhone Distribution”) must talk to a server that uses the Production certificate. If there is a mismatch between these, push notifications cannot be delivered to your app.

Digital certificates are based on public-private key cryptography.

1. Open **Keychain Access** on your **Mac** and choose the menu option **Request a Certificate from a Certificate Authority...**
2. Enter your email address and a descriptive app name `HeyApp`.
3. Save the file as HeyApp.certSigningRequest
4. In the **Keys** section, select the new private key and export it as `HeyAppKey.p12`

Let's upload it to **iOS Dev Center**:

Go to **App ID**s and create a new one with the following details:

    ```
    App ID Description: HeyApp
    App Services: Check the Push Notifications Checkbox
    Explicit App ID: com.codefirstgirls.phonegap
    ```

Open the **App ID** you have just created, it should look like this:

<a class="img-center">
    <img src="http://f.cl.ly/items/2u3d3o0t2G2v1K213K17/Screen%20Shot%202014-06-20%20at%2018.50.06.png" width="400" >
</a>

Now let's create the **SSL Certificate:**

1. Click on **Settings** and scroll down to the **Push Notifications** section
2. Select **Create Certificate**
3. The first thing it asks you is to generate a **Certificate Signing Request** so we can click on **continue** and and upload it
4. Once your **SSL Certificate** has been created download it and save it to the `notifications` folder.

Making a **PEM** file.

Let's recap, so far we have three files:

- the **CSR**
- the private key as p12 file (HeyAppKey.p12)
- the **SSL Certificate** (aps_development.cer)

We have to convert the certificate and private key into a format that is more usable, a PEM file.

**NOTE:** There are two different APNS servers: the "sandbox" server that you can use for testing, and the live server that you use in production mode. Above, we used the sandbox server because our certificate is intended for development, not production use.

### Making the provisioning profile

1. Select the **iOS App development**
2. Select the **HeyApp** App ID that we created earlier. This will ensure this provisioning profile is explicitly tied to the HeyApp app.
3. Next select the certificates you want to include in this provisioning profile.
4. Select the devices you want to include in this provisioning profile. Sine you're creating the development profile.
5. Name is `HeyApp Development`
6. Download the newly created profile
7. Add the **provisioning profile** to Xcode

**Source:** [Raywenderlich](http://www.raywenderlich.com/32960/apple-push-notification-services-in-ios-6-tutorial-part-1)

### Sending notifications

Guess what? Similarly to **node-gcm**, there's node module called [node-apn](https://github.com/argon/node-apn)

### Differences between Android and iOS

 1. Android payload size is 4k whereas iOS payload size is 256 bytes
 2. iOS requires extra set up from the Apple Developer Portal to authorize the app id for push notifications, as well as be signed with a unique SSL certificate that the server can verify against.
 3. GCM will always return a message indicating if a device id has changed or is invalid, but with Apple you need to periodically ping their feedback server to find out which device tokens have become invalid.
 4. You can specify a timeToLive parameter for Android of 0 seconds to 4 weeks on the life of your notification. Apple does not specify any given time period.
 5. For Android you can specify a collapseKey which will allow you to save up messages and only the last one will be delivered. On iOS if you send multiple notifications to a device that is offline, only the last message will get delivered.

 Source: [devgirl.org](http://devgirl.org/2013/07/17/tutorial-implement-push-notifications-in-your-phonegap-application/)
