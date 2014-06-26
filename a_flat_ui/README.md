<style>
.img-center {
    text-align: center;
    display: block;
}
</style>

## Building the UI

<a class="img-center">
<img src="https://lh5.ggpht.com/s4pc3wdLiauDJ08tRBznAvB9wNTVz2UI9alBI9YLr-CWahO2SXYhsPTmqYpBbmSb1ks=w300" width=200 >
</a>

### Prepare the www folder

First let's delete the boilerplate code from the PhoneGap seed application.

1. Create a new css stylesheet in ```www/css``` and call it **styles.css**
2. Create a new script file in ```www/js``` and call it **app.js**


**Challenge:** Include those files in the ```index.html``` and reload the page in the browser.
Open devtools and make sure those new files are being fetched from the web server.

**Challenge:** Comment out the PhoneGap html boilerplate. It should look something like [this](http://cl.ly/image/3f3q453Y1y08)

### A simple list

<a class="img-center">
<img width="200" src="http://f.cl.ly/items/1p3j1f2P1z3O2x1f0A18/IMG_0084.PNG">
</a>

**Challenge:** Create a simple list of 3 random names with ```div``` elements.

**Challenge:** From looking at the screenshot of the **Yo** app who can guess want font they used?

**Challenge:** Grab it from the **Google Font API** and add it to your ```index.html```.

```html
http://fonts.googleapis.com/css?family=Montserrat:700
```
**Challenge:** Let's set up some initial styling for the ```body``` tag.

1. Set the font to ```Montserrat```
2. Capitalize all text in the body with the ```text-transfrom``` property
3. Center align all text in the body with the ```text-align``` property
4. Set the background color to be ```#9B58B7```
5. Reset the margin to ```0```
6. Set the text color to ```white```
7. Set the font size to 2em

You should see this:

<a class="img-center">
<img width="200" src="http://f.cl.ly/items/2N3j2R0c0a3S3J2W0X2C/Screen%20Shot%202014-06-20%20at%2000.54.04.png">
</a>

### Styling the lines

Let's add some padding and color to those ```div``` element.

**Challenge:** Add a ```line``` css class and add the following css classes:

1. Set the ```width``` to ```100%```
2. Set ```float``` to ```left```
3. Set ```padding``` to ```1em 0```
4. Set ```background``` to ```#19BC9D```

Check you have something like below:

<a class="img-center">
<img width="200" src="http://f.cl.ly/items/1z1w0s1V1D073W2q011k/Screen%20Shot%202014-06-20%20at%2000.57.40.png">
</a>

**Challenge:** As you can see from the **Yo** screenshot background colors for the lines alternate. Use the ```nth-child``` pseudo-class to set different background colors depending on the **index** of the row. There are 7 different colors.

1. Add a ```2n+1``` pseudo-class with a ```background-color``` of ```#2ECB70```
2. ```3n+1``` with a ```background-color``` of ```#3699DD```
3. ```4n+1``` with a ```background-color``` of ```#364860```
4. ```5n+1``` with a ```background-color``` of ```#16A087```
5. ```6n+1``` with a ```background-color``` of ```#F2C40F```
6. ```7n+1``` with a ```background-color``` of ```#297FBA```
7. ```8n+1``` with a ```background-color``` of ```#9B58B7```

<a class="img-center">
<img width="200" src="http://f.cl.ly/items/1M3L3s352s28361f1m0D/Screen%20Shot%202014-06-20%20at%2001.07.20.png">
</a>

### A minimalist input text box

Add another ```div``` with the ```.line``` just after the opening body tag.

In this ```div``` add an ```input``` element with:

- the ```type``` attribute set to ```text```
- the ```name``` attribute set to ```username```
- the ```placeholder``` attribute set to ```Type your name```

This input box needs a bit of styling. Let's add some ```css``` properties to the ```.line input``` css selector:

```css
.line input {
    font-size: 1em;
    background: none;
    border: none;
    max-width: 90%;
    text-align: center;
    color: #ffffff;
    text-transform: uppercase;
}
```
