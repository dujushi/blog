layout: blog
title: CamelCase Json Result wth Json.NET
date: 2016-01-14 20:37:57
tags:
- Json.NET
categories:
- ASP.NET MVC
---
  .NET has PascalCase naming convention for properties while Javascript has camelCase. Is there an easy way to convert the Json result? The answer is Json.NET. This article will show you how to create a custom result with Json.NET.<!-- more -->

1. Install Newtonsoft.Json Nuget Package
2. Custom Action Result
```
public class JsonCamelCaseResult : ActionResult
{
    public Encoding ContentEncoding { get; set; }

    public string ContentType { get; set; }

    public object Data { get; set; }

    public override void ExecuteResult(ControllerContext context)
    {
        if (context == null)
        {
            throw new ArgumentNullException(nameof(context));
        }

        HttpResponseBase response = context.HttpContext.Response;
        response.ContentType = !string.IsNullOrEmpty(ContentType) ? ContentType : "application/json";
        if (ContentEncoding != null)
        {
            response.ContentEncoding = ContentEncoding;
        }
        if (Data != null)
        {
            var jsonSerializerSettings = new JsonSerializerSettings
            {
                ContractResolver = new CamelCasePropertyNamesContractResolver()
            };

            response.Write(JsonConvert.SerializeObject(Data, jsonSerializerSettings));
        }
    }
}
```
 We use CamelCasePropertyNamesContractResolver to convert PascalCase into camelCase.
3. Controller Extension Method
```
public static class JsonCamelCaseHelper
{
    public static JsonCamelCaseResult JsonCamelCase(this Controller controller, object data)
    {
        return new JsonCamelCaseResult
        {
            Data = data
        };
    }
}
```
 This extension method can save us some typing. We can use this.JsonCamelCase([data]) in any controllers. Or you can put this method into your base controller if you have one.
4. Usage
```
public ActionResult Index()
{
    var product = new Product
    {
        FancyName = "iPad Pro",
        SalesPrice = 12.10m
    };
    return this.JsonCamelCase(product);
}
```
 'FancyName' will be changed to 'fancyName' in the result.
### Gist
[https://gist.github.com/dujushi/890bf6b4455a58a7256e](https://gist.github.com/dujushi/890bf6b4455a58a7256e)
### References
1. [Serialize .NET objects as camelCase JSON](http://www.matskarlsson.se/blog/serialize-net-objects-as-camelcase-json)
2. [ASP.NET MVC and Json.NET](http://james.newtonking.com/archive/2008/10/16/asp-net-mvc-and-json-net)
3. [Setting the Default JSON Serializer in ASP.NET MVC](http://stackoverflow.com/questions/14591750/setting-the-default-json-serializer-in-asp-net-mvc)
