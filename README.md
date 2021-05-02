# SOLID prinicipals

<details>

<summary>Single Responsibility Principle</summary>

A class should have **one and only one** responsibility.  
We have a class `Customer`

```csharp
public class Customer
{
  public void Add(){
    try{
    }
    catch(Exception ex){
        System.IO.File.WriteAllText(@"c:\Error.txt", ex.ToString());
    }
  }
}
```

Here exception logging mechanism is written in class itself.  
As per this rule exception log related code should be written in another class, and that class's method should be called here.

</details>
<details>
  
<summary>Open-Closed Principle</summary>

Entities should be open for extension, but closed for modification.

```csharp
public class Customer
{
  public double GetDiscount(double totalPrice){
    return (totalPrice * 5)/100;
  }
}

public class SilverCustomer : Customer
{
public override double GetDiscount(double totalPrice){
return (totalPrice \* 10)/100;
}
}

public class GoldenCustomer : Customer
{
public override double GetDiscount(double totalPrice){
return (totalPrice \* 15)/100;
}
}

```

Here `Customer` class is closed for modification, but it is open for extention.
So `SilverCustomer` and `GoldenCustomer` classes inherit `Customer` class and override their methods.

</details>
<details>

<summary>Liskov Substitution Principle</summary>

Subclass/derived classes should be substitutable for their base/parent class

```csharp
public class Customer
{
  public void Add(){
    // save logic to db
  }
}

public class SilverCustomer : Customer
{
  public override void Add(){
    // save logic to db
  }
}

public class Enquiry : Customer
{
  public override void Add(){
    throw new Exception("For enquiry not allowed to save in db");
  }
}
```

Here `SilverCustomer` has method to save data in db, but `Enquiry` is not needed to save in db.
So here as per rule, both `SilverCustomer` and `Enquiry` are implemented in a way that they can convert to any child class on runtime.

</details>
<details>

<summary>Interface Segregation Principle</summary>

A client should not be forced to implement an interface that it doesn’t use.

```csharp
public interface ISave {
  void Add();
}

public interface IDiscount{
  double GetDiscount(double totalPrice);
}

public class SilverCustomer : ISave, IDiscount
{
  public void Add(){
    // save logic to db
  }

  public double GetDiscount(){
    // get discount logic
  }
}

public class Enquiry : IDiscount
{
  public double GetDiscount(){
    // get discount logic
  }
}
```

Here 2 new interfaces are defined, `IAdd` and `IDiscount`. So class which needs methods will only inherit that interface.  
So no need to forcefully implement `Add` method in `Enquiry`.

</details>
<details>

<summary>Dependency Inversion Principle</summary>

High-level modules should not depend on low-level modules. Both should depend on abstractions.

```csharp
public class Customer
{
  public ILogger _logger;
  public Customer(ILogger logger){
    _logger = logger;
  }
}
```

Here `ILogger` is passed in constructor as a dependecy of logger.

```csharp
public FileLogger : ILogger{
}
public DbLogger : ILogger{
}
```

So Here any logger can be passed in `Customer` class, no need to change `Customer` implementation.

```csharp
var customer1 = new Customer(new FileLogger());
var customer2 = new Customer(new DbLogger());
```

</details>
