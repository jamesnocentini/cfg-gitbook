## A Simple PhoneGap application

#### The PhoneGap lifecycle of an application


#### Installing Cordova CLI

Cordova comes with a **command line interface** to commands such as:

- starting a new project
- adding a platform (for us it will be ```android``` and ```ios```)
- and many more...

To install it run:

```js
sudo npm install -g cordova-cli
```

#### Starting the CFGMessenger

You can create a seed PhoneGap project with the following command.

```js
$ cordova create phonegap com.codefirstgirl.com CFGMessenger
```

**Challenge:** Launch the app in Chrome

Let's start an http web server with the SimpleHTTPServer python command:

```
$ python -m SimpleHTTPServer
```

The default port should be 8000. Open up your browser at ```localhost:8000```.

Open the emulation tab and select the device of your choice.
Hit refresh. You have it!

#### Adding the platforms

As you now know, the ```www``` folder contains all our "web" code. But to run on actual devices, we need more configuration. Luckily, cordova can handle this for us.

First, make sure you are in the phonegap directory:

```
$ cd phonegap
```

The generic command to add a platform is:

```js
$ cordova platform add <platform_name>
```

**Challenge:** Add the ios platform to your project (the platform name is ```ios```)

**Challenge:** Add the android one too (the platform name is ```android```)

#### Running the app in emulators

To run the app on the ios simulator you have to run two commands.

1. Build the app

	```cordova build ios```

2. Launch the CFGMessenger Xcode project by double-clicking the ```.xcodeproj``` at:

	```platforms/ios/CFGMessenger.xcodeproj```
<br />
3. Selector the device you want to test on and click the "play" button

#### On Android emulators

Just like iOS, build the application for Android:

```
$ cordova build android
```

The difference here is that we will use [Genymotion](http://genymotion.com) to emulate an android devices.
It's much faster than most other Android emulator because it uses the power of [Virtual Box](https://www.virtualbox.org/).
Virtual Box allows you to designate space on your hard drive from your main Operation System...That way you can install different
operation systems on the same machine.

1. Open Genymotion
2. Add a new virtual device (select the Nexus 5)
3. Start your virtual machine by clicking on the "play" button

Now back to our PhoneGap application. In your shell, run ```adb devices``` (adb stands for Android Debug Bridge).

You'll notice we have a machine running.

Now run the classic PhoneGap command to run the app on an android device.

```
$ cordova run android
```

#### Wrap up

In this section we learned how to start a new **PhoneGap** project what approach to take for running the app on Android and iOS.

In the next section we'll start building the User Interface for CFGMessenger.

## Bonus

#### Prevent the status from collapsing on the content on iOS 7

#### Make sure the onDeviceReady event gets fired
