# Proxy Pattern in C#

## Introduction

This repository contains an implementation of the Proxy Pattern in C#. The Proxy Pattern is a structural design pattern that provides a surrogate or placeholder for another object to control access to it. This is particularly useful in scenarios where an object is heavyweight, and you want to postpone its creation, or when access control to the object is necessary.

## Proxy Pattern Concept

In the Proxy Pattern, a proxy class represents the functionality of another class. This pattern involves three participants:

1. **Subject** - An interface which declares common operations for both the RealSubject and the Proxy.
2. **RealSubject** - The actual object that the Proxy represents.
3. **Proxy** - A class that maintains a reference to the RealSubject, controls access to it, and can perform additional actions (e.g., lazy initialization, logging, access control, etc.).

## Use Case in C#

In C#, the Proxy Pattern is beneficial when you're dealing with resource-intensive objects whose instantiation should be postponed until they're actually needed. This pattern is also useful for implementing access control or for adding other responsibilities to an object without modifying its code.

## Example: Document Editor

Consider an application like a document editor where loading large documents can be resource-intensive. We can use the Proxy Pattern to delay the loading of the document until it is actually required for display or processing.

### Participants

- `IDocument`: The Subject interface.
- `RealDocument`: The RealSubject class.
- `DocumentProxy`: The Proxy class.

### Code Implementation

```csharp
// Subject Interface
public interface IDocument
{
    void Display();
}

// RealSubject Class
public class RealDocument : IDocument
{
    private string _content;

    public RealDocument(string content)
    {
        LoadContent(content);
    }

    private void LoadContent(string content)
    {
        // Simulate time-consuming operation
        _content = content;
        Console.WriteLine("Document Loaded.");
    }

    public void Display()
    {
        Console.WriteLine($"Displaying Document: {_content}");
    }
}

// Proxy Class
public class DocumentProxy : IDocument
{
    private RealDocument _realDocument;
    private string _content;

    public DocumentProxy(string content)
    {
        _content = content;
    }

    public void Display()
    {
        // Lazy initialization: RealDocument is created only when it's actually needed
        if (_realDocument == null)
        {
            _realDocument = new RealDocument(_content);
        }
        _realDocument.Display();
    }
}

// Client code
class Program
{
    static void Main(string[] args)
    {
        IDocument document = new DocumentProxy("Hello, Proxy Pattern!");
        // The document is not loaded until Display is called
        document.Display();
    }
}
```
