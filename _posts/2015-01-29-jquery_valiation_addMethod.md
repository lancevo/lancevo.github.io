---
layout: post
title:  "jquery Validation addMethod()"
date:   2015-01-29 12:34:25
categories:
tags: jquery-validation
#image:
published: true
---

#An example of addMethod()

In a use case where I have to compare an input with another input,
jQuery Validation Plugin provides a helper method `addMethod` to write my own validator.

Example:
I created a form to ask user to enter his/her current age and retirement age, and retirement age must be greater current age.

See [addMethod notation](http://jqueryvalidation.org/jQuery.validator.addMethod/).

[Demo](http://jsbin.com/covawaboxi/1/edit?html,js,output)

(for brevity, I stripped all the unnecessary html attributes)

```html
<form>
    <label>Current Age</label>
    <input id="age" type="number" name="age" required value="60"><br>
    <label>Age At Retirement</label>
    <input type="number" name="ageAtRetirement" required value="50"><br>
    <input type="submit" value="check">
</form>
```


```javascript
$.validator.addMethod('greaterThan',
    function(value, element, params){
        // It must returns a true or false.
        // A false value will invokes error message
        return value > $(params).val();
    },
    // default error message
    'The age when payments begin must be greater than the current age. ');


var validationConfigs = {
    rules: {

      ageAtRetirement : {  // input name attribute
        name of the method created above, and `#age` is id of the input field `age`
        greaterThan : '#age'
      }
    },
    // customize error messages
    messages: {
      ageAtRetirement : { // for ageAtRequirement input
        // error message when retirement age is smaller than current age
        greaterThan : 'Retirement age must be greater than current age'
      }
    }
  };

// to run **Validation** with the rules above,
// pass the configs object `validationConfigs` in the `validate()` param
$('form').validate(validationConfigs);
```



# Bonus: validate input value on blur, and add classnames

Validation has many other events: **onsubmit, onfocusout**,etc. These events are configurable in the similar fashion as `rules` above.
To validate an input as soon user moves away from the input field, add `onfocusout` method, and use `element()` method to validate the
that input.

```javascript

var validationConfigs = {
    onfocusout: function onfocusout(input, event){
        this.element(input);
    },
    rules: {.... },
    messages: {...}
}
```

[Demo](http://jsbin.com/jiqidoruja/1/edit)

For other things such adding my own classnames for errors or valid inputs,
see `errorClass`, `validClass`, `highlight`, `unhighlight`, `errorPlacement` on this page <http://jqueryvalidation.org/validate/>



