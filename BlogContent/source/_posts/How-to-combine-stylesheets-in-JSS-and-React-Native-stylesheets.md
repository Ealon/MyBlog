---
title: How to combine stylesheets in JSS and React Native stylesheets
tags:
  - jss
  - css
  - react-native
  - react
  - javascript
date: 2017-09-20 19:33:43
---
> JSS is a more powerful abstraction over CSS. It uses JavaScript as a language to describe styles in a declarative and maintainable way. It is a high performance JS to CSS compiler which works at runtime and server-side.

[See on Github](https://github.com/cssinjs/jss)

---

If you are using jss in ***React***, the trick is to use `classnames`. 

Step1: 
```javascript
import { withStyles, createStyleSheet } from 'material-ui/styles';
import classNames from 'classnames';
```
Step2: compose your stylesheets
```js
const styles = {
  ...

  textWidth:{
    width: '40%',
    minWidth: '200px',
  },
  ...
  table: {
    margin: '0 auto',
    textAlign: 'left',
  }
  ...
};
```
step3: 
```js
<table className={classNames(this.props.classes.table, this.props.classes.textWidth)}>
```

Step4: 
```js
export default withStyles(createStyleSheet(styles))(ComponentName);
```
---
If you are using ***StyleSheet*** in ***React Native***, see docs [here](https://facebook.github.io/react-native/docs/stylesheet.html)

Step1: Create a new StyleSheet:
```js
var styles = StyleSheet.create({
  container: {
    borderRadius: 4,
    borderWidth: 0.5,
    borderColor: '#d6d7da',
  },
  title: {
    fontSize: 19,
    fontWeight: 'bold',
  },
  activeTitle: {
    color: 'red',
  },
});
```

Step2: Use a StyleSheet:
```
<View style={styles.container}>
  <Text style={[styles.title, this.props.isActive && styles.activeTitle]} />
</View>
```