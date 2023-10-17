# Routing in a React Application

Routing allows our single-page-application to change in such a way that it looks like a new page had loaded when the user clicks a link or button.

Routing is implemented with a library call `react-router`. React router comes in a few flavors but we will be focusing on `react-router-dom`. This library will give us everything we need to implement Routing into a React web application.

[Official Docs](https://reactrouter.com/en/main)

## Install react-router-dom

In order to use `react-router-dom`, it has to be installed as a dependency. From the root directory of your demo application run the following command.

```bash
npm i react-router-dom
```

This will install `react-router-dom` permanent production dependency for our application.

## Import BrowserRouter

In order to use the router, we will need to import some components from the library into our application.

The first file we will need to update is src/index.js. We will need to include the `BrowserRouter` component from `react-dom-router`. We will wrap our **App** in the `<BrowserRouter>` router component. This will allow us to use routing features in the **App** component and its children.

src/index.js

```javascript
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import * as serviceWorker from "./serviceWorker";

//import browser router so we can use it in our application
import { BrowserRouter } from "react-router-dom";

//wrap our default App in the BrowserRouter we just imported
ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
);

serviceWorker.unregister();
```

Now we need to update App.js with our routes. To declare routes we need to import the **Route** component from react-dom-router.

[Route Documentation](https://reacttraining.com/react-router/web/api/Route)

src/App.js

```javascript
import React, { Component } from "react";
import Welcome from "./components/welcome/welcome";
import Clock from "./components/clock/clock";
import Contact from "./components/contact/contact";

//Import the Route component
import { Route } from "react-router-dom";

class App extends Component {
  render() {
    return (
      <div>
        {/* define our routes */}
        <Route
          exact
          path="/"
          render={(props) => <Welcome {...props} name="eric" />}
        />
        <Route path="/clock" component={Clock} />
        <Route path="/contact" component={Contact} />
      </div>
    );
  }
}

export default App;
```

At this point, you should be able to run our application and see the Welcome Component. Try accessing the other routes by updating the URL in your browser. This is nice but not ideal, we are forcing a refresh by manually accessing the URL from the browser. We want our React application to transition between routes when a user clicks a link or a button without refreshing the page.

## Create a Navigation Component

Let's create a simple navigation component our users can interact with to access our routes. We will need to import **Link** from react router. We can't use the standard `<a>` tag for a link because it makes a request and causes the browser to reload. **Link** helps us navigate to our routes without reloading the browser.

src/components/navigation/Navigation.js

```javascript
import React from "react";

//import Link from react-dom-router
import { Link } from "react-router-dom";

function Navigation(props) {
  return (
    <ul>
      <li>
        <Link to="/">Home</Link>
      </li>
      <li>
        <Link to="/clock">Clock</Link>
      </li>
      <li>
        <Link to="/contact">Contact</Link>
      </li>
    </ul>
  );
}

export default Navigation;
```

Now import and display the navigation component in the App component

src/App.js

```javascript
import React, { Component } from "react";
import Welcome from "./components/welcome/welcome";
import Clock from "./components/clock/clock";
import Contact from "./components/contact/contact";

//Import the Navigation component
import Navigation from "./components/navigation/navigation";

import { Route } from "react-router-dom";

class App extends Component {
  render() {
    return (
      <div>
        {/* render the Navigation component */}
        <Navigation />
        <Route
          exact
          path="/"
          render={(props) => <Welcome {...props} name="eric" />}
        />
        <Route path="/clock" component={Clock} />
        <Route path="/contact" component={Contact} />
      </div>
    );
  }
}

export default App;
```

You should now see your Navigation component and be able to click the links to display the related component
