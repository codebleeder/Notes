
# Contents
- [Contents](#contents)
- [Action vs Func](#action-vs-func)
- [Extension method](#extension-method)

# Action vs Func
Delegates are type-safe function pointers

Action: no return type
Func: have return type

# Extension method
Extension methods allow you to add new methods to existing types without modifying the original type or creating a new derived type. They are defined as static methods but are called as if they were instance methods on the extended type.

Here's an example of an extension method in C# that adds a `ToTitleCase` method to the `string` class:

1. **Define the extension method**:

```csharp
using System.Globalization;

public static class StringExtensions
{
    public static string ToTitleCase(this string str)
    {
        if (string.IsNullOrEmpty(str))
        {
            return str;
        }

        TextInfo textInfo = CultureInfo.CurrentCulture.TextInfo;
        return textInfo.ToTitleCase(str.ToLower());
    }
}
```

2. **Usage**:

```csharp
using System;

class Program
{
    static void Main()
    {
        string example = "hello world!";
        string titleCaseExample = example.ToTitleCase();
        
        Console.WriteLine(titleCaseExample); // Output: "Hello World!"
    }
}
```

In this example:
- The `StringExtensions` class contains the `ToTitleCase` extension method.
- The `this string str` parameter in the method signature indicates that this is an extension method for the `string` type.
- The `ToTitleCase` method converts a string to title case using the `TextInfo` class from the `System.Globalization` namespace.

To use the extension method, simply include the namespace where the `StringExtensions` class is defined, and call the method as if it were a regular instance method on the `string` object. In the `Main` method, we demonstrate how to convert the string "hello world!" to title case using the `ToTitleCase` extension method.

