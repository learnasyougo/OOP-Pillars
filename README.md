# Pillars of Object Oriented Programming
1. [Encapsulation](#encapsulation)
2. [Abstraction](#abstraction)
3. [Inheritance](#inheritance)
4. [Polymorphism](#polymorphism)
5. [References](#references)


## Encapsulation
> **Hiding unnecessary implementation details from the outside world.**
Encapsulatiion is the hiding of data implementation by restricting access to accessors and mutators. It's major benefit is that it reduces system complexity by hiding unnecessary details from the outside world.
It is actually 

*Accessor*: a method used to ask an object about itself, usually in OOP in the form of a property with a get 'accessor' method.<br/>
*Mutator*: a public method used to modify the state on an object, by hiding the exact implementation how the data gets modified.

### Example
In this example we encapsulate the data and methods used to calculate a person's `Age`. For the outside world, it's just a property on the `Person` object we can use.

```
public class Person {
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int Age { 
        get {
            var today = DateTime.Today;
            var age = DateTime.Today - DateOfBirth.Year;

            if(age > today.AddYears(-age)) {
                age--;
            }

            return age;
        }
    }
    public DateTime DateOfBirth { get; set; }    
}
```

## Abstraction
> Abstraction is the implementation on an object that contains the same essential properties and actions we can find in the original object we are representing.

## Inheritance
> Inheritance is a way to reuse code of existing objects, or to establish a subtype from an existing object.

*Subclass*: a modular, derivative class, that inherits one or more properties from aanother class (it's *superclass*).<br/>
*Superclass*: establishes a common interface and/or foundation of functionality which specialized *subclassed* can inherit.

### Example
In this example we have a superclass `Person`, that implements common methods and properties, the subclasses `Customer` and `Employee` each have their own varying data and behaviour.

```
public class Person {
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int Age { 
        get {
            var today = DateTime.Today;
            var age = DateTime.Today - DateOfBirth.Year;

            if(age > today.AddYears(-age)) {
                age--;
            }

            return age;
        }
    }
    public DateTime DateOfBirth { get; set; }
}
```

```
public class Customer : Person {
    public string CustomerNumber { get; set; }
}
public class Employee : Person {
    public int DepartmentId { get; set; }
    public int ContractId { get; set; }    
}
```

## Polymorphism
> One name, many forms - poly-*(many)*-morph-*(forms)*-ism, refers to the ability to take multiple form.

*Overriding*: decision to use which method is used at runtime.<br/>
*Overloading*: decision to use which method is made at compile time.

### Example
Building from the example superclass `Person`, and the subclasses `Customer` and `Employee`, we will implement to method `ToString` differently, we *override* the method the method of the superclass. Since `ToString` is baked into each object in C#, we also use the keyword `override` in the superclass `Person`.

```
public class Person {
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int Age { 
        get {
            var today = DateTime.Today;
            var age = DateTime.Today - DateOfBirth.Year;

            if(age > today.AddYears(-age)) {
                age--;
            }

            return age;
        }
    }
    public DateTime DateOfBirth { get; set; }
    public override ToString() {
        return $"{FirstName} {LastName}";
    }
}
```

```
public class Customer : Person {
    public string CustomerNumber { get; set; }
    public override ToString() {
        return $"{CustomerNumber} - {FirstName} {LastName}";
    }
}
public class Employee : Person {
    public int DepartmentId { get; set; }
    public int ContractId { get; set; }
    public override ToString() {
        return $"{DepartmentId} - {FirstName} {LastName} ({ContractId})";
    }
}
```

Below is some sample output

```
var people = new Person[] {
    new Employee() { FirstName = "John", LastName = "Doe", DateOfBirth = new DateTime(1985, 10, 11), DepartmentId = 5, ContractId = 854722 },
    new Customer() { FirstName = "Jane", LastName = "Doe", DateOfBirth = new DateTime(1985, 10, 11), CustomerNumber = 5497141 }
};

foreach(var person in people) {
    Console.WriteLine(person.ToString());
}

// Output:
// 5 - John Doe (854722)
// 5497141 Jane Doe
```

### C# specific "new" keyword
C# has a modifier `new` which can be used on `virtual` or `abstract` methods and properties. When used it instructs the compiler to use the `subclass`'s implementation in stead of the `superclass`'s. See [this blog post on MSDN](https://blogs.msdn.microsoft.com/csharpfaq/2004/03/12/whats-the-difference-between-override-and-new/) on the difference between `override` and `new`. **TL;DR** please try to **avoid** using the `new` keyword as it is confusing behaviour.

```
public class Customer : Person {
    public string CustomerNumber { get; set; }
    public new ToString() {
        return $"{CustomerNumber} - {FirstName} {LastName}";
    }
}
public class Employee : Person {
    public int DepartmentId { get; set; }
    public int ContractId { get; set; }
    public new ToString() {
        return $"{DepartmentId} - {FirstName} {LastName} ({ContractId})";
    }
}

var people = new Person[] {
    new Employee() { FirstName = "John", LastName = "Doe", DateOfBirth = new DateTime(1985, 10, 11), DepartmentId = 5, ContractId = 854722 },
    new Customer() { FirstName = "Jane", LastName = "Doe", DateOfBirth = new DateTime(1985, 10, 11), CustomerNumber = 5497141 }
};

foreach(var person in people) {
    Console.WriteLine(person.ToString());
    if(person is Employee) {
        Employee emp = (Employee) person;
        Console.WriteLine(emp.ToString());
    }
    if(person is Customer) {
        Customer cust = (Customer) person; 
        Console.WriteLine(cust.ToString());
    } 
}

// Output:
// John Doe ................... // calls Person.ToString()
// 5 - John Doe (854722) ...... // calls Employee.ToString() because it was cast to Employee
// Jane Doe ................... // calls Person.ToString()
// 5497141 Jane Doe ........... // calls Customer.ToString() because it was cast to Customer
```

As said before, it's best **not to use** this feature, as it is only useful in egde cases and not for your typical project.

## References
- https://chesterli0130.wordpress.com/2012/10/04/four-major-principles-of-object-oriented-programming-oop/
- https://en.wikipedia.org/wiki/Object-oriented_programming
- http://themoderndeveloper.com/the-modern-developer/back-to-basics-three-or-four-oop-pillars/
