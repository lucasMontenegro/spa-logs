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

### react-ui

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
