# Concept Overview

## What is Inheritance?
- Inheritance is an OOP concept where a child class acquires the properties and methods of a parent class.
- It helps us create a new class based on an existing class instead of writing the same code again.
- Inheritance represents an IS-A Relationship. For example: SavingsAccount IS-A Account, CurrentAccount IS-A Account
## Why do we need Inheritance?
- ✅ Code Reusability
- ✅ Reduces Duplicate Code
- ✅ Easier Maintenance
- ✅ Creates a logical relationship between classes

<img src="https://drive.google.com/file/d/1K4ZF7FqX6XJCWz0VKNnG_uisj74CIicP/view?usp=sharing" alt="Inheritance Image">

# C# Code Example

``csharp
class Account
{
    public int AccountNumber;
    public string HolderName;

    public void GetAccountDetails()
    {
        Console.WriteLine($"Account No : {AccountNumber}");
        Console.WriteLine($"Holder Name : {HolderName}");
    }
}

//Derived Class - 1
class SavingsAccount : Account
{
    public double InterestRate;

    public void CalculateInterest()
    {
        Console.WriteLine($"Interest Rate : {InterestRate}%");
    }
}

//Derived Class - 2
class CurrentAccount : Account
{
    public double OverdraftLimit;

    public void ShowOverdraftLimit()
    {
        Console.WriteLine($"Overdraft Limit : {OverdraftLimit}");
    }
}

class Program
{
    static void Main()
    {
        SavingsAccount sa = new SavingsAccount();

        sa.AccountNumber = 1001;
        sa.HolderName = "Rahul";
        sa.InterestRate = 6.5;

        sa.GetAccountDetails();     // Inherited method
        sa.CalculateInterest();     // Own method
    }
}
```
