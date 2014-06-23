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
