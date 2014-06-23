<style>
.img-center {
    text-align: center;
    display: block;
}
</style>

## Push Notifications

Android devices receive push notifications through the **GCM** service (Google Cloud Messaging) and iOS through **APN** (Apple Push Notifications).

 You can read more about the **GCM** service [here](http://developer.android.com/google/gcm/index.html).

<a class="img-center">
    <img src="http://cdn2.raywenderlich.com/wp-content/uploads/2011/05/PushNotifWhy.jpg" width="300">
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
