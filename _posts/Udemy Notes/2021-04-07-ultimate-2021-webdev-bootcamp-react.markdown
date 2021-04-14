---
layout: post
title: Udemy - The Complete 2021 Web Development Bootcamp - Part 8 - React
categories: webdev
permalink: pretty
---

Course URL: (https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/17038306#notes)[https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/lecture/17038306#notes]

---

### React Intro

- React is one of the top web frameworks
- A frontend framework that makes it easier and faster to build frontend frameworks
- Build user infrastructure into a component tree
  - Views can be reused and customized

#### Resource: Code Sandbox

- Lets you build and deploy within the same application: [https://codesandbox.io/s/infallible-mahavira-2kx8g](https://codesandbox.io/s/infallible-mahavira-2kx8g)

---

### Intro to JSX

Module: [https://codesandbox.io/s/introduction-to-jsx-h8ub1?fontsize=14&module=%2Fpublic%2Findex.html%20babeljs.io](https://codesandbox.io/s/introduction-to-jsx-h8ub1?fontsize=14&module=%2Fpublic%2Findex.html%20babeljs.io)

- Every React app has an index.html with a single div with id="root".

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>JSX</title>
    <link rel="stylesheet" href="styles.css" />
  </head>

  <body>
    <div id="root"></div>
    <script src="../src/index.js" type="text/javascript"></script>
  </body>
</html>
```

- In index.js, a basic script would look like this:

```
var React = require("react");
var ReactDOM = require("react-dom");

// ReactDOM.render(WHAT TO RENDER, WHERE TO RENDER)
ReactDOM.render(<h1>Hello World</h1>, document.getElementById("root"));

// render() can only take ONE HTML element; the below will crash
ReactDOM.render(<h1>Hello World</h1><p>This is a paragraph</p>, document.getElementById("root"));

// So you just wrap everything in one div
ReactDOM.render(<div><h1>Hello World</h1><p>This is a paragraph</p></div>, document.getElementById("root"));
```

- React works by creating jsx files that are converted to regular javascript code.
- Inside the React module is Babel, a javascript compiler, that compiles all versions of javascript to a langauge that the browser understands. It coverts jsx to javascript.
- Babel converts the above to:

```
"use strict";

ReactDOM.render(React.createElement("h1", null, "Hello World"),
document.getElementById("root"));

```

- Which in turn is less verbose than

```
var h1 = document.createElement("h1");
h1.innerHTML = "Hello World!";
document.getElementById("root").appendChild(h1);

```

- Babel also lets us use modern Javascript and makes it compatible with old versions of internet explorer, for example.

---

### Modern Javascript!

```
var React = require("react"); // OLD!
import React from "react"; // MODERN!

```

---

### Insert variables to the HTML elements

- Wrap the variable around curly braces.

```
const num = 11;
ReactDOM.render(
  <h1>Your lucky number is {num}</h1>,
  document.getElementById("root")
);

```

- Inside curly braces, you can put in any javascript. E.g.

```
// Expressions are valid
{1 + 3}
{Math.floor(Math.random()*10)}

// Statements however are NOT VALID
{if (name === "Angela") {
    return 7;
} else if (name === "Jack") {
    return 12;
}} // BAD

// Note you are not limited to a single set of curly braces
const fName = "Jane";
const lName = "Doe";

<h1>Hello {fname} {lname}!</h1> // renders "Hello Jane Doe!"
<h1>Hello {fname + " " + lname}!</h1> // is the same
<h1>Hello {`${fname} ${lname}`}!</h1> // is also the same

```

---

### JSX Attribute variation:

#### Insert classes with className

Although React will render "class" correctly most of the time, the correct attribute name is className.

#### Attributes are camelCase in JSX

- In React, if the attribute is multiword, it should be camelCase, although in HTML it is typically all lower case. This is a key difference.
- Styling is still recommended to be applied via classes.
- Targeting in CSS is still the same.
- See a list of global attributes here: (https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)[https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes]

```
ReactDOM.render(
  <div>
    <h1 className="heading" contentEditable="true">My Favorite Foods</h1>
  </div>
)

```

---

Misc. Notes on JSX

#### Specify JSX file in index.html

```
// ...
<body>
    <div id="root></div>
    <script src="../src/index.js" type="text/JSX"></script>
</body>

```

#### JSX requires closing tags on self-terminating tags

- While HTML is much more forgiving

---

### Inline styling in React

- React styles attribute accepts Javascript objects, and they must be presented in key-value pairs, hence 2 sets of curly brackets.
- Remember that css properties are kebab cased, but when passed in as a Javascript object in react, they must be camelCased and the values must be strings.

```
import React from "react"
import ReactDOM from "react-dom"

ReactDOM.render(<h1 style={{color: "red"}}>Hello World!</h1>, document.getElementByID("root"))

```

- For more complicated styling, you can create a separate variable to store the styles.

```
import React from "react"
import ReactDOM from "react-dom"

const customStyle = {
  color: "red",
  fontSize: "20px",
  border: "1px solid black"
}

customStyle.color = "blue"; // will update the customStyle color property on the go

ReactDOM.render(<h1 style={customStyle}>Hello World!</h1>, document.getElementByID("root"))

```

---

### React components

To convert the following to components:

- Before:

```
import React from "react"
import ReactDOM from "react-dom"

ReactDOM.render(
  <div>
    <h1>My Favorite Food</h1>
    <ul>
      <li>Bubble tea</li>
      <li>Ethiopian beef</li>
      <li>Spicy beef empanadas</li>
    </ul>
  </div>,
  document.getElementById("root")
)

```

- After:

```
import React from "react"
import ReactDOM from "react-dom"

function Heading() {
  return <h1>My Favorite Foods</h1>
}

ReactDOM.render(
  <div>
    <Heading/>
    <ul>
      <li>Bubble tea</li>
      <li>Ethiopian beef</li>
      <li>Spicy beef empanadas</li>
    </ul>
  </div>,
  document.getElementById("root")

```

- Note custom component names are capitalized
- In React, if there is no children in HTML, just make it self-closing.
- The Airbnb guide is generally accepted: (https://github.com/airbnb/javascript/tree/master/react)[https://github.com/airbnb/javascript/tree/master/react]

- You should move these components to separate files:

```
// in Heading.jsx
import React from "react"

function Heading() {
  return <h1>My Favorite Foods</h1>
}

export default Heading


// in List.jsx
function List() {
  return (
    <ul>
      <li>Bubble tea</li>
      <li>Ethiopian beef</li>
      <li>Spicy beef empanadas</li>
    </ul>
  )
}

export default List


// in index.js

import React from "react"
import ReactDOM from "react-dom"
import Heading from "./Heading"
import List form "./List"

ReactDOM.render(
  <div>
    <Heading />
    <List />
  </div>,
  document.getElementById("root")

```

- Most React apps do not have many components in index.js, just a single component called <App />
- It is best practice to organize components. Move all to a components folder under src. You can add subcategories like home page components, etc.

- You can export other things besides react components e.g.

```
// in math.js
const pi = 3.1415962;

function doublePi() {
  return pi * 2;
}

function triplePi() {
  return pi * 3;
}

// the default export is what is imported by default when another file imports the module
export default pi;

// These are also available, but are not default so when importing, you must specify them by name in brackets, see below
export {doublePi, triplePi};


// in app.js

import React from "react"
import ReactDOM from "react-dom"
import PI, { doublePi, triplePi} from "./math.js"
// you can also import everything with
// import * as pi from "./math.js"
// which will import all pi as an object
// and you can use pi.default, pi.doublePi()
// but the wildcard removes the usefulness of a single default export
// so it is discouraged in many style guides

ReactDOM.render(
  <h1>{PI}</h1>
  <h2>{doublePi()}</h2>
)

```

- When importing you have the following options
  - Node.js require: `const React = require("react");`
    - Node has stronger browser penetration (though this may be changing as browsers modernize)
  - ES6 import/export: `import React from "react";`
    - Babel converts this to the above CommonJS
    - ES6 is the standard
    - The import/export syntax is more clear

---

### Getting started with React

1. Check you have up-to-date (latest stable) Node.js with ` $ node --version`
2. Install VSCode
3. Create React App ` $ npx create-react-app my-app`

- Will download react-scripts which helps run app locally
- Delete templates you don't need
- Default Hello World app:

```
<!-- in index.html -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <title>React App</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="./src/index.js" type="text/jsx"></script>
  </body>
</html>

// in index.js
import React from "react"
import ReactDOM from "react-dom"

ReactDOM.render(
  <h1>Hello World!</h1>,
  document.getElementById("root")
)
```

---

### React Props

React props help remove repetitive elements. You set up props in the component and reference them within the component, and you set the individual values for props where the component is referenced.

- Example before props:

```
// index.js
import React from "react";
import ReactDOM from "react-dom";

ReactDOM.render(
  <div>
    <h1>My Contacts</h1>

    <h2>Beyonce</h2>
    <img
      src="https://blackhistorywall.files.wordpress.com/2010/02/picture-device-independent-bitmap-119.jpg"
      alt="avatar_img"
    />
    <p>+123 456 789</p>
    <p>b@beyonce.com</p>

    <h2>Jack Bauer</h2>
    <img
      src="https://pbs.twimg.com/profile_images/625247595825246208/X3XLea04_400x400.jpg"
      alt="avatar_img"
    />
    <p>+987 654 321</p>
    <p>jack@nowhere.com</p>

    <h2>Chuck Norris</h2>
    <img
      src="https://i.pinimg.com/originals/e3/94/47/e39447de921955826b1e498ccf9a39af.png"
      alt="avatar_img"
    />
    <p>+918 372 574</p>
    <p>gmail@chucknorris.com</p>
  </div>,
  document.getElementById("root")
);

```

- After props

```
// Card.jsx

import React from "react";

function Card(props) {
  return (
    <div>
      <div className="card">
        <div className="top">
          <h2 className="name">{props.name}</h2>
          <img className="circle-img" src={props.img} alt="avatar_img" />
        </div>
        <div className="bottom">
          <p className="info">{props.phone}</p>
          <p className="info">{props.email}</p>
        </div>
      </div>
    </div>
  );
}

export default Card;


// contacts.js

const contacts = [
  {
    name: "Beyonce",
    imgURL:
      "https://blackhistorywall.files.wordpress.com/2010/02/picture-device-independent-bitmap-119.jpg",
    phone: "+123 456 789",
    email: "b@beyonce.com"
  },
  {
    name: "Jack Bauer",
    imgURL:
      "https://pbs.twimg.com/profile_images/625247595825246208/X3XLea04_400x400.jpg",
    phone: "+987 654 321",
    email: "jack@nowhere.com"
  },
  {
    name: "Chuck Norris",
    imgURL:
      "https://i.pinimg.com/originals/e3/94/47/e39447de921955826b1e498ccf9a39af.png",
    phone: "+918 372 574",
    email: "gmail@chucknorris.com"
  }
];

export default contacts;


// App.jsx
import React from "react";
import Card from "./Card";
import contacts from "../contacts";

function createContact(contact) {
  return (
    <Card
      key={contact.id}
      name={contact.name}
      img={contact.imgURL}
      phone={contact.phone}
      email={contact.email}
    />
  );
}

function App() {
  return (
    <>
      <h1 className="heading">My Contacts</h1>
      <div className="contacts">
        {contacts.map(createContact)}
      </div>
    </>
  );
}

export default App;


// App.js v2, if you don't like creating a separate function to create a contact
import React from "react";
import Card from "./Card";
import contacts from "../contacts";

function createContact(contact) {
  return
}

function App() {
  return (
    <>
      <h1 className="heading">My Contacts</h1>
      <div className="contacts">
        {contacts.map(contact => (
          <Card
            key={contact.id}
            name={contact.name}
            img={contact.imgURL}
            phone={contact.phone}
            email={contact.email}
          />
        ))}
      </div>
    </>
  );
}

export default App;


// index.js
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";

ReactDOM.render(<App />, document.getElementById("root"));


```

- Do not forget to include a unique key for every component you generate with a loop
  - Can be string or number
  - Note you cannot render it with {props.key}. Instead you should set another prop with the same value
- You can make as many props as you wish.
- Note that you must apply className directly to the component HTML template, so it is not mistaken as a prop.

- You want to separate out anything that may be reusable and may be repeated, like the profile image and the info lines. After refactoring:

```
// ProfileImage.jsx
import React from "react"

function ProfileImage(props) {
  return (
    <img className="circle-img" src={props.img} alt="avatar_img" />
  )
}

export default ProfileImage


// Info.jsx
import React from "react";

function Info(props) {
  return <p className="info">{props.info}</p>;
}

export default Info;


// Card.jsx
import React from "react";
import ProfileImage from "./ProfileImage";
import Info from "./Info";

function Card(props) {
  return (
    <div className="card">
      <div className="top">
        <h2 className="name">{props.name}</h2>
        <ProfileImage img={props.img} />
      </div>
      <div className="bottom">
        <Info info={props.tel} />
        <Info info={props.email} />
      </div>
    </div>
  );
}

export default Card;
```

---

### React DevTools

- Add the React DevTools extension to your browser
- This add on will be visible in your devtools, and you will see a new Components tab
- It will show you components tree and the props passed between them
- You can use it on deployed React websites like airbnb.com
- There is a setting that lets you also see the HTML
- Source tab will show you the source code for the component

---

#### Emoji Dictionary Project Notes

- The <dl> tag, aka Description List, encloses a list of groups of terms specified by the <dt> tag and their descriptions specified by the <dd> tag.
  - Commonly used to implement a glossary or display metadata
  - Formatted like a dictionary

---

### Map, Filter, Reduce Notes

- Map() returns a new array produced by mapping a function onto it
  - Unlike forEach, which mutates the original array data if not explicitly told to push to a new array
- Filter() returns a new array containing only truthy values
- Reduce() accumulates a value
- Find() finds the first item that matches from an array
  - e.g.
    numbers.find((num) => {
    num > 10;
    })
- FindIndex() will behave the same as Find but returns the index

---

### Arrow functions in dept

- (https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/)[https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/]

---

### Conditional Rendering with Ternery Flow

- e.g. when user is not logged in, show login form, else not.

```
function App() {
  return isLoggedIn ? <h1>Hello</h1> : <Login />
}
```

#### You can also render conditionally with &&

```
function App() {
  return x > 5 && <h1>True</h1> />
}
```

- Will evaluate right-hand side only if true, else will not render anything at all.

---

### Declarative vs. Imperative Programming in React

- Declarative Programming

  - expresses the logic of a computation without describing its control flow.
  - declarative programming can result in less direct control, which may not be correct for applications like embedded systems where “the right answer delivered too late becomes the wrong answer.”

  ```
  class Button extends React.Component{
  this.state = { color: 'red' }
  handleChange = () => {
    const color = this.state.color === 'red' ? 'blue' : 'red';
    this.setState({ color });
  }
  render() {
    return (<div>
      <button
         className=`btn ${this.state.color}`
         onClick={this.handleChange}>
      </button>
    </div>);
    }
  }

  ```

- Imperative Programming

  - uses statements that change a program’s state.
  - Not used as much in React
    - In React, good practice to avoid Refs and avoid manipulating DOM directly

  ```
  const container = document.getElementById(‘container’);
  const btn = document.createElement(‘button’);
  btn.className = ‘btn red’;
  btn.onclick = function(event) {
  if (this.classList.contains(‘red’)) {
    this.classList.remove(‘red’);
    this.classList.add(‘blue’);
  } else {
    this.classList.remove(‘blue’);
    this.classList.add(‘red’);
  }
  };
  container.appendChild(btn);
  ```

```
Other guidance from (https://codeburst.io/declarative-vs-imperative-programming-a8a7c93d9ad2)[https://codeburst.io/declarative-vs-imperative-programming-a8a7c93d9ad2]:

- Instead of returning functions that render a component, prefer to return functions that return the necessary information to render a component. In the first we are instructing what to do(render precisely this thing), while in the second we’re just returning some information (use this information to do something).
- Communicating between siblings, instead of through components. Try to only communicate with other components through props.
- Use pure functional components where possible. Because these components don’t have lifecycle methods, they require you to rely on a declarative, props-based approach. And they can also provide performance improvements.

```

---

### React Hooks - UseState

Clock App Example

```
import React, { useState } from "react";

function App() {
  setInterval(updateTime, 1000);

  const now = new Date().toLocaleTimeString();
  const [time, setTime] = useState(now);

  function updateTime() {
    const newTime = new Date().toLocaleTimeString();
    setTime(newTime);
  }

  return (
    <div className="container">
      <h1>{time}</h1>
      <button onClick={updateTime}>Get Time</button>
    </div>
  );
}

export default App;

```

---

### Destructuring

- Names must be unique when you destructure

- Note you can also rename the properties you pull out by adding your name after the original name + :

  - e.g. ` const {name: catName, sound: catSound} = cat;`

- You can set a default in the case the value is not defined in the object that is being destructured

  - e.g. ` const {name = "Fluffy", sound = "Purr"} = cat;`

- To destructure a nested object, you can just destructure to another object

  - e.g. `const { name, sound, feedingRequirements: {food, water}} = cat `

- Note that useState is likely structured like so:

```
  // in data
  function useAnimals(animal) {
    return [
      animal.name,
      function() {
        console.log(animal.sound)
      }
    ]
  }

  export {useAnimals};

  // in app.js

  import {useAnimals} from "./data"

  const [animal, makeSound] = useAnimals(cat);

  console.log(animal); // cat
  makeSound(); // meow

```

- Sample destructuring complex objects

```
// in practice.js
const cars = [
  {
    model: "Honda Civic",
    //The top colour refers to the first item in the array below:
    //i.e. hondaTopColour = "black"
    coloursByPopularity: ["black", "silver"],
    speedStats: {
      topSpeed: 140,
      zeroToSixty: 8.5
    }
  },
  {
    model: "Tesla Model 3",
    coloursByPopularity: ["red", "white"],
    speedStats: {
      topSpeed: 150,
      zeroToSixty: 3.2
    }
  }
];

export default cars;


// in index.js
// CHALLENGE: uncomment the code below and see the car stats rendered
import React from "react";
import ReactDOM from "react-dom";
import cars from "./practice";

const [honda, tesla] = cars;
const {
  speedStats: { topSpeed: hondaTopSpeed }
} = honda;
const {
  speedStats: { topSpeed: teslaTopSpeed }
} = tesla;
const {
  coloursByPopularity: [hondaTopColour]
} = honda;
const {
  coloursByPopularity: [teslaTopColour]
} = tesla;

ReactDOM.render(
  <table>
    <tr>
      <th>Brand</th>
      <th>Top Speed</th>
    </tr>
    <tr>
      <td>{tesla.model}</td>
      <td>{teslaTopSpeed}</td>
      <td>{teslaTopColour}</td>
    </tr>
    <tr>
      <td>{honda.model}</td>
      <td>{hondaTopSpeed}</td>
      <td>{hondaTopColour}</td>
    </tr>
  </table>,
  document.getElementById("root")
);
```

---

### Event Handling in React

#### Mouseover event styling

- useState to change mouseover styling of button

```
import React, { useState } from "react";

function App() {
  const [isMousedOver, setMousedOver] = useState(false);

  // function handleMouseOver() {
  //   setMousedOver(true);
  // }

  // function handleMouseOut() {
  //   setMousedOver(false);
  // }

  function handleMouseEvent() {
    setMousedOver(!isMousedOver);
  }

  return (
    <div className="container">
      <h1>Hello</h1>
      <input type="text" placeholder="What's your name?" />
      <button
        onMouseOver={handleMouseEvent}
        onMouseOut={handleMouseEvent}
        style={{ backgroundColor: isMousedOver ? "black" : "white" }}
      >
        Submit
      </button>
    </div>
  );
}

export default App;


```

---

### React Forms

- You can get the information entered in input with an onChange function
  - For example, you can log what has been entered with `console.log(event.target.value)`
  - This will log every single letter you add.
- Be sure to set the value of inputs explicitly if you use useState. This is a controlled component. See docs: (https://reactjs.org/docs/forms.html)[https://reactjs.org/docs/forms.html]

```
import React from "react"

function App() {
  const [name, setName] = useState("");
  const [heading, setHeading] = useState("");

  function handleChange(event) {
    console.log(event.target.value) // what is in the input
    console.log(event.target.placeholder) // placeholder "What's your name?"
    console.log(event.target.type)  // type of input: text

    setName(event.target.value) // updates name in header with each keypress
  }

  function handleClick() {
    setHeading(name); // note that in a function, do not use {} to refer to a variable, or you will pass in a JS object instead of just text
  }

  return (
    <h1>Hello {heading}</h1>
    <input
      onChange={handleChange}
      type="text"
      placeholder="What's your name?"
      value={name}
    />
    <button onClick={handleClick}>Submit</button>
  );
}

export default App;

```

- Default behavior of buttons in form is to refresh the page. As a result, the below code displays the same behavior as the above:

```
import React from "react"

function App() {
  const [name, setName] = useState("");
  const [heading, setHeading] = useState("");

  function handleChange(event) {
    setName(event.target.value) // updates name in header with each keypress
  }

  function handleClick() {
    setHeading(name);
    event.preventDefault(); // this will stop the page from refreshing
  }

  return (
    <form onSubmit={handleClick}>
      <h1>Hello {heading}</h1>
      <input
        onChange={handleChange}
        type="text"
        placeholder="What's your name?"
        value={name}
      />
      <button>Submit</button>
    </form>
  );
}

export default App;
```

---

### Class components vs. functional components

- Functional

```
import React from "react";

function App() {
  return <h1>Hello</h1>
}

export default App;
```

- Class

```
import React from "react";

class App extends React.Component {
  render() {
    return <h1>Hello</h1>
  }
}

export default App;
```

- Classes used to be required for state
- This has been replaced with hooks, which are more concise and easier to read

- Class state example:

```
import React from "react";

class ClassComponent extends React.Component {
  constructor() {
    super();
    this.state = {
      count: 0
    };
    this.increase = this.increase.bind(this);
  }

  increase() {
    this.setState({count: this.state.count + 1})
  }
  render() {
    return (
      <div>
        <h1>{this.state.count}</h1>
        <button onClick={this.increase}>+</button>
      </div>
    )
  }
}

export default ClassComponent;
```

- Function hook example:

```
import React, {useState} from "react";

function FunctionComponent ()) {
  [count, setCount] = useState(0);

  function increase() {
    setState(count + 1);
  }
  render() {
    return (
      <div>
        <h1>{count}</h1>
        <button onClick={increase}>+</button>
      </div>
    )
  }
}

export default FunctionComponent;

```

---

### Changing Complex State

- You can set a state to be an object
- Note that every time you setState() it will create the object fresh and erase any previous key values.

  - To go around this, you need to pass in the previous value

  ```
  import React, { useState } from "react";

  function App() {
    const [name, setName] = useState({
      fname: "",
      lname: ""
    });

    function handleChange(event) {
      const { value, name } = event.target; // where value is input text value, name is input text name
      // note you must never reference these React SyntheticEvents inside of state setters
      setName((prevValue) => {
        if (name === "fname") {
          return {
            fname: value,
            lname: prevValue.lname
          };
        } else if (name === "lname") {
          return {
            fname: prevValue.fname,
            lname: value
          };
        }
      });
    }

    return (
      <div className="container">
        <h1>
          Hello {name.fname} {name.lname}
        </h1>
        <form>
          <input
            onChange={handleChange}
            name="fname"
            placeholder="First Name"
            value={name.fname}
          />
          <input
            onChange={handleChange}
            name="lname"
            placeholder="Last Name"
            value={name.lname}
          />
          <button>Submit</button>
        </form>
      </div>
    );
  }

  export default App;

  ```

---

### Spread Operator

- Use the spread operator to shorten the above code to:

```
setContact(prevValue => {
  return (
    ...prevValue,
    [name]: value // you need to put key in square brackets so it will not be interpreted as a literal string
  )
})

```

#### Using the spread operator

- Concatenation:

```
const citrus = ["Lime", "Lemon", "Orange"];
const fruits = ["Apple", "Banana", "Coconut", ...citrus];

// Note if you move the spread operator, the order will be modified as expected
const fruits = ["Apple", ...citrus, "Banana", "Coconut"];

// Works with objects too
const fullName = {
  fName: "James",
  lName: "Bond"
};

const user = {
  ...fullName,
  id: 1,
  username: "jamesbond007"
}
```

---

### Managing a component tree

- Note that you cannot directly modify props with events inside components
- But you can have state inside of components
- E.g. Strikethrough todo when clicked

```
import React, { useState } from "react";

function ToDoItem(props) {
  const [complete, setComplete] = useState(false);

  function handleClick(event) {
    setComplete((prevValue) => !prevValue);
  }
  return (
    <li
      onClick={handleClick}
      style={{ "text-decoration": complete ? "line-through" : "" }}
    >
      {props.item}
    </li>
  );
}

export default ToDoItem;
```

- How to impact the state of the parent component?
- Send the function that handles the state change as a prop to the item
  - Setting a unique key will help identify which instance of the component (if applicable) called the function
  - Note that React does NOT recommend using the index as the key; instead use something unique like uuid.
    - `npm i uuid` use V4. Link: (https://www.npmjs.com/package/uuid)[https://www.npmjs.com/package/uuid]
- E.g. Delete todo from todos when clicked (truncated for clarity)

```
// In App.js

function deleteItem(id) {
  setItems((prevItems) => {
    return prevItems.filter((item, index) => {
      return item.id !== id
    });
  });
}

...
<ToDoItem key={uuid} text={todoItem} onChecked={deleteItem} id={uuid}>
...

// In ToDoItem.jsx
...

function ToDoItem(props) {
  return (
    <div onClick={() => props.onChecked(props.id)}>
      <li>{props.text}</li>
    </div>
  )
}

```

- Splitting out the input component

```
// In InputArea.jsx

import React, {useState} from "react";

function InputArea(props) {
  const [inputText, setInputText] = useState("");

  function handleChange(event) {
    const newValue = event.target.value;
    setInputText(newValue);
  }

  return (
    <div className="form">
      <input

        <!-- note you must pass the event up if you keep it in here -->
        <!-- onChange={() => props.onChange(event)}  -->
        <!-- but it's better to just move the functions that relate to this component only into the component -->

        onChange={handleChange}
        type="text"
        value={inputText}
      />
      <button onClick={() => {
        props.onAdd(inputText);
        setInputText("");
      }}>
        <span>Add</span>
      </button>
    </div>
  );
}

export default InputArea;


// In App.jsx
import React, { useState } from "react";
import ToDoItem from "./ToDoItem";
import InputArea from "./InputArea";

function App() {
  const [items, setItems] = useState([]);

  function addItem(inputText) {
    setItems((prevItems) => {
      return [...prevItems, inputText];
    });
  }

  function deleteItem(id) {
    setItems((prevItems) => {
      return prevItems.filter((item, index) => {
        return index !== id;
      });
    });
  }

  return (
    <div className="container">
      <div className="heading">
        <h1>To-Do List</h1>
      </div>
      <InputArea
        onAdd={addItem}
        inputText={inputText}
      />
      <div>
        <ul>
          {items.map((todoItem, index) => (
            <ToDoItem
              key={index}
              id={index}
              text={todoItem}
              onChecked={deleteItem}
            />
          ))}
        </ul>
      </div>
    </div>
  );
}

export default App;

```

---

### React Dependencies and Styling

- Using Material UI
  1. Import material-ui/core and material-ui/icons in npm
  2. Import the required icons and styles in the header of each .jsx
  3. Insert like regular components
- (https://www.transparenttextures.com/)[https://www.transparenttextures.com/]
  - A place to find transparent textures for backgrounds
  - Grab CSS and add as background-image: url()

---

#### Note CSS Display vs Visability

- CSS Display − none does not render the element on the document and thus not allocating it any space.

- CSS Visibility − hidden does renders the element on the document and even the space is allocated but it is not made visible to the user. The same is true for collapse.

---
