# Fixie

Fixie is a .NET convention-based test framework similar to NUnit and xUnit, but with an emphasis on low-ceremony defaults and flexible customization. Fixie's development is documented at [http://plioi.github.io/fixie](http://plioi.github.io/fixie).

## How do I install Fixie?

First, [install NuGet](http://docs.nuget.org/docs/start-here/installing-nuget). Then, install the [Fixie NuGet package](https://www.nuget.org/packages/Fixie) from the package manager console:

    PM> Install-Package Fixie

## How do I use Fixie?

1. Create a Class Library project to house your test fixtures.
2. Add a reference to Fixie.dll.
3. Add fixture classes and test cases to your testing project.
4. Use the console runner from a command line to execute your tests:

    Fixie.Console.exe path/to/your/test/project.dll
5. Use the TestDriven.NET runner from within Visual Studio, using the same keyboard shortcuts you would use for NUnit tests.

## Default Convention

When using the default convention, a test fixture is any concrete class that has a default constructor and a name ending in "Tests".  Within such a fixture class, a test case is any public instance void method with zero arguments.  Additionally, test cases include public instance async methods returning `Task` or `Task<T>`.

One instance of your fixture class is constructed for *each* test case. To perform setup steps before each test case executes, use the fixture's default constructor. To perform cleanup steps after each test cases executes, implement `IDisposable` and place cleanup code within the `Dispose()` method.

No [Attributes], no "using Fixie;" statement, no muss, no fuss.

## How do I make assertions?

Most test frameworks such as NUnit or xUnit include their own assertion libraries so that you can make statements like this:

```cs
Assert.AreEqual(expected, actual);
```

Assertion libraries are orthogonal to test frameworks.  Your choice of assertion library should be independent of your choice of test framework.  Therefore, Fixie will *never* include an assertion library.

Here are some useful third-party assertion libraries:

* [Should](http://nuget.org/packages/Should/)
* [Shouldly](http://nuget.org/packages/Shouldly/)

## Example
```cs
using Should;

public class CalculatorTests
{
    readonly Calculator calculator;

    public CalculatorTests()
    {
        calculator = new Calculator();
    }

    public void ShouldAdd()
    {
        calculator.Add(2, 3).ShouldEqual(5);
    }

    public void ShouldSubtract()
    {
        calculator.Subtract(5, 3).ShouldEqual(2);
    }

    public async Task SupportsAsyncTestCases()
    {
        int result = await AddAsync(2, 3);

        result.ShouldEqual(5);
    }

    private Task<int> AddAsync(int x, int y)
    {
        return Task.Run(() => calculator.Add(x, y));
    }
}
```
