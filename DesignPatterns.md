# Contents
- [Contents](#contents)
- [Singleton pattern](#singleton-pattern)

# Singleton pattern
Implementing the Singleton pattern in C# ensures that a class has only one instance and provides a global point of access to it. Here's a simple implementation:

```csharp
public sealed class Singleton
{
    private static readonly Singleton _instance = new Singleton();

    // Private constructor to prevent instantiation from outside
    private Singleton() { }

    public static Singleton Instance
    {
        get
        {
            return _instance;
        }
    }

    // Example property
    public string Data { get; set; }
}
```

In this implementation:

1. **Sealed class**: Prevents inheritance, ensuring the class cannot be extended.
2. **Static readonly instance**: The Singleton instance is created when the class is first accessed, and the `readonly` modifier ensures it cannot be modified after creation.
3. **Private constructor**: Prevents creating new instances from outside the class.
4. **Public static property**: Provides global access to the single instance.

Usage:

```csharp
class Program
{
    static void Main()
    {
        var singleton = Singleton.Instance;
        singleton.Data = "Hello, Singleton!";
        
        Console.WriteLine(singleton.Data);
    }
}
```

This example demonstrates how to create and use a Singleton class in C#. The Singleton pattern is useful when you need to ensure that only one instance of a class exists and provides a single point of access to it.
