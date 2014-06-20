### Dependencies and Services

Our ```app.js``` file starting to get a little bit long. Directives, controllers are all in the same file. We need a way to organise this code.

We are going to start refactor some of our code into their own modules.

Let's create a message module to store message related directive

```js
	(function() {
    	var app = angular.module('message-module', []);

        app.directive('message', function() { ... });

        app.directive('message', function() { ... });

        app.directive('message', function() { ... });

    }())
```

**Note:** Diifferent closure means different app variable.

Now we need to tell our ```messageApp``` module that it depends on the ```message``` module.

```js
	var app = angular.module('messageApp', ['message-module']);
```

Don't forget to include the ```script``` tag in ```index.html```



### More with Directives

Let's add some more interactivity to our application by adding some tabs. Twitter bootstrap comes with CSS classes that make it pretty easy to build tabs.

```html
<section>
	<ul class="nav mav-pills">
    	<li><a>Contacts</a></li>
        <li><a>Messages</a></li>
        <li><a>Profile</a></li>
    </ul>
</section>
```

Now we need to add behaviour to our tabs. One way to do this with angularJS is to use the ng-click directive. It takes an expression.

Copy paste the snippet below and see how clicking updates the value of tab.

```html
<section>
	<ul class="nav mav-pills">
    	<li><a ng-click="tab = 1">Contacts</a></li>
        <li><a ng-click="tab = 2">Messages</a></li>
        <li><a ng-click="tab = 3">Profile</a></li>
    </ul>
</section>
{{ tab }}
```

When ```ng-click``` changes the value of tab, the ```{{tab}}``` expression automagically get updated!

Expressions like this define 2-way data binding. This means that expression are reavuation when a property changes.

Now let's add the tab content panels.

```html
<div class="panel">
	<h4>Contacts</h4>
    <p></p>
</div>
<div class="panel">
	<h4>Messages</h4>
    <p></p>
</div>
<div class="panel">
	<h4>Profile</h4>
    <p></p>
</div>
```

How do we make the tabs trigger the panel to show?

We can use ng-show again and this time use an expression

```html
<div class="panel" ng-show="tab === 1">
	<h4>Contacts</h4>
    <p></p>
</div>
<div class="panel" ng-show="tab === 2">
	<h4>Messages</h4>
    <p></p>
</div>
<div class="panel" ng-show="tab === 3">
	<h4>Profile</h4>
    <p></p>
</div>
```

Now when a tab is selected it's going to show the appropriate panel.

But you will notice that when we refresh the page, none of the panel are shown. How do we set an initial value for tab?

The ```ng-init``` directive allows us to evaluate an expression in the current scope.

```html
<section ng-init="tab = 1">
	<ul class="nav mav-pills">
    	<li><a ng-click="tab = 1">Contacts</a></li>
        <li><a ng-click="tab = 2">Messages</a></li>
        <li><a ng-click="tab = 3">Profile</a></li>
    </ul>
</section>
```

Wouldn't it be nice to detect which tab is active? As you know Bootstrap has a class ```active``` if the current tab is selected.

To do this we need to use the ```ng-class``` directive and we pass in an object. As the key the class to add and as the value the expression to evaluate. If the expression is true, we'll append this class to the element.

```html
<section ng-init="tab = 1">
	<ul class="nav mav-pills">
    	<li ng-class="{active:tab === 1}">
        	<a ng-click="tab = 1">Contacts</a>
        </li>
        <li ng-class="{active:tab === 2}">
        	<a ng-click="tab = 2">Messages</a>
        </li>
        <li ng-class="{active:tab === 3}">
        	<a ng-click="tab = 3">Profile</a>
        </li>
    </ul>
</section>
```

Now you can in the browser that the tab is highlighted when we click on one of them.



## Form validation

AngularJS has great hooks to help us add validation to our form. That we can let the user know if he mistyped some information.

First we need to use the ```attribute``` to prevent the browser's default validation behaviour. Add it to the ```form``` tag.

Mark the fields at required:

```html
<form name="createUser" ng-submit="createUser(user)">
	<label>Username</label>
    <input type="text" ng-model="user.username" required />
    <label>Email</label>
    <input type="email" ng-model="user.email" required />
    <label>Avatar</label>
    <input type="file" ng-model="user.avatar" required />
</form>
```

Prevent the submit the form is not valid:

```html
<form name="createUser" ng-submit="createUser.$valide && createUser(user)">
	<label>Username</label>
    <input type="text" ng-model="user.username" required />
    <label>Email</label>
    <input type="email" ng-model="user.email" required />
    <label>Avatar</label>
    <input type="file" ng-model="user.avatar" required />
</form>
```

Let's style our elements to give a visual feedback to the user.

For elements with both the ```ng-invalid``` and ```ng-dirty``` classes, give the ```border-color``` of red.

```css
.ng-invalid.ng-dirty {
	border-color: red;
}
```

For element with both ```ng-dirty``` and ```ng-valid``` classes, give a green border.

```css
.ng-valid.ng-dirty{
	border-color: green;
}
```

## Creating our first custom Directive

Using ```ng-include```:

```html
<h3 ng-include="'product-title.html'"></h3>
```

Custom directive:

```html
<product-title></product-title>
```

Why should you write your custom directive instead of using ```ng-include```?

Directives allow you to write HTML that expresses the behaviour of your application.

Expressivness is the real power of writing custom directives. And there are several types of directives you can use:

- Template-expanding Directives. They are the simplest:
	- define a custom tag or attribute that is expanded or replaced. And can include controller logic if needed.
- For expressing complex UI
- Calling events and registering event handlers
- Reusing common components

But for this workshop we'll stick to the first, most basic way of using custom Directives.

How to Build Custom Directives?

```html
<message></message>
```

```js
app.directive('message', function() {
	return {
    	//A configuration object defining how your directive will work
    };
});
```

We'll use two configuration properties for this directive:

```js
app.directive('message', function() {
	return {
    	restrict: 'E',
        templateUrl: 'message.html'
    };
});
```

The ```restrict``` property configuration is where we specify the type of directive. In this case, ```E``` stands for ```Element```. Then we'll specify the url of a template we want this directive to load onto the page. In this case, ```message.html```.

We could also write this Directive as an ```attribute``` directive and it might look like this:

```html
	<p message></p>
```

It's a good practice to use Element Directives for UI widgets and Attribute Directives for mixin behavious...like a tooltip.

In the case of an ```atribute``` directive we'd specify ```restrict: 'A'```.

When you think of a dynamic web application, do you think you'll be able to understand the functionality just by looking at the HTML?

Typically not.

And this is way angular is so awesome.

It allows you to write expressive ```HTML``` through custom directives.

So when you're reading an AngularJS application, you should be able to understand the behaviour and intent from just the ```HTML```.

**Challenge 10:** Separate our message into ```message.html``` and the ```ng-include``` directive to import the template back.

**Challenge 11:** Create an ```Element``` Directive for ```Message``` div that includes the message.html

```js
app.directive('message', function() {
	return {
    	restrict: 'E',
        templateUrl: 'message.html'
    }
});
```

#### Directive with controllers

In this section we are going to add another Directive. And this time we will attach a controller to it.

We can use the ```controller``` configuration property to do so.

```js
app.directive('message', function() {
	return {
    	restrict: 'E',
        templateUrl: 'message.html',
        controller: function() {

        },
        controllerAs: 'message'
    };
});
```

**Note:** The ```controllerAs``` configuration property stands for the alias.



#### Strong and Emphasize

**strong** or __strong__ ( Cmd + B )

*emphasize* or _emphasize_ ( Cmd + I )

**Sometimes I want a lot of text to be bold.
Like, seriously, a _LOT_ of text**

#### Blockquotes

> Right angle brackets &gt; are used for block quotes.

#### Links and Email

An email <example@example.com> link.

Simple inline link <http://chenluois.com>, another inline link [Smaller](http://smallerapp.com), one more inline link with title [Resize](http://resizesafari.com "a Safari extension").

A [reference style][id] link. Input id, then anywhere in the doc, define the link with corresponding id:

[id]: http://mouapp.com "Markdown editor on Mac OS X"

Titles ( or called tool tips ) in the links are optional.

#### Images

An inline image ![Smaller icon](http://smallerapp.com/favicon.ico "Title here"), title is optional.

A ![Resize icon][2] reference style image.

[2]: http://resizesafari.com/favicon.ico "Title"

#### Inline code and Block code

Inline code are surround by `backtick` key. To create a block code:

	Indent each line by at least 1 tab, or 4 spaces.
    var Mou = “exactlyTheAppIwant”;

####  Ordered Lists

Ordered lists are created using "1." + Space:

1. Ordered list item
2. Ordered list item
3. Ordered list item

#### Unordered Lists

Unordered list are created using "*" + Space:

* Unordered list item
* Unordered list item
* Unordered list item

Or using "-" + Space:

- Unordered list item
- Unordered list item
- Unordered list item

#### Hard Linebreak

End a line with two or more spaces will create a hard linebreak, called `<br />` in HTML. ( Control + Return )
Above line ended with 2 spaces.

#### Horizontal Rules

Three or more asterisks or dashes:

***

---

- - - -

#### Headers

Setext-style:

This is H1
==========

This is H2
----------

atx-style:

# This is H1
## This is H2
### This is H3
#### This is H4
##### This is H5
###### This is H6


### Extra Syntax

#### Footnotes

Footnotes work mostly like reference-style links. A footnote is made of two things: a marker in the text that will become a superscript number; a footnote definition that will be placed in a list of footnotes at the end of the document. A footnote looks like this:

That's some text with a footnote.[^1]

[^1]: And that's the footnote.


#### Strikethrough

Wrap with 2 tilde characters:

~~Strikethrough~~


#### Fenced Code Blocks

Start with a line containing 3 or more backticks, and ends with the first line with the same number of backticks:

```
Fenced code blocks are like Stardard Markdown’s regular code
blocks, except that they’re not indented and instead rely on
a start and end fence lines to delimit the code block.
```

#### Tables

A simple table looks like this:

First Header | Second Header | Third Header
------------ | ------------- | ------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell

If you wish, you can add a leading and tailing pipe to each line of the table:

| First Header | Second Header | Third Header |
| ------------ | ------------- | ------------ |
| Content Cell | Content Cell  | Content Cell |
| Content Cell | Content Cell  | Content Cell |

Specify alignement for each column by adding colons to separator lines:

First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right


### Shortcuts

#### View

* Toggle live preview: Shift + Cmd + I
* Toggle Words Counter: Shift + Cmd + W
* Toggle Transparent: Shift + Cmd + T
* Toggle Floating: Shift + Cmd + F
* Left/Right = 1/1: Cmd + 0
* Left/Right = 3/1: Cmd + +
* Left/Right = 1/3: Cmd + -
* Toggle Writing orientation: Cmd + L
* Toggle fullscreen: Control + Cmd + F

#### Actions

* Copy HTML: Option + Cmd + C
* Strong: Select text, Cmd + B
* Emphasize: Select text, Cmd + I
* Inline Code: Select text, Cmd + K
* Strikethrough: Select text, Cmd + U
* Link: Select text, Control + Shift + L
* Image: Select text, Control + Shift + I
* Select Word: Control + Option + W
* Select Line: Shift + Cmd + L
* Select All: Cmd + A
* Deselect All: Cmd + D
* Convert to Uppercase: Select text, Control + U
* Convert to Lowercase: Select text, Control + Shift + U
* Convert to Titlecase: Select text, Control + Option + U
* Convert to List: Select lines, Control + L
* Convert to Blockquote: Select lines, Control + Q
* Convert to H1: Cmd + 1
* Convert to H2: Cmd + 2
* Convert to H3: Cmd + 3
* Convert to H4: Cmd + 4
* Convert to H5: Cmd + 5
* Convert to H6: Cmd + 6
* Convert Spaces to Tabs: Control + [
* Convert Tabs to Spaces: Control + ]
* Insert Current Date: Control + Shift + 1
* Insert Current Time: Control + Shift + 2
* Insert entity <: Control + Shift + ,
* Insert entity >: Control + Shift + .
* Insert entity &: Control + Shift + 7
* Insert entity Space: Control + Shift + Space
* Insert Scriptogr.am Header: Control + Shift + G
* Shift Line Left: Select lines, Cmd + [
* Shift Line Right: Select lines, Cmd + ]
* New Line: Cmd + Return
* Comment: Cmd + /
* Hard Linebreak: Control + Return

#### Edit

* Auto complete current word: Esc
* Find: Cmd + F
* Close find bar: Esc

#### Post

* Post on Scriptogr.am: Control + Shift + S
* Post on Tumblr: Control + Shift + T

#### Export

* Export HTML: Option + Cmd + E
* Export PDF:  Option + Cmd + P


### And more?

Don't forget to check Preferences, lots of useful options are there.

Follow [@chenluois](http://twitter.com/chenluois) on Twitter for the latest news.

For feedback, use the menu `Help` - `Send Feedback`
