---
layout: post
title:  "Angular 2 Common Errors"
date:   2016-02-27 06:00:00
categories: Angular
tags: angular2
#image:
published: true
---

# Angular 2 - Beta 7 

Many Angular 2 examples online are outdated. I have a mixed feelings for Angular 2 syntaxes, 
and hopefully it doesn't deviate much in the upcoming versions.  

### COMMON_DIRECTIVES 

**Directives:** `NgClass`, `NgStyle`, `NgIf`, `NgSwitch`, `NgFor`

Error ([StackOverFlow](http://stackoverflow.com/questions/34228971/cant-bind-to-ng-forof-since-it-isnt-a-known-native-property)): 
`Can't bind to 'ng-forOf' since it isn't a known native property`  
[Github - Typings](https://github.com/angular/angular/blob/2.0.0-beta.7/modules/angular2/src/common/common_directives.ts#L49-L49)  

Because I was using `*ng-for`, it was changed to `*NgFor`. 

```js
import {COMMON_DIRECTIVES} from 'angular2/common';
@Component({
  selector: 'name-list'
})
@View ({
    directives:[COMMON_DIRECTIVES],
    template: `
        <ul>
            <li *ngFor="#name of names">{{name}}</li>
        </ul>
    `
})
export class NameList {
    names:string[] = ['John','Ellen','Aida'];
}
```
