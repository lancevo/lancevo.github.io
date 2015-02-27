---
layout: post
title:  "protractorjs: select option with a value"
date:   2015-02-27 13:57:05
categories:
tags: angularjs, protractorjs
#image:
published: true
---

There are a lot of outdated protractorjs articles out there on the Internet.
If you see anything like `by.input` or `ptor.` are probably outdated. So go to the source <http://angular.github.io/protractor/#/api-overview> to see the latest and official docs.

Now you are here, probably wondering if protractor has a method to choose an option with a `value`, or text for that matters. Well, I ran into this issue real quick after writing a few specs for my project. So after reading the official docs, there's none, but it can be done.

Here's how:

    1. use `elementFinder` to find all the sub-elements by tag name `option`
    2. compare each option value with the value you want to choose
    3. if it matches, then click that element

Simple Right? Let put it code:

```
function selectOptionByValue(elementFinder, value) {
  elementFinder.all(by.tagName('option'))
    .then(function findMatchingOption(options) {

      options.some(function (option) {
        option.getAttribute('value').
          then(function (optionValue) {
            if (optionValue === value) {
              option.click();
              return true;
            }
          });
      });

    })
}


var elementFinder = element(by.id('selectId'));

selectOptionByValue(elementFinder, 'somevalue');
```

Why are there a lot of `then`? All methods of elementFinder return a promise.

