---
layout: post
title:  "Custom validation for jQuery Validation"
date:   2015-01-29 12:34:25
categories: Dev-Misc
tags: jquery-validation
#image:
published: true
---

jQuery Validation - a form validation plug-in covers many common validations. There are times, a project requires a custom validation, it can be easily to extend with [`addMethod()`]((http://jqueryvalidation.org/jQuery.validator.addMethod/).

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
        return value > $(params).val();  // It must returns a boolean, true: passed or false: failed.
    },
    'The age when payments begin must be greater than the current age. ');  // default error message


var validationConfigs = {
    rules: {
      ageAtRetirement : {  // input's attribute name
        greaterThan : '#age' // greaterThan: is the name of the method, #age: if the inputs to compare
      }
    },
    // customize error message
    messages: {
      ageAtRetirement : {
        greaterThan : 'Retirement age must be greater than current age'
      }
    }
  };

$('form').validate(validationConfigs);
```

#validate onfocusout, onkeyup, onsubmit...

What about if I want to validate an input field as soon user moves to the next input field (onfocusout)? Or validate characters while user's typing?
jQuery Validation supports these events, it can be easily configured in the object like the example below.


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


#Error message placement, add or remove classes


`highlight`: it's being used to add error classes etc. an input failed validation
`unhighlight`: when an input passed validation, use this method to remove previous error classes
`errorPlacement`:

```javascript
var validationConfigs = {
      highlight: function(element, errorClass, validClass) {
        $(element)
          .closest('.form-group')
          .addClass('has-error');
      },
      unhighlight: function(element, errorClass, validClass) {
        $(element)
          .closest('.form-group')
          .removeClass('has-error');
      },
      errorPlacement: function(error, element) {
        var $element = $(element);
        $element.parent().append(error);
      }
   }
};
```

See docs: <http://jqueryvalidation.org/validate/>



