# SCSS

## Nesting Elements

SCSS is short for Sassy Cascading Stylesheets. Basically, it seems like CSS on steroids, built to handle the design needs of modern developers. The syntax is similar to CSS, and is compatible with vanilla css files. Here is an example: 

```
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

In the above code snippet, the nav element contains ul, li and a elements. (Another way to say this is that the ul, li and a elements are nested within the nav element.) The styling applied to these elements will only be applied to elements within the nav, not outside. Writing nested element stylings reduces our need to repeat ourselves, and helps keep our css DRY (Don't Repeat Yourself). Keeping code DRY is important for performance and human-readability purposes. Nobody wants to read 10 lines of the same thing.

In plain CSS, it will look like this: 

```
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```
## Intro to the Mixin

Another way that SCSS helps to keep our style code DRY is by introducing the "mixin." A mixin is similar to a variable in javascript. It holds information and applies it when called upon later on, when the style stored in the mixin is required in an element. 

This is how a mixin is written: 

```
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}
```

The mixin declares that we are creating a style holder, and the $ symbol declares our parameter, much like a function. In the above mixin, we are saying that "this mixin is called border-radius, and the thing that we are going to make open to change about this style is its radius (contained within the parenthesis), so later on, when you pass in an argument for the $radius in the parenthesis, it will be passed into everything contained within the border-radius mixin."

The way we call on this mixin is so:

```
.box { @include border-radius(10px); }
``` 

When we call the mixin above in SCSS, we are saying "for the box class, use the border-radius style with 10px." We use "@include" within our style declaration to declare that we are using a prefabricated mixin within our style declaration (instead of just statying we are using a border-radius of 10px). 

In vanilla CSS, the .box class style declaration would look like this: 
```
.box {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  border-radius: 10px;
}
```

## Extend/Inheritance

Another useful feature of SCSS is something called Extend/Inheritance. Basically, you can write a set of css properties in one selector, then "extend" those properties to other selectors to share them. 

A placeholder class goes hand-in-hand with the Extend feature. The placeholder is a class that only prints when it is extended. It helps to keep your compiled css pretty clean.
Placeholder is designated with the % symbol. 

The example: 

```
//this is the placeholder which only shows up if it is //extended
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.message {
  @extend %message-shared;
}

.success {
  @extend %message-shared;
  border-color: green;
}

.error {
  @extend %message-shared;
  border-color: red;
}

.warning {
  @extend %message-shared;
  border-color: yellow;
}
```
And here is what that same styling would look like written in vanilla CSS: 

```
.message, .success, .error, .warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

## Useful Examples of a Mixin

A useful example of using the mixin is by setting colors equal to names so that you have a set of pre-chosen theme colors you can use. This requires some pre-planning on your part. Prior planning prevents poor performance. 

For example:

```
$dark-red: rgb(159,28,25);
$happy-blue: rgb(101,209,251);
$chill-blue: rgb(5,171,235);
$mexican-house: rgb(255,133,0);
$sassy-salmon: rgb(255,36,31);
$gray: #333;
```
I named these colors with the first words that came to mind when I saw the color. They don't have any pragmatic purpose. If I wanted to name them pragmatically, I would do something like this: 

```
$nav-clr: rgb(159,28,25);
$button-clr: rgb(101,209,251);
$header-clr: rgb(5,171,235);
$important-clr: rgb(255,133,0);
$border-clr: rgb(255,36,31);
$font-clr: #333;
```
You can name the mixin colors whatever you want, as long as your team or other developers can understand them easily when you deploy. Coding isn't a competition of confusion: much the opposite.

Later on, let's say we want to use a color we already have available as a mixin. So, we can do something like this: 

```
$body-background: lighten($color-1, 90%);
```

Lighten() is a feature (as well as darken()) available in SCSS that let's us alter a pre-existing color by a certain percentage. 

Something else that we could do is hold our font-family in a mixin: 

```
$heading-font-family: 'Open Sans', sans-serif;
...
@mixin typography-sample-h4 {
  font: italic 1.5em/1.25 $heading-font-family;
  margin-bottom: .25em;
  color: $heading-color;
}
```

## A More Complex Mixin

This is a more complex mixin that creates parameters of height, width and background-color that can be changed when the mixin is called later.

```
@mixin box($height, $width, $backgroundColor) {
  height: $height
  width: $width
  background-color: $backgroundColor
  margin-bottom: 5px
  border: 1px solid black
}
...
.box-1 {
    box(100px, 200px, tomato);
    }
.box-2 {
    box(50px, 100px, rbga(100, 255, 255, 0.5);
    }
```

This allows two boxes to be made with the same border and margin on the bottom, but with different arguments for height,width and background-color. 

..."It may be a little underwhelming with only 2 boxes. But if you have a site where you need to define several similar items, Mixins can save you a ton of time. And if you need to change or add a property to all these, you have only one place to go.

...If I wanted to make these boxes into ovals by adding a border-radius property, I just do it once in the Mixin rather than for each box in my CSS."
...-https://medium.freecodecamp.org/sass-mixins-and-loops-171122499a2

SCSS also has the ability to use for loops and conditionals. 

https://egghead.io/lessons/css-loop-over-data-with-the-scss-each-control-directive

https://egghead.io/lessons/css-write-similar-classes-with-the-scss-for-control-directive

## Example of For Loop in SCSS

Here is an example of a for loop created by Dylan Macnab on codepen: https://codepen.io/DylanMacnab/pen/rLAvNQ.

It creates a series of dots that increase and decrease in size as a loading feature. 

HTML: 
```
<div class="loading-icon">
  <p>Loading...97%</p>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
  <div class="dot"></div>
</div>
```

SCSS: 
```
body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 95vh;
  background: orange;
}

.loading-icon {
  text-align: center;
  background: rgba(white, 1);
  border-radius: 5px;
  padding: 25px 35px 35px;
  box-shadow: -10px 10px 0 rgba(black, .07);
  p {
    text-transform: uppercase;
    font-size: 12px;
    letter-spacing: 4px;
    font-family: futura, sans-serif;
    color: black;
    margin-bottom: 20px;
  }
}

.dot {
  display: inline-block;
  height: 10px;
  width: 10px;
  background: orangered;
  border-radius: 50%;
  margin: 10px;
  
  animation-name: scaleUp;
  animation-duration: 1s;
  animation-timing-function: ease-in-out;
  animation-iteration-count: infinite;
}

// Used a scss for loop to calc and add animation delay to each dot
// animation duration is length of last animation delay
@for $i from 1 through 10 {
  $delay: $i * .1s;
  
  .dot:nth-of-type(#{$i}n) {
    animation-delay: $delay;
  }
}

@keyframes scaleUp{
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(3);
  }
  100% {
    transform: scale(1);
  }
}
```

The last part of the code is the for loop. I will break it down below: 

```
//@for is called the "for directive"
//$i is called the "iteration variable"
//1 through 10 tells the function to work through the end 
//of the list, not to stop at 9. That would be "1 to 10"
@for $i from 1 through 10 {
//we are setting a variable to equal the number in the order
//of the list multiplied by 1. So, the second dot in the list //would have a delay of .2 seconds, the third .3, etc. 
  $delay: $i * .1s;
```

In the continuation of the for loop, we are calling the class dot and setting whatever number the list is on when the for loop runs to run the delay variable. 

```
  .dot:nth-of-type(#{$i}n) {
    animation-delay: $delay;
  }
}
```
Then we set our keyframes.

```
@keyframes scaleUp{
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(3);
  }
  100% {
    transform: scale(1);
  }
}
```

That is the quick and dirty intro to SCSS. # Learning_SCSS
