---
layout: post
title:  "Number.prototype.toLocaleString() : a very helpful little method"
date:   2015-03-12 17:04:00
categories: javascript
tags: javascript
#image:
published: true
---

There are many times that I've written a small function to convert a number to a pretty formatted string like 12,345,678.90, 
and never took the time to look inside the Javascript already available method `Number.prototype.toLocaleString()`. What a 
useful little method, it converts a number to different locales, with its currency symbol and its language.  


More here: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString>


```javascript
var number = new Number(12345.6789);

// convert to locale string
console.log( number.toLocaleString('en',{minimumFractionDigits:2, maximumFractionDigits:2}) );
```