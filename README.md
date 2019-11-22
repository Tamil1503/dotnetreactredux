# dotnetreactredux
To learn dotnet core with react and redux 

 Node JS

Visual Studio Code

NET Core SDK (I’m using SDK version 2.2)

Omni Sharp C# extension


mkdir DotnetReactRedux

cd DotnetReactRedux

dotnet new mvc

Code .

npm init

If you didn’t already install Webpack globally run below:

npm install webpack -g

npm install webpack-cli -g



Then run below to add Webpack to your project



npm i webpack –-save-dev


npm i webpack-cli --save-dev



open package.json file and add below

{ "name": "myapp", "version": "1.0.0", "description": "", "main": "index.js", "scripts": { "build": "webpack", "test": "echo \"Error: no test specified\" && exit 1" }, "author": "", "license": "ISC", "devDependencies": { "webpack": "^4.20.2", "webpack-cli": "^3.1.2" } }







We will write our React app with ES6. ES6 refers to version 6 of the ECMA Script programming language. ECMA Script is the standardized name for JavaScript, and version 6 is the next version after version 5, which was released in 2011. Not all Browsers support es6, so we need a translator for this purpose.
Babel is a JavaScript compiler that includes the ability to compile JSX into regular JavaScript.
Run below commands in CMD to add required Babel libraries to our project



npm install babel-loader @babel/core --save-dev 

npm install @babel/preset-env --save-dev 

npm install @babel/preset-react --save-dev





Create .babelrc configuration file in the root folder of the project with below content.



{


  "presets": ["@babel/preset-env", "@babel/preset-react"]

}



Now, we should set up Webpack to use Babel as loader of JavaScript files.

Create webpack.config.js file in root directory of the project and write below code in it.





const path = require('path');




module.exports = {

entry: "./src/index.js",

mode: "development", // "production" | "development" | "none"

output: {

path: path.resolve(__dirname, "./wwwroot/dist"), // string

// the target directory for all output files

// must be an absolute path (use the Node.js path module)

filename: "bundle.js", // the filename template for entry chunks

publicPath: "dist/", // string

// the url to the output directory resolved relative to the HTML page

},

module: {

rules: [

    {

test: /\.js$/,

exclude: /node_modules/,

use: {

     loader: "babel-loader"

     }

   }

 ]

}

}








const path = require('path');



module.exports = {

entry: "./src/index.js",

mode: "development", // "production" | "development" | "none"

output: {

path: path.resolve(__dirname, "./wwwroot/dist"), // string

// the target directory for all output files

// must be an absolute path (use the Node.js path module)

filename: "bundle.js", // the filename template for entry chunks

publicPath: "dist/", // string

// the url to the output directory resolved relative to the HTML page

},

module: {

rules: [

    {

test: /\.js$/,

exclude: /node_modules/,

use: {

     loader: "babel-loader"

     }

   }

 ]

}

}

Add ReactJs to Project

Run the below command to add React to our project.


npm install react react-dom --save-dev

1

npm install react react-dom --save-dev

Create src folder in the root directory of the project then create src/index.js file.


import * as ReactDOM from 'react-dom';

import * as React from 'react';


ReactDOM.render(

  <h1>Hello, world!</h1>,

  document.getElementById('root')

);



import * as ReactDOM from 'react-dom';

import * as React from 'react';



ReactDOM.render(

  <h1>Hello, world!</h1>,

  document.getElementById('root')

);

Edit Views/Home/Index.cshtml file and remove extra content from it to be like below.


@{

    ViewData["Title"] = "Home Page";

}


<div id="root">Loading...</div>


@section scripts {

  <script src="~/dist/bundle.js"></script>

}


@{

    ViewData["Title"] = "Home Page";

}



<div id="root">Loading...</div>



@section scripts {

  <script src="~/dist/bundle.js"></script>

}

Now, that we added React to our project we don’t need other razor views and controllers came with ASP.NET Core CLI Template. So let get rid of them.


First, lets clear wwwroot folder completely then delete extra views like Privacy.cshtml file from View folder. We only need Index.cshtml file. Then modify layout.cshtml file as below:


<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8" />

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>@ViewData["Title"] - MyApp</title>

    <base href="~/" />


</head>

<body>

    @RenderBody()


    @RenderSection("scripts", required: false)

</body>

</html>


<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8" />

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>@ViewData["Title"] - MyApp</title>

    <base href="~/" />



</head>

<body>

    @RenderBody()



    @RenderSection("scripts", required: false)

</body>

</html>

Rename Models folder to ViewModels. Then open ErroViewModel file and change the namespace of it.

Open ViewImports.cshtml and Change namespace there


@using MyApp.ViewModels


@using MyApp.ViewModels

And also change using of this namespace in HomeController.cs file.


Run Project

Before testing our application first run below command to create our bundle.js file


Webpack --config webpack.config.js


Webpack --config webpack.config.js

Then run


dotnet run


dotnet run

you may see below Privacy error:




DOTNET 2.2 templates by default require HTTPS for all requests and redirect all HTTP requests to HTTPS. When we run newly created project, we will see serving pages as below:


.../myapp>dotnet run

Hosting environment: Development

Content root path: C:\Code\imnapo\naravan

Now listening on: https://localhost:5001

Now listening on: http://localhost:5000

Application started. Press Ctrl+C to shut down.


.../myapp>dotnet run

Hosting environment: Development

Content root path: C:\Code\imnapo\naravan

Now listening on: https://localhost:5001

Now listening on: http://localhost:5000

Application started. Press Ctrl+C to shut down.

As you can see, its listening HTTP over port 5000 and HTTPS over port 5001. If we run the application, we may see an above privacy error.


To solve this untrusted SSL cert error on local machine and to trust the certificate run below command:


dotnet dev-certs https --trust


dotnet dev-certs https --trust

Below message will be shown, press Yes to install the certificate.




You may close your browser and open it again to see trusted badge in your browser. Now you should see “Hello World!” text in your browser.

Now try to change Hello World text and save the file and refresh your browser, but as you can see nothing happened. This happens because we didn’t run our Webpack command to bundle our JavaScript again. There is a solution for this to instantly see changes as you save your file and we will explain it in the next topic.


Adding Hot Module Replacement

What we need are Webpack middleware and Hot Module replacement:


Webpack middleware so that, during development, any webpack-built resources will be generated on demand, without you having to run webpack manually or compile files to disk


Hot module replacement so that, during development, your code and markup changes will be pushed to your browser and updated in the running application automatically, without even needing to reload the page.


Adding Webpack middleware

Open Startup.cs file and add this using:


using Microsoft.AspNetCore.SpaServices.Webpack;


using Microsoft.AspNetCore.SpaServices.Webpack;

Go to configure method and add Webpack middleware to it as below:


if (env.IsDevelopment())

{

    app.UseDeveloperExceptionPage();

    app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {

    HotModuleReplacement = true,

    ReactHotModuleReplacement = true

    });

}

else

{

    app.UseExceptionHandler("/Home/Error");

}


if (env.IsDevelopment())

{

    app.UseDeveloperExceptionPage();

    app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {

    HotModuleReplacement = true,

    ReactHotModuleReplacement = true

    });

}

else

{

    app.UseExceptionHandler("/Home/Error");

}

Open _ViewImports.cshtml file and add below to it:


@addTagHelper *, Microsoft.AspNetCore.SpaServices


@addTagHelper *, Microsoft.AspNetCore.SpaServices

there are other packages to install too:


npm install react-hot-loader

npm install --save-dev webpack-hot-middleware

npm install --save-dev webpack-dev-middleware


npm install react-hot-loader

npm install --save-dev webpack-hot-middleware

npm install --save-dev webpack-dev-middleware

Adding hot module replacement:

We need two packages to enable hot module replacement in ASP.NET, so run below command:


npm install -D aspnet-webpack aspnet-webpack-react


npm install -D aspnet-webpack aspnet-webpack-react

our webpack configuration file needs some modifications:


Change entry point from string of “./src/index.js” to an object with a main property like { main: "./src/index.js"}.

Add react-hot-module/babel plugin to babel-loader

So open webpack.config.js file and change it as below:


  const path = require('path');


  module.exports = {

  entry: { main: "./src/index.js"},

  mode: "development", // "production" | "development" | "none"

  output: {

    path: path.resolve(__dirname, "./wwwroot/dist"), // string

    // the target directory for all output files

    // must be an absolute path (use the Node.js path module)

    filename: "bundle.js", // the filename template for entry chunks

    publicPath: "dist/", // string    

  // the url to the output directory resolved relative to the HTML page

  },

  module: {

     rules: [

       {

         test: /\.js$/,

         exclude: /node_modules/,

         use: {

          loader: "babel-loader",

          options: {

            plugins: [

              "react-hot-loader/babel"

            ]

          }

        }

      }

    ]

  }

 }


  const path = require('path');



  module.exports = {

  entry: { main: "./src/index.js"},

  mode: "development", // "production" | "development" | "none"

  output: {

    path: path.resolve(__dirname, "./wwwroot/dist"), // string

    // the target directory for all output files

    // must be an absolute path (use the Node.js path module)

    filename: "bundle.js", // the filename template for entry chunks

    publicPath: "dist/", // string    

  // the url to the output directory resolved relative to the HTML page

  },

  module: {

     rules: [

       {

         test: /\.js$/,

         exclude: /node_modules/,

         use: {

          loader: "babel-loader",

          options: {

            plugins: [

              "react-hot-loader/babel"

            ]

          }

        }

      }

    ]

  }

 }

Add the end open index.js and add this to end of the file.


// Allow Hot Module Replacement

if (module.hot) {

  module.hot.accept();

}


// Allow Hot Module Replacement

if (module.hot) {

  module.hot.accept();

}

Now, run the project and try to change the “hello world!” Text one more time and save the file. You should instantly see changes in your browser (you may run  webpack –config webpack.config.js command ones more before run the app).


Create Required React Components and navigate between them

Now, that we initialized our ASP.NET Application, let create our React app to register and login users. To simplify, we are going to create 4 components. We are not going to define the real implementation of them. We will just return simple text in render method of each component.


App.js : our main component (Home Page)

Signin.js : our login page

Signup.js: our register page

Feature.js: our special component that should be shown only when a user is logged in.

Add body of Required Components

We are going to create simple components first and not their real implementation. let us create “components” folder under “src” folder:


…\src> mkdir components


…\src> mkdir components

Then let’s create our first component named app.js:


…\src>cd components


…\src\components> echo.>App.js


…\src>cd components



…\src\components> echo.>App.js

Modify app.js file as below:


import React, { Component} from "react";


class App extends Component {

    constructor(props) {

         super(props);

    }


    render() {

       return (<div>This is App.js</div>);

    } 

} 

export default App;


import React, { Component} from "react";



class App extends Component {

    constructor(props) {

         super(props);

    }



    render() {

       return (<div>This is App.js</div>);

    } 

} 

export default App;

Then let’s create the Feature component:


…\src\components> echo.> Feature.js


…\src\components> echo.> Feature.js

Modify the Feature.js file as below:


import React, { Component} from "react";


class Feature extends Component {

  

  constructor(props) {

    super(props);

    

  }

  


  render()

  {

    return (

      <div>This is Feature.js. Only authenticated Users allowed.</div>

    );

  }

}


export default Feature;


import React, { Component} from "react";



class Feature extends Component {

  

  constructor(props) {

    super(props);

    

  }

  



  render()

  {

    return (

      <div>This is Feature.js. Only authenticated Users allowed.</div>

    );

  }

}



export default Feature;

For the sake of having a better structure, we will create a sub-folder for related components.

Create a new folder named auth, under the components folder and add Signup.js and Signin.js files to it.


…\src\components> mkdir auth

…\src\components\auth> echo.> Signin.js

…\src\components\auth> echo.> Signin.js


…\src\components> mkdir auth

…\src\components\auth> echo.> Signin.js

…\src\components\auth> echo.> Signin.js

Create the sign-in component as below:


import React, { Component } from 'react';

class Signin extends Component {

    constructor(props) {

        super(props);

    }


    render() {

        return("This is signin page!") 

    } 

}



export default Signin;


import React, { Component } from 'react';

class Signin extends Component {

    constructor(props) {

        super(props);

    }



    render() {

        return("This is signin page!") 

    } 

}



export default Signin;

Next, let’s create our signup component:


import React, { Component } from 'react';

class Signup extends Component {

    constructor(props) {

        super(props);

    }


    render() {

        return("This is signup page!") 

    } 

} 


export default Signup;


import React, { Component } from 'react';

class Signup extends Component {

    constructor(props) {

        super(props);

    }



    render() {

        return("This is signup page!") 

    } 

} 



export default Signup;

Add Navigation Between Components

Next step is to create navigation between these 4 components.


Add react router to project.


npm install react-router-dom

1

npm install react-router-dom

modify the index.js file to add the ability to navigate between components.


import * as ReactDOM from 'react-dom';

import * as React from 'react';

import { BrowserRouter as Router, Route, Link,Switch } from "react-router-dom";

import App from './components/App';

import Signup from './components/auth/Signup';

import Signin from './components/auth/Signin';



ReactDOM.render(

    <Router>

      <div>

          <ul>

          <li>

            <Link to="/">Home</Link>

          </li>

          <li>

            <Link to="/Signin">Signin</Link>

          </li>

          <li>

            <Link to="/Signup">Signup</Link>

          </li>

        </ul>


        <hr />

        <Route exact path="/" component={App} />  

        <Route path="/Signin" component={Signin} />                                             

        <Route path="/Signup" component={Signup} />   

          

      </div>              

  </Router>

  ,

  document.getElementById('root')

);


// Allow Hot Module Replacement

if (module.hot) {

  module.hot.accept();

}


import * as ReactDOM from 'react-dom';

import * as React from 'react';

import { BrowserRouter as Router, Route, Link,Switch } from "react-router-dom";

import App from './components/App';

import Signup from './components/auth/Signup';

import Signin from './components/auth/Signin';





ReactDOM.render(

    <Router>

      <div>

          <ul>

          <li>

            <Link to="/">Home</Link>

          </li>

          <li>

            <Link to="/Signin">Signin</Link>

          </li>

          <li>

            <Link to="/Signup">Signup</Link>

          </li>

        </ul>



        <hr />

        <Route exact path="/" component={App} />  

        <Route path="/Signin" component={Signin} />                                             

        <Route path="/Signup" component={Signup} />   

          

      </div>              

  </Router>

  ,

  document.getElementById('root')

);



// Allow Hot Module Replacement

if (module.hot) {

  module.hot.accept();

}

Run the application and if everything is set as I said you should be able to navigate between these components using provided links inside home page. Now, try to type desired page (Ex. http://localhost:5000/Signin) on the browsers address bar. As you can see, it will not load the page. So why we access our pages from links inside pages but we can not access them when typing directly on browsers address bar?


Server-side vs Client-side Routing

The first big thing to understand about this is that there are now 2 places where the URL is interpreted. The very first request will always be to the server. That will then return a page that contains the needed script tags to load React and React Router etc. Only when those scripts have loaded does phase 2 start.


So in our case when we type directly on browsers address bar no react-router is running yet. So, it will make a server request and that is where our problem starts.


To fix this we should add single page application fallback rout to our ASP.NET application routes. Open startup.cs file and modify below section in Configure method:


       app.UseMvc(routes =>

            {

                routes.MapRoute(

                    name: "default",

                    template: "{controller=Home}/{action=Index}/{id?}");


                    routes.MapSpaFallbackRoute(

                    name: "spa-fallback",

                    defaults: new { controller = "Home", action = "Index" });

            });


       app.UseMvc(routes =>

            {

                routes.MapRoute(

                    name: "default",

                    template: "{controller=Home}/{action=Index}/{id?}");



                    routes.MapSpaFallbackRoute(

                    name: "spa-fallback",

                    defaults: new { controller = "Home", action = "Index" });

            });

Run the application once again and now if we type our desired page like http://localhost:5000/Signin on browser’s address bar, we will navigate to the sign in page.
