layout: blog
title: Generate A Dropdown List for Enum
date: 2015-11-26 19:56:52
tags:
- Dropdown List
- Enum
- C Sharp
categories:
- ASP.NET MVC
---
We often use an Enum in C# to define all options for a field, such as a category Enum:
```
public enum CategoryEnum
{
    Book,
    Food,
    Tourism
}
```
And we need a dropdown list for this category field. How would you implement this?<!-- more -->

### Basic
First, we get all values of CategoryEnum:
```
Enum.GetValues(typeof(CategoryEnum))
```
Then, we generate all select items for these values:
```
Enum.GetValues(typeof(CategoryEnum)).Cast<CategoryEnum>().Select(t => new SelectListItem { Text = t.ToString(), Value = ((int)t).ToString() })
```
Here we use `Cast<CategoryEnum>()` to cast all the values so we can apply Select method on them.
Finally, we have our dropdown list:
```
@Html.DropDownListFor(m => m.Category, Enum.GetValues(typeof(CategoryEnum)).Cast<CategoryEnum>().Select(t => new SelectListItem { Text = t.ToString(), Value = ((int)t).ToString() }), "Select one")
```
### Custom Text
We can add descriptions to CategoryEnum and use them as the text value of each select list item.
```
public enum CategoryEnum
{
    [Description("Fancy Book")]
    Book,
    [Description("Delicious Food")]
    Food,
    [Description("The World Is Big")]
    Tourism
}
```
We then get the description with Attribute.GetCustomAttribute method.
```
((DescriptionAttribute)Attribute.GetCustomAttribute((t.GetType()).GetField(t.ToString()), typeof(DescriptionAttribute))).Description
```
The updated code for the dropdown list would be:
```
@Html.DropDownListFor(m => m.Category, Enum.GetValues(typeof(CategoryEnum)).Cast<CategoryEnum>().Select(t => new SelectListItem { Text = ((DescriptionAttribute)Attribute.GetCustomAttribute((t.GetType()).GetField(t.ToString()), typeof(DescriptionAttribute))).Description, Value = ((int)t).ToString() }), "Select one")
```
### Option Group
Sometimes we only want to display a subset of the Enum. We can define a static class for this so it can be shared everywhere.
```
public static class CategoryEnumGroups
{
    public static CategoryEnum[] OutdoorGroup = { CategoryEnum.Food, CategoryEnum.Tourism };
}
```
Then we can filter them with Where method.
```
Enum.GetValues(typeof(CategoryEnum)).Cast<CategoryEnum>().Where(t => CategoryEnumGroups.OutdoorGroup.Contains(t))
```
Final dropdown list code:
```
@Html.DropDownListFor(m => m.Category, Enum.GetValues(typeof(CategoryEnum)).Cast<CategoryEnum>().Where(t => CategoryEnumGroups.OutdoorGroup.Contains(t)).Select(t => new SelectListItem { Text = ((DescriptionAttribute)Attribute.GetCustomAttribute((t.GetType()).GetField(t.ToString()), typeof(DescriptionAttribute))).Description, Value = ((int)t).ToString() }), "Select one")
```
