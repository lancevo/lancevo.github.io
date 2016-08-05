---
layout: post
title:  "Angular 2 - Oh My Forms!"
date:   2016-08-02 14:05:00
categories: Angular
tags: angular2
#image:
published: true
---
Form is complex with all the business rules. Below is a few things that I ran into while coding Angular 2 rc 4, if you know a better solution, please let know.    

I have a form allows user to enter information such as age, name, etc.. One of the fields is age, user can enter either in `input[type=text]`
or `input[type=range]`. There are several requirements:

1. As user making changes to one input, it will updates another. 
2. It's a required field.
3. The age has to be in the range of 50 to 90.
4. If submit button is clicked, it must validates all inputs, and show errors if necessary.
  

2-Way Binding
=============

By default `ngModel` is a 1 way binding, to share the value in both fields (text & slider), 
it has to be set 2-way binding `[(ngModel)]`. Also, attribute `value` is set to `age` model, 
so it's updated automatically by Angular.

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

Form Validators 
===============

Angular has several built in form Validators, and `Validators.required` is one of them. I need to write custom validator 
to validate a range of numbers, which I created a numberRange validator [(gist)](https://gist.github.com/lancevo/bedaec7c7dfbecd9cb37544e85038553) . 
It's a higher-order function and returns `ValidatorFn` type. If validation is passed, it returns `null`, 
otherwise it returns an object with error name and its details.

```ts
export class App {
  myForm: FormGroup;
  age: number;
  
  constructor(private fb.FormBuilder) {

    this.myForm = this.fb.group({
      'age': [50, Validators.compose([Validators.required, validateNumberRange(50, 90)])]
    })
  }
  
  onSubmit(event, value){
    event.preventDefault();
    // do something here
    console.log('submitted value',value);
  }
}
```

```html
<form [formGroup]="myForm" (submit)="onSubmit($event, myForm.value)">
  <input type="number" [(ngModel)]="age" name="age" [value]="age">
  <input type="range"  [(ngModel)]="age" name="age" [value]="age" min="50" max="90" step="1">
  
 <!-- show error -->
  <div *ngIf="!myForm.controls['age'].valid">
    Please enter an age between 50 and 90
  </div>
  
  <button type="submit">Submit</button>
</form>
```

### Is there a way to validate all inputs? 
When the form is submitted, all inputs must be re-validated. I couldn't find a convenient method in `FormGroup` or `FormBuilder` to do this. However, there's a way to get around it with `FormControl` methods:

1. `updateValueAndValidity({onlySelf: false, emitEvent: true})` - re-validate this input, trigger the event to parent form.
2. `markAsDirty()` - when it's invalid. Why do this? It will remove `pristine` status of this input. I use this to control the display of error, and ensure the error is only shown when it's touched or the form is submitted. 



```ts
myForm: FormGroup;
age: number;
name: string;
  
constructor(private fb.FormBuilder) {

  this.myForm = this.fb.group({
    'age': [50, Validators.compose([Validators.required, validateNumberRange(50, 90)])],
    'name': ['', Validators.required]
  })
}

onSubmit(event, value) {
  let formValid = true;
  
  Object.keys(this.myForm.controls).forEach( key => {
    let control = this.myForm.controls[key];
    // revalidate input
    control.updateValueAndValidity({onlySelf: false, emitEvent: true });
    if (!control.valid) {
      formValid = false;
      control.markAsDirt();
    }
    
    if (formValid) {
      /// do something here
    }
  
  });
}
```

```html
<form [formGroup]="myForm" (submit)="onSubmit($event, myForm.value)">
  Name:
  <input type="text" ngModel name="name">
  <!-- 
    Notice !myForm.controls['name'].pristine, without it the error will display instantly because the initial value is empty,
    and it's a required field.
  -->
  <div *ngIf="!myForm.controls['name'].valid && !myForm.controls['name'].pristine">
    Please enter an age between 50 and 90
  </div>
  <br>
  Age:
  <input type="number" [(ngModel)]="age" name="age" [value]="age">
  <input type="range"  [(ngModel)]="age" name="age" [value]="age" min="50" max="90" step="1">
  
 <!-- show error -->
  <div *ngIf="!myForm.controls['age'].valid">
    Please enter an age between 50 and 90
  </div>
 
  
  <button type="submit">Submit</button>
</form>
```

`FormControl` [methods](https://angular.io/docs/ts/latest/api/common/index/AbstractControl-class.html) that dealing with an input. 

