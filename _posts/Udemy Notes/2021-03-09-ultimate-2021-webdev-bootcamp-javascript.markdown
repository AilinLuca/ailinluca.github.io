---
layout: post
title: Udemy - The Complete 2021 Web Development Bootcamp - Part 2 - Javascript Review
categories: webdev
permalink: pretty
---

Course URL: [https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12371320#overview](https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/12371320#overview)

---

### Javascript Background

- Brendan Eich helped Netscape team create a lightweight scripting language that could run in the browser, removing the need to do computations in the server.
  - He created LiveScript in 10 days!
  - Then Microsoft made JScript
  - Then Ecma international standardized it to ECMA Script
- Javascript is interpreted
  - Java is compiled
- Javascript is the language that powers the web

#### Interpreted vs Compiled

- Interpreted
  - Javascript
  - Python
  - Ruby
  - Historically slower, and compiled line by line
- Compiled
  - Java
  - C/C++
  - Swift
  - Faster

#### Javascript Resources

- Chrome Devtools
  - You can write js in the console
  - Create snippets under sources, which allows more easy multiline
- Javascript styleguide: [https://github.com/rwaldron/idiomatic.js](https://github.com/rwaldron/idiomatic.js)

#### Javascript tips

- You can only use letters, numbers, $, and \_ in variable names. No other symbols are valid.

#### Funny jokes

- Why did the software developer quit?
  - Because they didn't get arrays!
- [https://www.jitbit.com/alexblog/249-now-thats-what-i-call-a-hacker/](https://www.jitbit.com/alexblog/249-now-thats-what-i-call-a-hacker/)

---

#### The Dom

- The browser converts HTML to the DOM. It turns elements to a tree structure with document at the root, then html, and then head and body siblings.
  - HTML tree generator helps you visualize
  - There are functions to select parts of the DOM
    - `var heading = document.firstElementChild.firstElementChild;`
  - You can manipulate it once you get it
    - ` heading.innerHTML = "Hi";`

#### Selecting elements with HTML

- Methods that return a list

  - `document.getElementsByTagName("li")[2].style.color = "purple";`
    - Note this always returns a list even if there is only one item, so you must get the element with square brackets.
    - Note the plural.
  - `document.getElementsByTagName("li").length;`
  - `document.getElementsByClassName("btn");`
  - `document.querySelectorAll(#list .item")[2];`

- Methods that return a single item

  - `document.getElementById("title");`
  - `document.querySelector("li a");`
    - Note how you can combine selectors. The above is hierarchical. The child `<a>` inside the `<li>`
    - `document.querySelector("li.item");`
      - This one is not hierarchical. It is combined with a dot. It means the list item that also has a class item.
    - You only get the first item that matches the selector

- To change CSS dynamically, access properties.
  - List of HTML DOM Style Objects
    - Generally the same as CSS, except dashes replaced with camel-casing
    - Properties need to be set as strings
      - e.g. `document.querySelector("h1").style.visibility = "hidden";`
      - e.g. `document.querySelector("button").style.backgroundColor = "yellow";`

#### Separation of concerns

- `document.querySelector("button").classList`
  - Shows the classes attached
  - `document.querySelector("button").classList.add("invisible");`
    - You can add(), remove(), and toggle() classes to change styles. This is better than directly changing styles in JS.

#### Text manipulation and text content property

- .innerHTML will also get HTML tags inside the HTML object selected, so you can add HTML tags on the fly
  - e.g. `document.querySelector("h1").innerHTML = "<em>Hi</em>";`

#### Manipulating HTML attributes

- You can get attributes with `document.querySelector("a").attributes;`
  - You can select with `document.querySelector("a").getAttribute("href"); \\https://www.google.com"`
  - You can set with `document.querySelector("a").setAttribute("href", "https://www.bing.com");`
    - Note it takes the attribute and the setting

---

### The Dicee Challenge

- See on codepen
  [https://codepen.io/ailinluca/pen/KKNJPBx](https://codepen.io/ailinluca/pen/KKNJPBx)

---

### The Drumkit Challenge

- See on github
  []()

Adding event listeners:

```javascript
document.querySelectorAll(".drum").forEach((button) =>
  addEventListener("click", () => {
    alert("works");
  })
);
```

Playing audio:

```
var audio = new Audio("sound.mp3");
audio.play();

```

#### Difference between arrow functions and regular functions

Note that arrow functions have a different this (window) than the normal function. To make "this" work in referring to an item clicked, it's easier to use the regular function.

```
buttons.forEach(function (button) {
  button.addEventListener("click", function () {
    var key = this.innerHTML;
    makeSound(key);
    buttonAnimation(key);
  });
});
```

- Key differences:

  1.  Inside regular functions, _this_ is dynamic. Inside arrow functions, _this_

  - The dynamic context means that the value of _this_ depends on how the function is invoked. In JavaScript, there are 4 ways you can invoke a regular function.

    - Simple invocation, _this_ is global object or undefined.

    ```
    function myFunction() {
      console.log(this);
    }

    myFunction(); // logs global object (window)
    ```

    - Method invocation, _this_ is the object owning the method.

    ```
    const myObject = {
      method() {
        console.log(this);
      }
    };
    // Method invocation
    myObject.method(); // logs myObject
    ```

    - Indirect invocation, _this_ is the first argument.

    ```
    function myFunction() {
      console.log(this);
    }

    const myContext = { value: 'A' };

    myFunction.call(myContext);  // logs { value: 'A' }
    myFunction.apply(myContext); // logs { value: 'A' }
    ```

    - Constructor invocation using new keyword, _this_ equals to the newly created instance. So when called in a method, this is always the object.

    ```
    function MyFunction() {
      console.log(this);
    }

    new MyFunction(); // logs an instance of MyFunction
    ```

  - Meanwhile inside an arrow function, _this_ is always the _this_ value of the outer function.

  2. Arrow functions can never be used as a constructor.

  ```
  // BAD
  const Car = (color) => {
  this.color = color;
  };

  const redCar = new Car('red'); // TypeError: Car is not a constructor
  ```

  3. Inside the body of a regular function is the arguments array containing the list of arguments with which the function has been invoked.

  ```
  function myFunction() {
    console.log(arguments);
  }

  myFunction('a', 'b'); // logs { 0: 'a', 1: 'b'}
  ```

  While no arguments special keyword is defined inside an arrow function. Instead, the arguments object is resolved lexically, and access the arguments from the outer function.

  ```
  function myRegularFunction() {
  const myArrowFunction = () => {
    console.log(arguments);
  }

  myArrowFunction('c', 'd');
  }

  myRegularFunction('a', 'b'); // logs { 0: 'a', 1: 'b' }

  ```

  To access direct arguments of the arrow function, you need the rest parameters feature:

  ```
  function myRegularFunction() {
  const myArrowFunction = (...args) => {
    console.log(args);
  }

  myArrowFunction('c', 'd');
  }

  myRegularFunction('a', 'b'); // logs ['c', 'd']

  ```

  4. Regular functions do not have implicit returns. If return is not specified, the function will return undefined.

  ```
  function myEmptyFunction() {
  42;
  }

  function myEmptyFunction2() {
    42;
    return;
  }

  myEmptyFunction();  // => undefined
  myEmptyFunction2(); // => undefined
  ```

  If the arrow function contains one expression, and you omit the function’s curly braces, then the expression is implicitely returned.

  ```
  const increment = (num) => num + 1;

  increment(41); // => 42
  ```

  5. WIP

  Source: [https://dmitripavlutin.com/differences-between-arrow-and-regular-functions/](https://dmitripavlutin.com/differences-between-arrow-and-regular-functions/)

---

### jQuery

- Get CDN link

  - Google has one: https://developers.google.com/speed/libraries
  - Place above javascript tag that contains the jQuery functions, at the foot of the html body.
    - Do not put both script tags in the head, because again, the index.js may load faster than jQuery.
    - Do not put CDN in head, because things in the head load slower.
      - If you do, you must check if jQuery library is ready in index.js. (You do not need this if you just put them at the bottom of the body though.)
      ```
      $(document).ready(function() {
        $("h1").css("color", "red");
      });
      ```

#### Minification vs. Compression

- Minified removes unneccessary characters; more advanced algorithms will remove duplicate code.
- Compression alters the binary code; e.g gzip

#### jQuery functions

- Manipulating styles

  - `document.querySelector("h1");` is the same as `$("h1")`
    - jQuery gets all h1s
  - Manipulate CSS in jQuery using `$(elements).css(property, newValue)`
  - `.addClass("class");`
  - `.removeClass("class class");`
    - Can handle multiple classes, just add them in the parameter with a space inbetween
  - `.hasClass("class");`
    - Returns bool

- Manipulating text

  - `$("h1").text("New text");`
  - `$("h1").html("<em>New text</em>")'`
    - Allows you to update the inner HTML

- Manipulating attributes

  - Get the attribute: `$("img").attr("src);`
  - Set the attribute with the second parameter: `$("a").attr("href", "www.google.com");`
  - You can get classes with attr. `$("h1").attr("class");`

- Adding event listeners with jQuery

```
$("h1").click(function() {
  $("h1").css("color", "purple");
})

// equals
document.querySelector("h1").forEach(h1, function () {
  addEventListener("click", function () {
    h1.style.color = purple";
  })
});

$("body").keypress(function(e) {
  console.log(e.key);
})

// If you want to get all keypresses:
$(document).keypress(function(e) {
  console.log(e.key);
  $("h1").text(e.key);
})

```

- A more generic way to add events with jQuery is "on", which takes parameter any DOM event

```
$("h1").on("mouseover", function() {
  console.log("boop");
})
```

- Adding elements with jQuery

  - `$("h1").before("<button>Button</button>")` will create a button before the selected element
    - `<button>Button</button><h1>Hi</hi>`
  - `$("h1").after("<button>Button</button>")` will create a button after the selected element
    - `<h1>Hi</hi><button>Button</button>`
  - `$("h1").prepend("<button>Button</button>")` will create a button inside the selected element just after the opening tag
    - `<h1><button>Button</button>Hi</hi>`
  - `$("h1").append("<button>Button</button>")` will create a button inside the selected element just before the closing tag
    - `<h1>Hi<button>Button</button></hi>`

- Removing elements with jQuery

  - `$("button").remove();`
    - Removes all buttons

- Website animations with jQuery
  - These remove the elements from the flow of HTML
    - `$("h1").hide()`
    - `$("h1").show()`
    - `$("h1").toggle()`
  - These do the same as above, but more slowly
    - `$("h1").fadeOut()`
    - `$("h1").fadeIn()`
    - `$("h1").fadeToggle()`
  - These do the same as above with a collapsing animation
    - `$("h1").slideUp()`
    - `$("h1").slideDown()`
    - `$("h1").slideToggle()`
  - `.animate()` allows you to define custom animation
    ```
    //animate only accepts numeric values
    // You can chain animate functions
      $("button").on("click", function() {
        $("h1").slideUp().slideDown().animate({opacity: 0.5});
      });
    ```
