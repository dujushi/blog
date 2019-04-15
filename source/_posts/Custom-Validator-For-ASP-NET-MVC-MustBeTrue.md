layout: blog
title: 'Custom Validator For ASP.NET MVC: MustBeTrue'
date: 2015-11-18 20:22:05
tags: 
- Validation
categories: 
- ASP.NET MVC
---
Recently I was working on a small project to add a checkbox for users to accept Terms and Conditions during registration. In order to use the built-in validation functions by ASP.NET MVC, I wrote a small custom validator for this: MustBeTrue.<!-- more -->

1. Create MustBeTrueAttribute.cs file under Attributes folder
```
[AttributeUsage(AttributeTargets.Property, AllowMultiple = false, Inherited = true)]
public class MustBeTrueAttribute : ValidationAttribute, IClientValidatable
{
	// for server side validation
	public override bool IsValid(object value)
	{
		return value is bool && (bool)value;
	}

	// add data- to elements for client side validation
	public IEnumerable<ModelClientValidationRule> GetClientValidationRules(ModelMetadata metadata, ControllerContext context)
	{
		var clientValidationRule = new ModelClientValidationRule()
		{
			ErrorMessage = FormatErrorMessage(metadata.GetDisplayName()),
			ValidationType = "mustbetrue"
		};
		return new[] { clientValidationRule };
	}
}
```

2. Create an adapter for Unobtrusive: jquery.validate.unobtrusive.mustbetrue.js
```
(function ($) {
    $.validator.addMethod("mustbetrue", function (value, element, params) {
		//check whether the element is checked
        return $(element).is(":checked");
    });
    $.validator.unobtrusive.adapters.addBool("mustbetrue");
}(jQuery));
```

3. Usage in View Model
```
[MustBeTrue(ErrorMessage = "Please accept the Terms & Conditions.")]
[DisplayName("I accept the Terms & Conditions.")]
public bool AcceptTerms { get; set; }
```

### Github
[https://github.com/dujushi/snippets/tree/master/MustBeTrue](https://github.com/dujushi/snippets/tree/master/MustBeTrue)

### Reference:
1. [ASP.NET MVC - Required Checkbox with Data Annotations](http://jasonwatmore.com/post/2013/10/16/ASPNET-MVC-Required-Checkbox-with-Data-Annotations.aspx)
2. [THE COMPLETE GUIDE TO VALIDATION IN ASP.NET MVC 3](http://www.devtrends.co.uk/blog/the-complete-guide-to-validation-in-asp.net-mvc-3-part-1)
2. [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
