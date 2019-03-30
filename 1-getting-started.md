---
layout: default
title: Create React App, Director's Cut
nav_order: 2
description: "Setting up React, Heroku, etc."
permalink: /getting-started
---

## Create React App, Director's Cut

My application will have two separate GitHub repositories:
1. react-ui: front-end assets and development toolset
2. server: the HTTP server designed to be hosted on Heroku.

### "react-ui" Package

Initialize a git repo through the GitHub interface and then clone it locally:

```bash
git clone https://github.com/<username or organization name>/react-ui.git
```

Start a new React app:

```bash
npm init react-app ./react-ui
```

Add the following to `react-ui/package.json`:

```diff
 {
-  "name": "react-ui",
-  "version": "0.1.0",
+  "name": "<username or organization name>-react-ui",
+  "version": "0.0.1",
+  "description": "<package description>",
   "private": true,
+  "main": "index.js",
   "dependencies": {
     "react": "^16.8.6",
     "react-dom": "^16.8.6",
     "react-scripts": "2.1.8"
   },
   "scripts": {
+    "install": "react-scripts build",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
     "eject": "react-scripts eject"
   },
+  "repository": {
+    "type": "git",
+    "url": "git+https://github.com/<username or organization name>/react-ui.git"
+  },
+  "author": "<your name>",
+  "license": "MIT",
+  "bugs": {
+    "url": "https://github.com/<username or organization name>/react-ui/issues"
+  },
+  "homepage": "https://github.com/<username or organization name>/react-ui#readme",
   "eslintConfig": {
     "extends": "react-app"
   },
```

The important bits:

- `"install": "react-scripts build"` will build the app for production after the package is installed
- `"main": "index.js"` will set `react-ui/index.js` as the entry point when this package is installed and required from another npm package.

Next create `react-ui/index.js` and, from there, export the path to the `build` folder:

```javascript
module.exports = require('path').join(__dirname, 'build');
```

Add a `react-ui/.env` file with the following:
```
PUBLIC_URL=/
```
This will be the mount point for the assets.

Finally, push the changes to GitHub.

### "server" Package

Similarly, create a GitHub repo and clone it locally. Then initialize a new npm package inside the repo folder:
```bash
npm init
```

Edit `server/package.json`:
```diff
   "version": "0.0.1",
   "description": "<package description>",
-  "main": "index.js",
+  "private": true,
+  "engines": {
+    "node": "11.3.0"
+  },
   "scripts": {
+    "start": "node index.js",
     "test": "echo \"Error: no test specified\" && exit 1"
   },
```

- `"engines"."node": "11.3.0"` will make sure that Heroku and your local setup use the same NodeJS version
- Heroku will automatically run `node index.js` to start the app.

Create a `server/app.json`:
```json
{
  "name": "<app name>",
  "description": "web app made of create-react-app UI + Node API",
  "scripts": {
  },
  "env": {
  },
  "formation": {
    "web": {
      "quantity": 1
    }
  },
  "addons": [

  ],
  "buildpacks": [
    {
      "url": "heroku/nodejs"
    }
  ]
}
```
This is a configuration file that tells Heroku how to deploy the application.

- `"formation"."web"."quantity": 1` indicates the amount of processes to run.

In `server/index.js`, create an HTTP server similar to [this one](https://github.com/mars/heroku-cra-node/blob/master/server/index.js). Require the react-ui package to get the assets folder path and mount it in the root route.

Add a `server/.gitignore` file:

```
/node_modules
```

Install the dependencies needed. To install a package directly from GitHub do this:
```
npm install https://github.com/<username or organization name>/<repository name>.git
```

Push the changes to GitHub.

### Heroku

1. Create a new app from the Heroku dashboard.
2. Under "Deployment method" select "Connect to GitHub".
3. Then enter the server repository name and click "Enable Automatic Deploys".
4. Finally, under "Manual deploy" select a branch and click "Deploy branch".

### Development Server

To be able to do API calls during development, add the following to `react-ui/package.json`:

```diff
     "not dead",
     "not ie <= 11",
     "not op_mini all"
-  ]
+  ],
+  "proxy": "https://<app name>.herokuapp.com/"
 }
```

## Further Reading

- [Node.js on Heroku: A More Complete Tutorial: Part 1](https://codeburst.io/node-js-on-heroku-a-more-complete-tutorial-part-1-9e80cb071498)

- [Getting Started - Create React App](https://facebook.github.io/create-react-app/docs/getting-started)
- [Folder Structure - Create React App](https://facebook.github.io/create-react-app/docs/folder-structure)
- [Available Scripts - Create React App](https://facebook.github.io/create-react-app/docs/available-scripts)
- [Setting Up Your Editor - Create React App](https://facebook.github.io/create-react-app/docs/setting-up-your-editor#syntax-highlighting)

- [npm-package.json](https://docs.npmjs.com/files/package.json)
- [Adding Custom Environment Variables - Create React App](https://facebook.github.io/create-react-app/docs/adding-custom-environment-variables)
- [Webpack Output Management](https://webpack.js.org/guides/output-management/)
- [HTML Webpack Plugin](https://github.com/jantimon/html-webpack-plugin)
- `react-ui/public/index.html`

- [Introducing the app.json Application Manifest](https://blog.heroku.com/introducing_the_app_json_application_manifest)
- [create-react-app with a Node server on Heroku](https://github.com/mars/heroku-cra-node)
- [Deployment - Create React App](https://facebook.github.io/create-react-app/docs/deployment)

- [Node.js on Heroku: A More Complete Tutorial: Part 2](https://codeburst.io/node-js-on-heroku-a-more-complete-tutorial-part-2-53350381543c)

- [Proxying API Requests in Development - Create React App](https://facebook.github.io/create-react-app/docs/proxying-api-requests-in-development)
