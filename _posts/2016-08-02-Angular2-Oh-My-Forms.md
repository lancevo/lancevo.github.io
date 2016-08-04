---
layout: post
title:  "Angular 2 Oh My Forms!"
date:   2016-02-27 06:00:00
categories: angular2
tags: angular2
#image:
published: true
---

Below is a list of issues that I ran into with form while using Angular 2 Release Candidate 4. 
There are things that you may find obvious, but not documented any on official angular documents make it difficult.

I have a form allow user to enter information such as age, etc... In the form, it allows user to enter an age either in `input[type=text]`
or `input[type=range]`. There are several requirements:

1. As user making changes to one input, it will updates another. 
2. The age has to be in the range of 50 to 90.
3. If submit button is clicked, it must validates all inputs, and show errors if necessary.
  
Simple enough? Let's go!

1. 2-Way Binding - Range Slider Input
=====================================

By default `ngModel` is a 1 way binding, so it has to be set to 2-way binding as `[(ngModel)]`. Also, `age` model has to be
assigned to `value` attribute on each `input` to update the value as `age` is changed.

```html
<form [formGroup]="myForm" (submit)="onSubmit($event, myForm.value)">
  <input type="number" [(ngModel)]="age" name="age" [value]="age">
  <input type="range"  [(ngModel)]="age" name="age" [value]="age" min="50" max="90" step="1">
  <button type="submit">Submit</button>
</form>
```

```ts
export class App {
  myForm: FormGroup;
  age: number;
}
```


