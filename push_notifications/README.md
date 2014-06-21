<style>
.img-center {
    text-align: center;
    display: block;
}
</style>

## Push Notifications

Android devices receive push notifications through the **GCM** service (Google Cloud Messaging) and iOS through **APN** (Apple Push Notifications).

<a class="img-center">
    <img src="http://cdn2.raywenderlich.com/wp-content/uploads/2011/05/PushNotifWhy.jpg" width="300">
</a>

### The PhoneGap PushPlugin

There is an official PhoneGap plugin called **PushPlugin**. Let's add it to our project. In the `phonegap` folder run:

```shell
$ cordova plugin add https://github.com/phonegap-build/PushPlugin
```

Locate the **PushNotification.js** file that was installed into your **plugins** folder and copy it in the **www** folder.

Copy `PushNotification.js` located at `plugins/com.phonegap.plugins.PushPlugin/www/PushNotification.js` in your `www/js` folder:

Reference it in your `index.html` file:

```js
<script type="text/javascript" src="js/PushNotification.js"></script>
```
### Push Notifications on Android

 You can read more about the **GCM** service [here](http://developer.android.com/google/gcm/index.html).

 First we need to get a **GCM Project Number** and a corresponding **GCM API Key**. Follow those steps:

 1. Log On the [Google Developer Console](https://console.developers.google.com)
 2. Create a new **Project**
 3. Save the **Project ID**

 If you have already created one you can find it on the overview page:

 <a class="img-center">
    <img src="http://f.cl.ly/items/0P1J0w2b0b302Y3c1f23/Screen%20Shot%202014-06-20%20at%2015.23.12.png" width="600">
 </a>

### Register the application with GCM

 Open the file `www/js/index.js`. In the `receivedEvent` function add the following code:

 ```js
 var pushNotification = window.plugins.pushNotification;
 pushNotification.register(app.successHandler, app.errorHandler, {'senderID': PROJECT_NUMBER, 'ecb': "app.onNotificationGCM"});
 ```

 Don't forget to comment out the default PhoneGap code in the body of `receivedEvent`.

 Now let's implement the callbacks that we referenced above, the `successHandler` will be called when the registration is successful. The result will contain the registration token.

 ```html
 successHandler: function(result) {
    alert('Success! : ' result);
 }
 ```

 Let's display a possible error in the `errorHandler` callback:

 ```js
 errorHandler: function(error) {
    alert(error);
 }
 ```

 <a class="img-center">
    <img src="http://f.cl.ly/items/373r2n1o0Y2C2w3M2F2A/Screen%20Shot%202014-06-20%20at%2017.53.58.png" width="300" >
 </a>

 **For Genymotion users:** The `successHandler` gets called but not `onNotificationGCM`, we were expecting an alert message with the **registration ID** returned from Apple. To fix that follow the guide to [install](http://forum.xda-developers.com/showthread.php?t=2528952) the Google Apps.

 Make sure you copy paste the **registration ID**, we will need it in the next section.

 Now our application is ready to receive push notifications from **GCM**.

### Sending notifications

 We'll use **node-gcm** to push notifications to **GCM**. First we need to get a server **API** key. Go back to the **Developer Console** and click on **Create new Server key**.

  Now let's enable the **GCM Service**

  1. On the sidebar, select **APIs & auth**
  2. Turn **GCM for Android** on

  Obtaining an API Key:

  1. Select **APIs & auth > Credentials**
  2. Under **Public API access**, click **Create new key**
  3. In the **Create a new key** dialog, click **Server Key**
  4. Enter ```0.0.0.0/0``` for the server's IP
  5. Click `refresh` and copy the `API Key`

 Now let's install the node modules we need. Run `npm install`.

 Open your editor and create a file in `notifications/notify.js` and paste in the following:

 ```js
 var gcm = require('node-gcm');
 var message = new gcm.Message();

 //API Server Key
 var sender = new gcm.Sender('SERVER_API_KEY');
 var registrationIds = [];

 //Payload data to send...
 message.addData('message', 'Testing Push Notifications!');
 message.addData('title', 'Test');
 message.addData('msgcnt','3'); // Shows up in the notification in the status bar
 message.addData('soundname','beep.wav'); //Sound to play upon notification receipt - put in the www folder in app
 //message.collapseKey = 'demo';
 //message.delayWhileIdle = true; //Default is false
 message.timeToLive = 3000;// Duration in seconds to hold in GCM and retry before timing out. Default 4 weeks (2,419,200 seconds) if not specified.

  // At least one reg id required
  registrationIds.push('REGISTRATION-ID');

  /**
   * Parameters: message-literal, registrationIds-array, No. of retries, callback-function
   */
  sender.send(message, registrationIds, 4, function (result) {
      console.log(result);
  });
 ```

 Now let's run our script from the command line:

 ```js
 node notify.js
 ```

 You should see the notification and see it in your status bar like this on your Android device. When clicked on, the application should open and you should see this alert popup showing the message data.


### Other things to note...

 You can set the badge number from the PushPlugin API via:

 ```js
 pushNotification.setApplicationIconBadgeNumber(this.successHandler, 0);
 ```

 You can specify a custom sound file in your www folder and referring to it from the server side.

### Apple Push Notifications

<a class="img-center">
    <img src="http://devgirl.org/wp-content/uploads/2012/10/Push-Overview-467x500.jpeg" width="500" >
</a>

1. Your application communicates with the **APN** Service to authorize it to receive push notifications.
2. Apple responds with a unique token that must be used in all future communications to receive push notifications.
3. You application sends that token to a 3rd party server (like our `notify.js` one). It will store it for later events when the application needs to be notified.
4. When an event occurs where your application needs to receive a notification, the server script reads in the unique device token and sends a message to **APN** which then securely sends the notification to your device.

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


