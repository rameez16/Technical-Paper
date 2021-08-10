## Report on solid design principles
<hr>


## Executive Summery 
### this report is on solid design principles that can be used to refractor codebase
#### date  : 10-8-2021
#### name  : MUHAMMED RAMEES
  
## Introduction

Implementing Solid Design principles do make our code more robust ,manageable,extendable ,logical,and easier to read.using various solid design principle discussed here can effectively address the problems of unmanageable codebase .SOLID is based on 5 fundamental principles so i will be explaining each of the 5 SOLID design principle in detail so that we can implement in our design it to refract our code.
Following SOLID architecture also helps save time and effort in both development and maintenance.

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




## 1.Open-close principle


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
   





## 4. Interface Segregation Principle

 Interface segregation principle states:

> ### A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use

let us better understand above statement with an example.consider the code given below. Developer created a class 

```JAVa 

class  ISmartDevice
{
    void Print();

    void Fax();

    void Scan();
}
           
```                   





