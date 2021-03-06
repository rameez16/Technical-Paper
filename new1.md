## Report on solid design principles
<hr>


## Executive Summery 
### this report is on solid design principles that can be used to refractor codebase
#### date  : 10-8-2021
#### name  : MUHAMMED RAMEES
  
## Introduction

Implementing Solid Design principles do make our code more robust ,manageable,extendable ,logical,and easier to read.using various solid design principle discussed here can effectively address the problems of unmanageable codebase .SOLID is based on 5 fundamental principles so i will be explaining each of the 5 SOLID design principle in detail so that we can implement in our design it to refract our code.
Following SOLID architecture also helps save time and effort in both development and maintenance.

* It reduces the dependencies so that a block of code can be changed without affecting the other code blocks.
* The principles intended to make design easier, understandable.
 * By using the principles, the system is maintainable, testable, scalable, and reusable.
* It avoids the bad design of the software.





SOLID principles, which acronym means the following:
>1. S: Single Responsibility Principle
> 2. O: Open-Closed Principle
> 3. L: Liskov Substitution Principle
 >4. I: Interface Segregation Principle
 >5. D: Dependency Inversion Principle



## 1.single responsibility principle
As the name suggests the class , method or function must be designed to handle single task only.
 Another way of describing it would be class or function should have only one reason to change.When a class serves multiple purposes or responsibilities, it should be made into a new class.The ultimate goal of the class should be to serve single purpose only. 


Have a look at the class given below.the class 'UserSetting clearly violate the SRP principle because it handle multiple responsibility.it contains two function doing different task.one being change Setting and another verify credentials .so its a clear violation of single responsibility principle.

```javascript 
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```
How to rewrite above class to follow SRP principle.solution is simple and straightforward.break the class two each doing separate functions.

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

examine the above code.the class UserSetting has been disintegrated into two classes. 
        
1. UserAuth-responsible for verifyCredentials
1. UserSetting -responsible for change settings.

 




## 2.Open-close principle


> ### this principle states that the class/functions is open for extension but closed for modification
what does this statement actually imply to?. according to this principle we are allowed to extend a class for enhanced functionality.but we are never allowed to modify it.Any kind of modification on the existing class is prohibited.we cant touch existing code.what to do if we want to update the class for better functionality or we want to add a new requirement by strictly following Open-Close Principle?.Just add more code without touching the existing code. extend any part of it’s features and inject your custom implementation out of the box. I
. Which means, any new functionality should be implemented by adding new classes, attributes and methods, instead of changing the current ones or existing ones.
* Extention -Allowd
* Modification-Prohibited

lets understand this better by an example.Consider the following code.Here the developer has created a class that calculate interest for All savings account based on type of account.two account type are there .
1. regular 
2. salary 
  
  
  

```JAVA Public class SavingAccount  
{  
    //Other method and property and code  
    Public decimal CalculateInterest(AccountType accountType)  
    {  
        If(AccountType==”Regular”)  
        {  
            //Calculate interest for regular saving account based on rules and   
            // regulation of bank  
            Interest = balance * 0.4;  
            If(balance < 1000) interest -= balance * 0.2;  
            If(balance < 50000) interest += amount * 0.4;  
        }  
        else if(AccountType==”Salary”)  
        {  
            //Calculate interest for saving account based on rules and regulation of   
            //bank  
            Interest = balance * 0.5;  
        }  
    }  
}  
```
now bank has revised its requirement and want to add one more account type named Student.One way to do this is to modify the current class and add one more if-else statement for student type.But this is a direct violation of OCP principle.we will be modifying the class here.which is not allowed.so how to re-design the code so that it obey OCP principle.a good practice to write the above logic is given below.

```JAVA Interface ISavingAccount  
{  
   //Other method and property and code  
   decimal CalculateInterest();  
}  
Public Class RegularSavingAccount : ISavingAccount  
{  
  //Other method and property and code related to Regular Saving account  
  Public decimal CalculateInterest()  
  {  
    //Calculate interest for regular saving account based on rules and   
    // regulation of bank  
    Interest = balance * 0.4;  
    If(balance < 1000) interest -= balance * 0.2;  
    If(balance < 50000) interest += amount * 0.4;  
  }  
}  
  
Public Class SalarySavingAccount : ISavingAccount  
{  
  //Other method and property and code related to Salary Saving account`  
  Public decimal CalculateInterest()  
  {  
    //Calculate interest for saving account based on rules and regulation of   
    //bank  
    Interest = balance * 0.5;  
  }  
}  
```

here instead of using if else statements we have created separate class for each Account type.Now if the bank needs to add new account type say Student,Developer can add a new class Student and achieve the requirements of the bank.this is done without modifying existing class.He just extended the class for new functionality.Now the code obeys OCP principle.3. So, at any given point of time when there is a requirement change instead of touching the existing functionality it’s always suggested to create new classes and leave the original implementation untouched

what are the consequences of not following ocp?

1. If a class or a function always allows the addition of new logic, as a developer we end up testing the entire functionality along with the requirement.
2.  Not following the Open Closed Principle breaks the SRP since the class or function might end up doing multiple tasks.
 
3. Also, if the changes are implemented on the same class, Maintenance of the class becomes difficult since the code of the class increases by thousands of unorganized lines. 
   
# 3.Liskov’s Substitution Principle:

The principle was introduced by Barbara Liskov in 1987 and according to this principle “Derived or child classes must be substitutable for their base or parent classes“. This principle ensures that any class that is the child of a parent class should be usable in place of its parent without any unexpected behavior.

  You can understand it in a way that a farmer’s son should inherit farming skills from his father and should be able to replace his father if needed. If the son wants to become a farmer then he can replace his father but if he wants to become a cricketer then definitely the son can’t replace his father even though they both belong to the same family hierarchy.

        


 ```java



     public abstract class Fruit
{
    public abstract string GetColor();
}

public class Orange : Fruit
{
    public override string GetColor()
    {
        return "Orange Color";
    }
}

public class Apple : Fruit
{
    public override string GetColor()
    {
        return "Red color";
    }
}

class Program
{
    static void Main(string[] args)
    {
        Fruit fruit = new Orange();

        Console.WriteLine(fruit.GetColor());

        fruit = new Apple();

        Console.WriteLine(fruit.GetColor());
    }
}
  ```



# 4.Interface Segregation Principle

 Interface segregation principle states:

> ### A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use

let us better understand above statement with an example.consider the code given below. Developer created a class ISmartDevice which have the interface for print,fax and scan.

```JAVa 

interface  ISmartDevice
{
    void Print();

    void Fax();

    void Scan();
}
 ```
 lets implement the class in a all in one printer.it work just fine.



``` JAVA                   
   class AllInOnePrinter : ISmartDevice
{
    public void Print()
    {
         // Printing code.
    }

    public void Fax()
    {
         // Beep booop biiiiip.
    }

    public void Scan()
    {
         // Scanning code.
    }
}          
  ```

 Now the client comes with an Electronic printer which can ony Print.FAX and Scan functionality is not there in the printer.lets see what happens  when we inherit the Electronic Printer class  from  ISmartdevice class.see the code below.

 ```JAVA 
         class EconomicPrinter : ISmartDevice
{
    public void Print()
    {
        //Yes I can print.
    }

    public void Fax()
    {
        throw new NotSupportedException();
    }

    public void Scan()
    {
        throw new NotSupportedException();
    }
}
```
   Here the client is forced to implement functions that he never use.so this is a violation of ISP.a better way of design is given below.

 ```JAVA
interface IPrinter{
    void Print();
}

interface IFax{
    void Fax();
}

interface IScanner{
    void Scan();
}  
```
here each interface is separated.this make the reuse more flexible and client  do not have to worry about implementing irrelevant logic in the design.so its always better to create small interfaces with single functionality rather than creating big interface with lots of methods.doing this way  make the code more easier to maintain and  provides improved flexibility.


## 5. Dependency Inversion Principle (DIP)

statement for this principle can be written as

>  High-level modules should not depend on low-level modules. Both should depend on abstractions.Abstractions should not depend on details. Details should depend on abstractions.

to understand above statements les's  understand the definition of each word

* high level module-A high-level module is a module that always depends on other modules




according this principle High-Level modules and low level modules should be loosely coupled or de-coupled.both class shouldn't depend on each other.


```java

     class MySQLConnection
{
    public function connect()
    {
        // handle the database connection
        return 'Database connection';
    }
}

class PasswordReminder
{
    private $dbConnection;

    public function __construct(MySQLConnection $dbConnection)
    {
        $this->dbConnection = $dbConnection;
    }
}
    
   ```


First, the MySQLConnection is the low-level module while the PasswordReminder is high level, but
This snippet above violates this principle as the PasswordReminder class is being forced to depend on the MySQLConnection class.

Later, if you were to change the database engine, you would also have to edit the PasswordReminder class, and this would violate the open-close principle.

The PasswordReminder class should not care what database your application uses. To address these issues, you can code to an interface since high-level and low-level modules should depend on abstraction:


The PasswordReminder class should not care what database your application uses. To address these issues, you can code to an interface since high-level and low-level modules should depend on abstraction:

```JAVA
interface DBConnectionInterface
{
    public function connect();
}
```




The interface has a connect method and the MySQLConnection class implements this interface. Also, instead of directly type-hinting MySQLConnection class in the constructor of the PasswordReminder, you instead type-hint the DBConnectionInterface and no matter the type of database your application uses, the PasswordReminder class can connect to the database without any problems and open-close principle is not violated.

```JAVA
          class MySQLConnection implements DBConnectionInterface
{
    public function connect()
    {
        // handle the database connection
        return 'Database connection';
    }
}

class PasswordReminder
{
    private $dbConnection;

    public function __construct(DBConnectionInterface $dbConnection)
    {
        $this->dbConnection = $dbConnection;
    }
}

 ```

 This code establishes that both the high-level and low-level modules depend on abstraction.



 ## CONCLUSION
 In this article, you were presented with the five principles of SOLID Code. Projects that adhere to SOLID principles can be shared with collaborators, extended, modified, tested, and refactored with fewer complications.           





