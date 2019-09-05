# Setup a Simple React Environment
*Note: If you want to skip the tutorial and just set up a basic app, skip down to [Quick Start](#quick-start).*

This tutorial is adapted from https://appdividend.com/2017/03/29/beginners-guide-to-setup-react-v15-4-2-environment/ and https://www.valentinog.com/blog/webpack/.

**Prerequisites**: Node.js must be installed.

## Step 1: Create a project tree.

Create a project directory with a `src` sub directory.

```bash
mkdir -p hello-react/src
```

## Step 2: Intialize node.js.

Initialize npm in the project directory.

```bash
cd hello-react
npm init -y
```

## Step 3: Install and configure webpack.

Install webpack and the webpack dev server.

```bash
npm install --save-dev webpack webpack-cli webpack-dev-server
```

Install the html plugin to allow webpack to process html files.

```bash
npm install --save-dev html-webpack-plugin html-loader
```

Create a webpack.config.js file to enable the html plugin.

```javascript
// webpack.config.js 

const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    })
  ]
};
```

## Step 4: Create a basic app.
Create a new file in the `src` directory called `index.html`.  The content of `index.html` is shown below.

```html
<!-- index.html -->

<!DOCTYPE html>
<html lang="en"> 
<head>
    <meta charset="UTF-8">
    <title>Hello React</title>
</head>
<body>
    <div id="app"></div>
</body>
</html>
```

Create another file in the `src` directory called `index.js`. For now we will just add a single log statement as shown below.

```javascript
// index.js

console.log('Hello from index.js!');
```

## Step 5: Add build scripts.
Add the following scripts to package.json:

```json
"dev_build": "webpack --mode development ./src/index.js",
"dev_server": "webpack-dev-server --mode=development --devtool=inline-source-map --history-api-fallback ./src/index.js --open"
 ```
Your package.json file should look like the following:
```json
{
  "name": "hello-react",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev_build": "webpack --mode development ./src/index.js",
    "dev_server": "webpack-dev-server --mode=development --devtool=inline-source-map --history-api-fallback ./src/index.js --open",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "html-loader": "^0.5.5",
    "html-webpack-plugin": "^3.2.0",
    "webpack": "^4.36.1",
    "webpack-cli": "^3.3.6",
    "webpack-dev-server": "^3.7.2"
  }
}
```

Buiild the project.
```bash
npm run dev_build
```
You should see something like the following:

```bash

> hello-react@1.0.0 dev_build /home/matr00949/tmp/hello-react
> webpack --mode development ./src/index.js

Hash: 02a9d37890d8f135d166
Version: webpack 4.36.1
Time: 478ms
Built at: 07/17/2019 2:32:46 PM
       Asset       Size  Chunks             Chunk Names
./index.html  191 bytes          [emitted]  
     main.js   3.82 KiB    main  [emitted]  main
Entrypoint main = main.js
[./src/index.js] 51 bytes {main} [built]
Child html-webpack-plugin for "index.html":
     1 asset
    Entrypoint undefined = ./index.html
    [./node_modules/html-webpack-plugin/lib/loader.js!./src/index.html] 157 bytes {0} [built]
```

You should now have a `dist` directory that contains an html file and a javascript file.  This is the output of webpack and it is ready to be served froma web server.

Try running the develpoment server.

```bash
npm run dev_server
```

A browser window should open to a blank page.  Open the console window and you should see the log message from our index.js file.

Close the server by pressing **`CTRL-C`**.

Next, we need to configure Babel in our webpack environment.

## Step 6: Install React.

Our environment is now configured and we are ready to install **React**.

```bash
npm install react --save react react-dom
```

## Step 7: Install and configure Babel.

Install `babel` and the `babel-loader` for webpack.

```bash
npm install --save-dev @babel/core babel-loader
```

We need to configure webpack to use Babel.  To do that we need to add a rule to the `webpack.config.js` file.

```javascript
{
  test: /\.js$/,
  exclude: /node_modules/,
  use: [
    {
      loader: "babel-loader"
    }
  ]
}
```

After adding that rule, the webpack.config.js file should look like the following:

```javascript
// webpack.config.js 
  
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          },
        ]
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: [
          {
            loader: "babel-loader"
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    })
  ]
};
```

Install the babel presets for webpack.

```bash
npm install --save-dev @babel/preset-react @babel/preset-env
```

Configure bable to use the presets by creating a `.babelrc` file:

```
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

## Step 8: Create a React app.

Create a file in the `src` directory called `App.js`:

```javascript
import React from "react";
import ReactDOM from "react-dom";
const App = () => {
  return (
    <div>
      <p>Hello from React!</p>
    </div>
  );
};
export default App;
ReactDOM.render(<App />, document.getElementById("app"));
```

Add in `import` statement to the `index.js` file to import the app.

```javascript
import App from "./App";
```

`index.js` should now look like this:

```javascript
// index.js

import App from "./App";

console.log('Hello from index.js!');
```

Now run the dev server and the app should work.

```bash
npm run dev_server
```

A browser window should open and you should see the message from the app.

## Quick Start

If you just want to get started quickly, run the following commands:

```bash
mkdir -p hello-react/src
cd hello-react
npm init -y
npm install --save-dev \
  webpack webpack-cli webpack-dev-server \
  html-webpack-plugin html-loader \
  @babel/core babel-loader \
  @babel/preset-react @babel/preset-env
npm install react --save react react-dom
```

Add the scripts to package.json:

```json
"dev_build": "webpack --mode development ./src/index.js",
"dev_server": "webpack-dev-server --mode=development --devtool=inline-source-map --history-api-fallback ./src/index.js --open"
 ```

Then copy `App.js`, `index.html`, `index.js`, .`babelrc`, and `webpack.config.js` from this project.