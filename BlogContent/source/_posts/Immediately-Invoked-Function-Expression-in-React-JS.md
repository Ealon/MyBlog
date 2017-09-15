---
title: Immediately Invoked Function Expression(IIFE) in React JS
tags:
  - javascript
  - react
date: 2017-09-15 14:25:23
---
什么是Immediately-Invoked Function Expression呢？它使一个被立即调用的函数表达式.就像引导你去调用的函数表达式. So, how to use IIFE in React JS, especially in Render()?
<!--more-->
Let's take a look at an example
```javascript
(function() {
  alert('IIFE!');
})()
```
Paste the code into your Chrome Console and execute, it works. It's an IIFE example. And, it equals to
```javascript
(function() {
  alert('IIFE!');
}())
```
or in ES6
```javascript
(() => {
  alert('IIFE!');
})()
```
In React, you need to wrap it in "{ }", of course
```javascript
{
  (function() {
    alert('IIFE!');
  })()
}
```
But if you need to use variables in **this** , you need to bind like this
```javascript
{
  (function() {
    if (this.state.editing) {
      Alert.alert('editing');
    }
  }).bind(this)()
}
```
Or simply use ES6 syntax like below
```javascript
{
  (() => {
    if (this.state.editing) {
      Alert.alert('editing');
    }
  })()
}
```
Cheers ~ 👹