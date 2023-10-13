# Create a Simple Application using create-react-app
In this guided exercise, we will build a simple application with 4 components. We will have the default App component provided by create-react-app as well as a Welcome, Clock, and Contact component. The Welcome component will be stateless and deal with a single prop. The Clock component will be stateful and leverage lifecycle hooks. The Contact component will work with user input.

## Scaffold project with create-react-app
First, we will need to scaffold our project using create-react

* Open your terminal to a directory you want to create your project in and run the following command:

```bash
  $ npx create-react-app react-demo-app
```

* Then open the react-demo-app project folder created in VSCode or your editor of choice

## Create The Stateless Welcome Component
The following example demonstrates how to create a simple stateless component that relies on 1 "prop" and then how to include that component in our application.

> Always start component names with a capital letter.
For example, `<div />` represents a DOM tag, but `<Welcome />` represents a component in JSX.

1. Within the *src* directory, create a new directory called *components*
2. Within *src/components*, create a directory called *welcome*.
3. Within *src/components/welcome*, create *welcome.js*

Your project structure should have been altered as follows. Note that *src* now contains a sub-directory with component files:

```
react-clock
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   └── favicon.ico
│   └── index.html
│   └── manifest.json
└── src
	└── components
    │   └── welcome
    │       └── welcome.js
    └── App.css
    └── App.js
    └── App.test.js
    └── index.css
    └── index.js
    └── logo.svg
    └── registerServiceWorker.js
```

### welcome.js

```javascript
/*** Copy this code block to Welcome.js ***/

import React from 'react';

function Welcome(props) {

    return (
        <div>
            <h1>Hello, {props.name}!</h1>
        </div>
    );
}

export default Welcome;
```

- The first line of code imports React so we can use JSX. Read more about [imports](https://stackoverflow.com/questions/36795819/when-should-i-use-curly-braces-for-es6-import/36796281#36796281)
- To create the `Welcome` component, we create a simple JavaScript function that takes "props" as its argument. Whenever a component is rendered, the application will render the returned JSX expression.
- Notice the line that includes **{props.name}**? That will get replaced with the "name" prop passed into the component.

## Including the Welcome Component in the main component, App.js
We want to use our welcome component to greet our visitors. Let's include our component in the App.js where we would like it to display

1. Import the Welcome component at the top of `App.js`
  ```javascript
  import Welcome from './components/welcome/welcome';
  ```

2. Add the Welcome component where we want it to show and set the name property. The Welcome component consumes the name property and then output using **{this.props.name}**. The name attribute becomes the value of this.props.name inside our component.
  ```javascript
  <Welcome name="Eric" />
  ```

3. Review App.js to make sure that you've imported the Welcome component and that the Component has been added to the render function.

  Your code should look similar to the following
  ```javascript
  import React, { Component } from 'react';
  import logo from './logo.svg';
  import './App.css';

  //*** Import the Welcome component ***//
  import Welcome from './components/welcome/welcome';

  class App extends Component {
    render() {
      return (
        <div className="App">
          <div className="App-header">
            <img src={logo} className="App-logo" alt="logo" />

            {/* Adding the Welcome component and passing the value "Eric" to the "name" prop */}
            <Welcome name="Eric" />

            <h2>Welcome to React</h2>
          </div>
          <p className="App-intro">
            To get started, edit <code>src/App.js</code> and save to reload.
          </p>
        </div>
      );
    }
  }
  export default App;
  ```

## Create The Clock Component
The following example will show how to make a stateful component that leverages state and lifecycle hooks. In order to have a stateful component the component has to be created as a class and the class must extend the base React.Component class. The class must also include a render method.

State is similar to props, but it is private and fully controlled by the component. To demonstrate this behavior we will steal an example from the React documentation and add a clock to our website. To begin, we'll add the files necessary for a Clock component.

1. Within *src/components*, create a directory called *clock*.
2. Within *src/components/clock*, create *clock.js*.

Your project structure should now be as follows:
```
react-demo-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   └── favicon.ico
│   └── index.html
│   └── manifest.json
└── src
	└── components
    │   └── welcome
    │       └── welcome.js
    |   └── clock
    │       └── clock.js
    └── App.css
    └── App.js
    └── App.test.js
    └── index.css
    └── index.js
    └── logo.svg
    └── registerServiceWorker.js
```

**Add the following code to clock.js**

```javascript
import React, { Component } from 'react';

class Clock extends Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
  tick() {
    this.setState({
      date: new Date()
    });
  }
  render() {
    return (
      <div>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
export default Clock;
```

Now, let's walk through our new Clock component and discuss all of the class methods we've added.

- `constructor()` - By overriding the constructor, we are now able to set the initial state for our component. We are calling `super(props)` to call the parent class constructor and maintain the original behavior of all components. We then set some initial properties to `this.state`. `this.state` is accessible throughout our component, including the render function and represents the local state of the component.

      **NOTE: Class components should always call the base constructor with props.**

- `componentDidMount()` and `componentWillUnmount()` -  These two methods are part of the lifecycle of a React Component. The `componentDidMount()` hook runs after the component output has been rendered to the DOM. This makes it a perfect spot to kick off the timer. If the Clock component is ever removed from the DOM, React calls the `componentWillUnmount()` lifecycle hook, so the timer is stopped.

- `tick()` - This method updates the date stored in `this.state` and is run by `setInterval()` to get a new date every second. Notice that the state isn't set directly. Instead, `tick()` uses `this.setState()` to update the `date` property of the state object. You should always (with the odd rare exception) use `this.setState()` to update the component state. 

### Include the Clock component in App.js

Add `<Clock />` to include our stateful clock component. App.js should now look similar to the following code sample:

```javascript

 //*** Import the Welcome component ***//
import Clock from './components/clock/clock';

class App extends Component {
  render() {
    return (
      <div className="App">
        <div className="App-header">
          <img src={logo} className="App-logo" alt="logo" />

          <Welcome name="Eric" />

          {/* Added the clock component here.Clock doesn't require any props. It manages everything with it's internal state */}
          <Clock />

          <h2>Welcome to React</h2>
        </div>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}
export default App;
```

## Create The Contact Component
The following example will show how to make a stateful component that deals with user input. We will set up the form using "controlled components". Form elements such as `<input>`, `<textarea>`, and `<select>` maintain their own state and update it based on user input. An example of this is that they can remember what is entered by the user.

We would want React state to be the “single source of truth” instead of having to grab what was entered directly from the form. An input form element whose value is controlled by React in this way is called a “controlled component”.

Using controlled components is optional. Read the following for more information:
[Uncontrolled Components](https://reactjs.org/docs/uncontrolled-components.html )
[When To Use Refs](https://reactjs.org/docs/refs-and-the-dom.html)

1. Within *src/components*, create a directory called *contact*.
2. Within *src/components/contact*, create *contact.js*.

Your project structure should now be as follows:
```
react-demo-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   └── favicon.ico
│   └── index.html
│   └── manifest.json
└── src
	└── components
    │   └── welcome
    │       └── welcome.js
    |   └── clock
    │       └── clock.js
    |   └── contact
    │       └── contact.js
    └── App.css
    └── App.js
    └── App.test.js
    └── index.css
    └── index.js
    └── logo.svg
    └── registerServiceWorker.js
```

**Add the following code to contact.js**

```javascript
import React, { Component } from 'react';

class Contact extends Component {
  
    constructor(props) {
        super(props);
        this.state = { 
            submitted: false, 
            formData: {
                firstName: "",
                lastName: "",
                email: ""
            }
        }
    }

    handleChange = (event) => {
        let formData = this.state.formData;
        formData[event.target.name] = event.target.value;
        this.setState({formData});
    }

    handleSubmit = (event) => {
        event.preventDefault();
        this.setState({
            submitted: true
        })
    }

    resetForm = (event) => {
        this.setState({
            submitted: false,
            formData: {
                firstName: "",
                lastName: "",
                email: ""
            }
        })
    }

    render() {

        //show the thank you message if the form has been submitted
        if(this.state.submitted){
            return (
                <div>
                    Thank you, {this.state.formData.firstName}, for submitting the form <br/>
                    <button onClick={this.resetForm}>Reset Form</button>
                </div>
            )
        }
        return (
            <div>
                <form onSubmit={this.handleSubmit}>
                    <div>
                        <label>First Name:</label>
                        <input onChange={this.handleChange} type="text" name="firstName" value={this.state.formData.firstName} />
                    </div>
                    <div>
                        <label>Last Name:</label>
                        <input onChange={this.handleChange} type="text" name="lastName" value={this.state.formData.lastName} />
                    </div>
                    <button>Submit Form</button> <br/>
                    {this.state.formData.firstName}
                    <br/>
                    {this.state.formData.lastName}
                </form>
            </div>
        );
    }
}
export default Contact;
```

Now, let's walk through our new Contact component and discuss what is happening.

- **The initial state** -  here we are setting submitted to false which will be used for conditional rendering of the thank you page. we also set up the formData object that will be the source of truth for the data in the form.
  ```javascript
    this.state = { 
      submitted: false, 
      formData: {
          firstName: "",
          lastName: "",
          email: ""
      }
    }
  ```

- `handleChange` - This method is responsible for keeping the form in sync with the formData object in state. It is called when the **onChange** event is triggered on the input fields.
```javascript

  handleChange = (event) => {
      //since this.setState only does a shallow merge we are going to create a new object from the related object in state
      //this will allow us to make sure we don't lose data when calling setState method
      let newFormData = this.state.formData;

      //update the specific field in our new object
      newFormData[event.target.name] = event.target.value;

      //update the formData object in state with the new 
      this.setState({formData: newFormData});
  }

```

- `handleSubmit` - This method is responsible for what happens when the form is submitted. It is run when the **onSubmit** event is triggered on the form. We use the `preventDefault()` method to prevent the form from refreshing the page when submitted.
```javascript

   handleSubmit = (event) => {
        //prevent the form submission from reloading the page
        event.preventDefault();
        //update state to reflect the form submission
        //we leverage this in the render method to show the thank you page instead of the form
        this.setState({
            submitted: true
        })
    }

```

- `resetForm` - This method clears our form by resetting the values in state. It is run when the **onClick** event is triggered on the reset button
```javascript

   resetForm = (event) => {
      this.setState({
          submitted: false,
          formData: {
              firstName: "",
              lastName: "",
              email: ""
          }
      }
   }

```

### Include the Contact component in App.js

Add `<Contact />` to include our stateful Contact component. App.js should now look similar to the following code sample:

```javascript

 //*** Import the Welcome component ***//
import Contact from './components/clock/clock';

class App extends Component {
  render() {
    return (
      <div className="App">
        <div className="App-header">
          <img src={logo} className="App-logo" alt="logo" />

          <Welcome name="Eric" />

          <Clock />

           {/* Added the Contact component here. */}
          <Contact />

          <h2>Welcome to React</h2>
        </div>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}
export default App;
```