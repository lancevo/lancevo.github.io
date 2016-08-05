---
layout: post
title:  "Number.prototype.toLocaleString() : a very helpful little method"
date:   2015-03-12 17:04:00
categories: Dev-Misc
tags: Javascript
#image:
published: true
---

`Number.prototype.toLocaleString()` a very helpful function to convert a number to locale format, ie. 12,345,678.90, 

More here: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString>


```javascript
var number = new Number(12345.6789);

// convert to locale string
console.log( number.toLocaleString('en',{minimumFractionDigits:2, maximumFractionDigits:2}) );
```