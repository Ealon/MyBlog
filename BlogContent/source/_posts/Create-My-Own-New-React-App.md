---
title: Create My Own New React App
tags:
  - react
  - javascript
  - eslint
  - redux
categories:
  - tech
description: description about this page
date: 2017-10-26 16:15:41
---

## Create My Own New React App
---

### 1. Create the baisc project skeleton
Read the docs at https://github.com/facebookincubator/create-react-app

run 
```
npm install -g create-react-app
create-react-app project-name
cd project-name/
npm start
```

Then the application is running at http://localhost:3000/

### 2. Add eslint
Read https://www.npmjs.com/package/eslint-config-airbnb
run 
```
npm info "eslint-config-airbnb@latest" peerDependencies
```
to check the peer dependencies that are needed

run 
```
npm install --save-dev eslint-config-airbnb
```
to install eslint configuration of airbnb coding style, then `--save-dev` the other peer dependencies as well, they should be ***eslint, eslint-plugin-import, eslint-plugin-jsx-a11y, eslint-plugin-react***

At last, add 
```js
{
  "extends": "airbnb"
}
```
to the `.eslintrc` file