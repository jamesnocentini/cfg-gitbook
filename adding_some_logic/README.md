## Logic with angularJS

### Add angularJS to index.html

AngularJS 1.2.9:

`https://code.angularjs.org/1.2.9/angular.js`

**Challenge:** Add the angularJS library to your ```index.html``` using a ```script``` tag. Reload the page and make sure you have them downloaded in devtools.

### Overview of angularJS

**Challenge:** Create a ```PsstApp``` module so we can get started building the logic. Then add the directive that will indicate that the entire **html** is an Angular app.

	var app = angular.module(‘PsstApp’, []);

### AngularJS Controllers

Controllers are where we define our app’s behaviour by defining functions and values. In the HTML, we use the ```ng-controller``` directive to attach the controller to a **DOM** element in the view. We only have access to the controller inside the `div` where we defined `ng-controller`.

**Challenge:** Add a controller named `HomeController` to our `PsstApp` module.

**Solution**:

	app.controller(‘HomeController’, function() {

	});

**Challenge:** Add the `HomeController` directive to the `body` tag.

	<body ng-controller=“MessageController”>
		…
	</body>

**Challenge:** Attach a friend object to the scope in your controller

```js
$scope.friend = {
    username: 'Monster Munch'
}
```

**Challenge:** Attach the username to a new **line** component.

```html
<body ng-controller=“MessageController“>
    <div class="line">
        {{ friend.username }}
    </div>
</body>
```


**Challenge:** Change the contact object to an array of contacts like so

```js
var friends = [
    {
    	username: "Bobby"
    },
    {
    	username: "Joe",
    },
    {
        username: "Sam"
    }
];
```

And update your `html` to get the list in your view:

```html
<div ng-repeat="friend in friends" ng-click="sendMessage(friend, $index)" class="line">
    {{ friend }}
</div>
```

### Forms and Models

Now let's add a form to allow the user to register.

First we'll create a new ```user``` object in our ```HomeController```. The user will have a ```username``` property.

```javascript
$scope.user = {};
```

How do we implement a form to create a new user?

We already have the input box. Let's wrap it in a ```form``` tag.

```html
<form name="updateUser">
    <input type="text" name="username" placeholder="Type your name">
</form>
```

Now we need to bind the user object in our controller to our view. We can use the ```ng-model``` directive to bind the form element to the property.

```html
<form name="saveUser">
    <input type="text" ng-model="user.username" name="username" placeholder="Type your name">
</form>
```

Now we need to add the ability to submit the form to persist this new user on our notification service. The ```ng-submit``` directive allows us to call a function when a form is submitted.

```html
<form name="saveUser" ng-submit="sendUser()">
    <input type="text" ng-model="user.username" name="username" placeholder="Type your name">
</form>
```

Next we need to define the ```sendUser``` function inside of our controller

```javascript
$scope.sendUser = function(user) {
	console.log('Username is: ' + user.username);
}
```

Notice that when we click enter the form doesn't get cleared out. So let's do that.

```javascript
$scope.createUser = function(user) {
    console.log('Username is: ' + user.username);
    $scope.user = {};
};
```

### Services

Services will enable us to take the data out of our controllers.

We would like to fetch the array of messages from an API. For instance, ```http://api.example.com/messages.json```.

Angular comes with a bunch of built-in services to give your controllers extra functionality. Things like:

- Fetching JSON data from a web service with ```$http```
- Logging messages to the JavaScript console with ```$log```
- Filtering an array with ```$filter```

All those services begin with a ```$``` sign...because they are built-in services with angular.

The ```$http``` Service is the one we need to use to send that ```json``` data to our server. We can do that using the http ```GET``` method:

```js
$http.post('/signup', $scope.user)
     .
```

This method returns a Promise with ```.success()``` and ```.error()```. A promise object allows you to do callbacks on it.

A great feature is that if we tell ```$http``` to fetch JSON, the result will be automatically decoded into JavaScript objects and arrays.

How does a controller use a service like $http?

We use a funky array syntax:
```js
app.controller('MessageController', function($http) {

});
```

The services are arguments in our controller function.

This way of specifying the different services a controller needs is called Dependency Injection.

When angular loads it creates something called an injector. When theses built-in services load they register with the injector as being available libraries.

Then when our application loads it registers our controller with the injector telling it that when it gets executed its going to need th ```$http``` service.

Injector says cool! Then when our page loads and our controller gets used the injector grabs the services that our controller needs  and passes them in as arguemnts, It's called DI becauswe its injecting the services in the controller as arguments.

```js
app.controller('MessageController', function($http, $scope) {

	$scope.messages = [];

	$http.get('/api/messages.json')
    	.success(function(data) {
        	$scope.messages = data;
        });

});
```

