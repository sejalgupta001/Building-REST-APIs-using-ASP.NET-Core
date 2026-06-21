# Collections in C#

## What is a Collection?

A **Collection** is a class that allows us to store and manage multiple objects dynamically.

### Why Use Collections?

* Dynamic Size
* Easy Insertion and Deletion
* Searching and Sorting Support
* Better Flexibility than Arrays

---

# Collection Hierarchy

```text
Collections
│
├── Non-Generic Collections
│   ├── ArrayList
│   ├── Hashtable
│   ├── Queue
│   ├── Stack
│   └── SortedList
│
├── Generic Collections
│   ├── List<T>
│   ├── Dictionary<TKey,TValue>
│   ├── SortedDictionary<TKey,TValue>
│   ├── SortedList<TKey,TValue>
│   ├── HashSet<T>
│   ├── SortedSet<T>
│   ├── Queue<T>
│   ├── Stack<T>
│   ├── LinkedList<T>
│   └── PriorityQueue<TElement,TPriority>
│
├── Concurrent Collections
│   ├── ConcurrentDictionary<TKey,TValue>
│   ├── ConcurrentQueue<T>
│   ├── ConcurrentStack<T>
│   ├── ConcurrentBag<T>
│   └── BlockingCollection<T>
│
└── Specialized Collections
    ├── StringCollection
    ├── StringDictionary
    ├── NameValueCollection
    ├── OrderedDictionary
    └── HybridDictionary
```

---
### What is FIFO?

FIFO means First In First Out and is used by Queue.

### What is LIFO?

LIFO means Last In First Out and is used by Stack.

---

# Types of Collections

## 1. Non-Generic Collections

#### Definition:
#### Non-Generic Collections are collections that store data as the object type. They can store different types of data in the same collection.

| Collection   | Generic Alternative          | 
| ------------ | ---------------------------- |
| `ArrayList`  | `List<T>`                    |
| `Hashtable`  | `Dictionary<TKey,TValue>`    |
| `Queue`      | `Queue<T>`                   |
| `Stack`      | `Stack<T>`                   |
| `SortedList` | `SortedList<TKey,TValue>`    |
| `BitArray`   | No direct generic equivalent |

---

| Collection   | Generic Alternative          | Short Explanation                                                  |
| ------------ | ---------------------------- | ------------------------------------------------------------------ |
| `ArrayList`  | `List<T>`                    | Stores items dynamically. `List<T>` is type-safe and faster.       |
| `Hashtable`  | `Dictionary<TKey, TValue>`   | Stores data as Key-Value pairs. `Dictionary` provides type safety. |
| `Queue`      | `Queue<T>`                   | Follows FIFO (First In, First Out).                                |
| `Stack`      | `Stack<T>`                   | Follows LIFO (Last In, First Out).                                 |
| `SortedList` | `SortedList<TKey, TValue>`   | Stores Key-Value pairs sorted by key.                              |
| `BitArray`   | No direct generic equivalent | Stores bits (`true`/`false`) efficiently.                          |

### Example

```csharp
using System.Collections;

ArrayList list = new ArrayList();

list.Add(10);
list.Add("Daksh");
list.Add(true);

foreach(var item in list)
{
    Console.WriteLine(item);
}
//output 
// 10
//Daksh
//True
```

---

## 2. Generic Collections

#### Definition:
#### Generic Collections are collections that store data of a specific type. They provide type safety, better performance, and avoid boxing/unboxing.

### < T > = DATA TYPE 

| Collection                           | Purpose                        |
| ------------------------------------ | ------------------------------ |
| `List<T>`                            | Dynamic array                  |
| `Dictionary<TKey, TValue>`           | Key-Value pairs                |
| `SortedDictionary<TKey, TValue>`     | Sorted key-value pairs         |
| `SortedList<TKey, TValue>`           | Sorted list of key-value pairs |
| `HashSet<T>`                         | Unique elements only           |
| `SortedSet<T>`                       | Unique sorted elements         |
| `Queue<T>`                           | FIFO (First In First Out)      |
| `Stack<T>`                           | LIFO (Last In First Out)       |
| `LinkedList<T>`                      | Doubly linked list             |
| `PriorityQueue<TElement, TPriority>` | Priority-based processing      |

| Collection                           | Short Explanation                                                 |
| ------------------------------------ | ----------------------------------------------------------------- |
| `List<T>`                            | Dynamic array that can grow or shrink at runtime.                 |
| `Dictionary<TKey, TValue>`           | Stores data as Key-Value pairs for fast lookup.                   |
| `SortedDictionary<TKey, TValue>`     | Key-Value pairs automatically sorted by key.                      |
| `SortedList<TKey, TValue>`           | Sorted Key-Value pairs stored in an indexed list.                 |
| `HashSet<T>`                         | Stores only unique values; duplicates are not allowed.            |
| `SortedSet<T>`                       | Stores unique values in sorted order.                             |
| `Queue<T>`                           | FIFO (First In, First Out) – first item added is removed first.   |
| `Stack<T>`                           | LIFO (Last In, First Out) – last item added is removed first.     |
| `LinkedList<T>`                      | Collection of nodes linked together; fast insertion and deletion. |
| `PriorityQueue<TElement, TPriority>` | Elements are processed based on priority, not insertion order.    |

### Example

```csharp

List<string> students = new List<string>();

students.Add("Daksh");
students.Add("Raj");
students.Add("Meet");

foreach(string student in students)
{
    Console.WriteLine(student);
}
```

---
| Non-Generic Collections         | Generic Collections                     |
| ------------------------------- | --------------------------------------- |
| Namespace: `System.Collections` | Namespace: `System.Collections.Generic` |
| Stores data as `object`         | Stores specific data type               |
| No Type Safety                  | Type Safe                               |
| Boxing/Unboxing Required        | No Boxing/Unboxing                      |
| Slower                          | Faster                                  |
| Example: ArrayList              | Example: List<T>                        |
---
# Boxing and Unboxing in C#

## Boxing
### Definition:
Boxing is the process of converting a value type (e.g., int, double, char, bool) into a reference type (object).

### or

Boxing is the implicit conversion of a value type to an object type. The value is copied from the stack to the heap

``` csharp
int num = 10;      // Value Type
object obj = num;  // Boxing
 ```
### What Happens?
- Memory is allocated on the Heap.
- The value is copied from the Stack to the Heap.
- obj references the boxed value.
---

# Unboxing
## Definition:
Unboxing is the process of converting a boxed object back to its original value type.

### or 

Unboxing is the explicit conversion of a boxed object back to its original value type
``` csharp
object obj = 10;  // Boxing
int num = (int)obj; // Unboxing
 ```
#### 
#### What Happens?
- CLR checks whether the object contains the correct value type.
- Value is copied from the Heap back to the Stack.


## Example 
``` csharp 
using System;

class Program
{
    static void Main()
    {
        int x = 100;

        // Boxing
        object obj = x;

        Console.WriteLine(obj);

        // Unboxing
        int y = (int)obj;

        Console.WriteLine(y);
    }
}

```
# 
# Why is Boxing/Unboxing Important?
- Boxing and unboxing have a performance cost.
- They involve memory allocation and copying.
- Frequent boxing/unboxing can slow down applications

---

# How can you avoid Boxing and Unboxing?

By using Generic Collections such as List<T> and Dictionary<TKey,TValue> instead of non-generic collections like ArrayList and Hashtable

### 👉 Boxing = Value Type → Object (Heap)
### 👉 Unboxing = Object → Value Type (Stack)

---
# List< T >

## Definition

List< T > is a generic collection used to store elements of the same data type.

### Example

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        List<string> names = new List<string>();

        names.Add("Daksh");
        names.Add("Raj");
        names.Add("Meet");

        foreach(string name in names)
        {
            Console.WriteLine(name);
        }
    }
}
```

### Common Methods

| Method     | Description         |
| ---------- | ------------------- |
| Add()      | Add Element         |
| Remove()   | Remove Element      |
| Insert()   | Insert Element      |
| Contains() | Search Element      |
| Clear()    | Remove All Elements |
| Count      | Total Elements      |

---

# Dictionary<TKey, TValue>

## Definition

A dictionary stores data in key-value pairs.

### Example

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        Dictionary<int, string> students =
            new Dictionary<int, string>();

        students.Add(101, "Daksh");
        students.Add(102, "Raj");

        Console.WriteLine(students[101]);
    }
}
```

### Output

```text
Daksh
```

---

# Queue<T>

## Definition

Queue follows FIFO (First In First Out).

### Diagram

```text
Front                     Rear

[10] [20] [30] [40]
```

### Example

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        Queue<string> queue = new Queue<string>();

        queue.Enqueue("A");
        queue.Enqueue("B");
        queue.Enqueue("C");

        Console.WriteLine(queue.Dequeue());
    }
}
```

### Output

```text
A
```

---

# Stack<T>

## Definition

Stack follows LIFO (Last In First Out).

### Diagram

```text
Top
 │
[30]
[20]
[10]
```

### Example

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        Stack<int> stack = new Stack<int>();

        stack.Push(10);
        stack.Push(20);
        stack.Push(30);

        Console.WriteLine(stack.Pop());
    }
}
```

### Output

```text
30
```

---

# Hashtable

## Definition

A hashtable stores data in key-value pairs.
Each key must be unique.
Values can be duplicated.
It belongs to the System.Collections namespace.
It stores data as object type.

### Example

```csharp
using System;
using System.Collections;

class Program
{
    static void Main()
    {
        Hashtable ht = new Hashtable();

        ht.Add(1, "Daksh");
        ht.Add(2, "Raj");

        Console.WriteLine(ht[1]);
    }
}
```

### Output

```text
Daksh
```

---

# Array vs Collection

| Feature       | Array     | Collection                |
| ------------- | --------- | ------------------------- |
| Size          | Fixed     | Dynamic                   |
| Insert/Delete | Difficult | Easy                      |
| Flexibility   | Low       | High                      |
| Type Safety   | Yes       | Yes (Generic Collections) |

---



### Difference between Hashtable and Dictionary

| Hashtable     | Dictionary |
| ------------- | ---------- |
| Non-Generic   | Generic    |
| Slower        | Faster     |
| Not Type Safe | Type Safe  |

---
# 3. Concurrent Collections (Thread-Safe)
### Concurrent collections are thread-safe collections used when multiple threads need to access and modify data simultaneously without causing race conditions.
## Namespace: System.Collections.Concurrent
| Collection                          | Short Explanation                                                              |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| `ConcurrentDictionary<TKey,TValue>` | Thread-safe Key-Value collection for multiple threads.                         |
| `ConcurrentQueue<T>`                | Thread-safe FIFO (First In, First Out) queue.                                  |
| `ConcurrentStack<T>`                | Thread-safe LIFO (Last In, First Out) stack.                                   |
| `ConcurrentBag<T>`                  | Thread-safe unordered collection; items can be added and removed in any order. |
| `BlockingCollection<T>`             | Thread-safe collection used for Producer-Consumer scenarios.                   |


# 4. Specialized Collection
### Specialized collections are designed for specific data storage needs, such as string-only data, multiple values per key, ordered key-value pairs, or optimized dictionary operations.
## Namespace: System.Collections.Specialized
| Collection            | Short Explanation                                                                  |
| --------------------- | ---------------------------------------------------------------------------------- |
| `StringCollection`    | Stores only string values.                                                         |
| `NameValueCollection` | Stores keys with one or more values.                                               |
| `StringDictionary`    | Stores string key-value pairs.                                                     |
| `HybridDictionary`    | Uses `ListDictionary` for small data and `Hashtable` for large data automatically. |
| `OrderedDictionary`   | Stores key-value pairs while maintaining insertion order.                          |


## Easy Memory Trick

| Requirement                     | Collection            |
| ------------------------------- | --------------------- |
| Only Strings                    | `StringCollection`    |
| Multiple Values per Key         | `NameValueCollection` |
| String Key-Value Data           | `StringDictionary`    |
| Small + Large Data Optimization | `HybridDictionary`    |
| Ordered Key-Value Data          | `OrderedDictionary`   |

# Summary

* Collections store multiple objects dynamically.
* Generic Collections are preferred over Non-Generic Collections.
* List<T> is the most commonly used collection.
* Queue follows FIFO.
* Stack follows LIFO.
* Dictionary stores key-value pairs efficiently.

---

# LAB - 1. Develop a menu-driven console application to manage student records using List<Student>. Implement Add, Display, Search, Update, and Delete functionalities.


## Program.cs
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    internal class Program
    {
        static List<Student> students = new List<Student>();
        static void Main(string[] args)
        {
            //List<T> list = new List<T>();
            //students.Add(new Student(1, "Alice", 20));
            //students.Add(new Student(2, "Bob", 22));
            //students.Add(new Student(3, "Charlie", 21));

            //foreach (var student in students)
            //{
            //    Console.WriteLine($"ID: {student.id}, Name: {student.name}, Age: {student.age}");
            //}
            int choice = 0;
            do
            {
                Console.WriteLine("1. Add Student");
                Console.WriteLine("2. Display Students");
                Console.WriteLine("3. Search Student");
                Console.WriteLine("4. Update Student");
                Console.WriteLine("5. Delete Student");
                Console.WriteLine("6. Exit");
                Console.Write("Enter your choice: ");
                choice = Convert.ToInt32(Console.ReadLine());

                switch (choice)
                {
                    case 1:
                        // Add Student
                        AddStudent();
                        break;
                    case 2:
                        // Display Students
                        DisplayStudents();
                        break;
                    case 3:
                        // Search Student
                        SearchStudent();
                        break;
                    case 4:
                        // Update Student
                        UpdateStudent();
                        break;
                    case 5:
                        // Delete Student
                        DeleteStudent();
                        break;
                    case 6:
                        Console.WriteLine("Exiting...");
                        return;
                    default:
                        Console.WriteLine("Invalid choice. Please try again.");
                        break;
                }
            } while (choice != 6);
        }

        // Add Student
        static void AddStudent()
        {
            Console.Write("Enter Student ID: ");
            int id = Convert.ToInt32(Console.ReadLine());
            Console.Write("Enter Student Name: ");
            string name = Console.ReadLine();
            Console.Write("Enter Student Age: ");
            int age = Convert.ToInt32(Console.ReadLine());
            students.Add(new Student(id, name, age));
            Console.WriteLine("Student added successfully.");
        }

        // Display Students
        static void DisplayStudents()
        {
            if (students.Count == 0)
            {
                Console.WriteLine("No students to display.");
                return;
            }
            foreach (var student in students)
            {
                Console.WriteLine($"ID: {student.id}, Name: {student.name}, Age: {student.age}");
            }
        }

        // Search Student
        static void SearchStudent()
        {
            Console.Write("Enter Student ID to search: ");
            int id = Convert.ToInt32(Console.ReadLine());
            var student = students.Find(s => s.id == id);
            if (student != null)
            {
                Console.WriteLine($"ID: {student.id}, Name: {student.name}, Age: {student.age}");
            }
            else
            {
                Console.WriteLine("Student not found.");
            }
        }

        // Update Student
        static void UpdateStudent()
        {
            Console.Write("Enter Student ID to update: ");
            int id = Convert.ToInt32(Console.ReadLine());
            var student = students.Find(s => s.id == id);
            if (student != null)
            {
                Console.Write("Enter new name: ");
                student.name = Console.ReadLine();
                Console.Write("Enter new age: ");
                student.age = Convert.ToInt32(Console.ReadLine());
                Console.WriteLine("Student updated successfully.");
            }
            else
            {
                Console.WriteLine("Student not found.");
            }
        }

        // Delete Student
        static void DeleteStudent()
        {
            Console.Write("Enter Student ID to delete: ");
            int id = Convert.ToInt32(Console.ReadLine());
            var student = students.Find(s => s.id == id);
            if (student != null)
            {
                students.Remove(student);
                Console.WriteLine("Student deleted successfully.");
            }
            else
            {
                Console.WriteLine("Student not found.");
            }
        }

}}
```
## Student.cs

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    internal class Student
    {
        public int id { get; set; }
        public string name { get; set; }
        public int age { get; set; }

        public Student(int id, string name, int age)
        {
            this.id = id;
            this.name = name;
            this.age = age;
        }

        
    }

}

```
---
---
# LAB- 2  Implement a Shopping Cart system using List<CartItem> that supports adding items, removing items, viewing cart details, and calculating total amount with discount.

### Program.cs
``` csharp 
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    internal class Program
    {
        static List<CartItem> cartItems = new List<CartItem>();
        static void Main(string[] args)
        {
            
            int choice = 0;
            do
            {
                Console.WriteLine("================================");
                Console.WriteLine("1. Add Cart Item");
                Console.WriteLine("2. Remove Cart Item");
                Console.WriteLine("3. View Cart Items");
                Console.WriteLine("4. Calculate Total Price");
                Console.WriteLine("5. Clear Cart");
                Console.WriteLine("6. Exit");
                Console.Write("Enter your choice: ");
                choice = Convert.ToInt32(Console.ReadLine());
                Console.WriteLine("================================");


                switch (choice)
                {
                    case 1:
                        // Add Cart Item
                        AddToCart();
                        break;
                    case 2:
                       // remove  Cart Item
                       RemoveFromCart();
                        break;
                    case 3:
                        //View Cart Item 
                        ViewCartItems();
                        break;
                    case 4:
                        // Calculate Total Price 
                        CalculateTotalPrice();
                        break;
                    case 5:
                        // Clear Cart
                        CalculateTotalPrice();
                   
                        break;
                    case 6:
                        Console.WriteLine("Exiting...");
                        return;
                    default:
                        Console.WriteLine("Invalid choice. Please try again.");
                        break;
                }
            } while (choice != 6);
        }

        static void AddToCart()
        {
            Console.Write("Enter Item ID: ");
            int id = Convert.ToInt32(Console.ReadLine());
            Console.Write("Enter Item Name: ");
            string itemName = Console.ReadLine();
            Console.Write("Enter Quantity: ");
            int quantity = Convert.ToInt32(Console.ReadLine());
            Console.Write("Enter Price: ");
            int price = Convert.ToInt32(Console.ReadLine());
            CartItem newItem = new CartItem(id, itemName, quantity, price);
            cartItems.Add(newItem);
            Console.WriteLine("Item added to cart.");
            Console.WriteLine("================================");

        }

        // Remove Cart Item
        static void RemoveFromCart()
        {
            Console.Write("Enter Item ID to remove: ");
            int id = Convert.ToInt32(Console.ReadLine());
            CartItem itemToRemove = cartItems.FirstOrDefault(item => item.id == id);
            if (itemToRemove != null)
            {
                cartItems.Remove(itemToRemove);
                Console.WriteLine("Item removed from cart.");
            }
            else
            {
                Console.WriteLine("Item not found in cart.");
            }
            Console.WriteLine("================================");

        }

        // Cart details
        static void ViewCartItems()
        {
            if (cartItems.Count == 0)
            {
                Console.WriteLine("Cart is empty.");
                return;
            }
            Console.WriteLine("Cart Items:");
            foreach (var item in cartItems)
            {
                Console.WriteLine("Id:"+ item.id);
                Console.WriteLine("Item Name: " + item.ItemName);
                Console.WriteLine("Quantity: " + item.Quantity);
                Console.WriteLine("Price: " + item.price);
            }
            Console.WriteLine("================================");

        }

        // Calculate Total Price
        static void CalculateTotalPrice()
        {

            // In Easy Way
             int totalPrice = 0;
            foreach (var item in cartItems)
            {

                totalPrice += item.Quantity * item.price;
                Console.WriteLine("Total Price: " + totalPrice);
                //int totalPrice = cartItems.Sum(item => item.Quantity * item.price);
                //Console.WriteLine("Total Price: " + totalPrice);
            }
            Console.WriteLine("================================");

        }

        // Clear Cart 
        static void ClearCart()
        {   
            cartItems.Clear();
            Console.WriteLine("Cart Celar");
            Console.WriteLine("================================");

        }
    }
}

```

### CartItem.cs

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    internal class CartItem
    {
        public int id { get; set; }
        public string ItemName { get; set; }
        public int Quantity { get; set; }

        public int price { get; set; }

        public CartItem(int id, string itemName, int quantity, int price)
        {
            this.id = id;
            this.ItemName = itemName;
            this.Quantity = quantity;
            this.price = price;
        }

    }

}
```
---
# LAB - 3 Create a Student Grade Book using Dictionary<int, Student> where each student record contains subject-wise marks. Implement features to add/update student details, calculate percentage, and display reports


### Program.cs
``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    internal class Program
    {
        static Dictionary<int, Student> students = new Dictionary<int, Student>();
        static void Main(string[] args)
        {
            int choice = 0;

            do
            {
                Console.WriteLine("\n===== Student Grade Book =====");
                Console.WriteLine("1. Add Student");
                Console.WriteLine("2. Update Student Marks");
                Console.WriteLine("3. Display Student Report");
                Console.WriteLine("4. Display All Students");
                Console.WriteLine("5. Exit");

                Console.Write("Enter Choice: ");
                choice = Convert.ToInt32(Console.ReadLine());
                switch (choice)
                {

                    case 1:
                        AddStudent();
                        break;

                    case 2:
                        UpdateStudent();
                        break;

                    case 3:
                        DisplayStudent();
                        break;

                    case 4:
                        DisplayAllStudents();
                        break;

                    case 5:
                        Console.WriteLine("Thank You!");
                        break;
                    default:
                        Console.WriteLine("Invalid Choice!");
                        break;
                }
            } while (true);
        }

        static void AddStudent()
        {
            Console.Write("Enter Student ID: ");
            int id = Convert.ToInt32(Console.ReadLine());
            if (students.ContainsKey(id))
            {
                Console.WriteLine("Student ID already exists!");
                return;
            }
            Console.Write("Enter Name: ");
            string name = Console.ReadLine();

            Console.Write("Enter Maths Marks: ");
            int maths = Convert.ToInt32(Console.ReadLine());

            Console.Write("Enter Science Marks: ");
            int science = Convert.ToInt32(Console.ReadLine());

            Console.Write("Enter English Marks: ");
            int english = Convert.ToInt32(Console.ReadLine());

            students.Add(id, new Student(id, name, maths, science, english));
            Console.WriteLine("Student Added Successfully!");
        }


        static void UpdateStudent()
        {
            Console.Write("Enter Student ID: ");
            int id = Convert.ToInt32(Console.ReadLine());
            if (students.ContainsKey(id))
            {
                Console.Write("Enter New Maths Marks: ");
                students[id].Math = Convert.ToInt32(Console.ReadLine());
                Console.Write("Enter New Physics Marks: ");
                students[id].Physics = Convert.ToInt32(Console.ReadLine());
                Console.Write("Enter New Chemistry Marks: ");
                students[id].Chemistry = Convert.ToInt32(Console.ReadLine());
                Console.WriteLine("Marks Updated Successfully!");
            }
            else
            {
                Console.WriteLine("Student Not Found!");
            }
        }
        static void DisplayStudent()
        {
            Console.Write("Enter Student ID: ");
            int id = Convert.ToInt32(Console.ReadLine());
            if (students.ContainsKey(id))
            {
                Console.WriteLine("==============================");
                Console.WriteLine($"Name: {students[id].Name}");
                Console.WriteLine($"Maths: {students[id].Math}");
                Console.WriteLine($"Physics: {students[id].Physics}");
                Console.WriteLine($"Chemistry: {students[id].Chemistry}"); 
                Console.WriteLine($"Total: {students[id].Math + students[id].Physics + students[id].Chemistry}");
                Console.WriteLine($"Average: {(students[id].Math + students[id].Physics + students[id].Chemistry) / 3}");
               
            }
            else
            {
                Console.WriteLine("Student Not Found!");
            }
        }
        static void DisplayAllStudents()
        {
            if (students.Count == 0)
            {
                Console.WriteLine("No Records Found!");
                return;
            }
            foreach (var student in students.Values)
            {
                Console.WriteLine("==============================");
                Console.WriteLine($"Name: {student.Name}");
                Console.WriteLine($"Maths: {student.Math}");
                Console.WriteLine($"Physics: {student.Physics}");
                Console.WriteLine($"Chemistry: {student.Chemistry}");

            }
        }
    }
}

```

## Student.cs
``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    internal class Student
    {
        public int Id { get; set; }

        public string Name { get; set; }

        public int Math { get; set; }

        public int Physics { get; set; }

        public int Chemistry { get; set; }

        public Student(int Id, string Name, int Math, int Physics, int Chemistry)
        {
            this.Id = Id;
            this.Name = Name;
            this.Math = Math;
            this.Physics = Physics;
            this.Chemistry = Chemistry;
        }
    }
}

```
---

# LAB - 4  Develop a Product Catalog and Inventory Management System using Dictionary<string, Product> with features for adding products, updating stock, searching, and generating bills.

``` csharp
using System;
using System.Collections.Generic;

class Product
{
    public string ProductId { get; set; }
    public string ProductName { get; set; }
    public double Price { get; set; }
    public int Stock { get; set; }

    public Product(string id, string name, double price, int stock)
    {
        ProductId = id;
        ProductName = name;
        Price = price;
        Stock = stock;
    }

    public void Display()
    {
        Console.WriteLine($"ID: {ProductId}");
        Console.WriteLine($"Name: {ProductName}");
        Console.WriteLine($"Price: {Price}");
        Console.WriteLine($"Stock: {Stock}");
        Console.WriteLine("------------------------");
    }
}

class Program
{
    static Dictionary<string, Product> products =
        new Dictionary<string, Product>();

    static void Main()
    {
        int choice;

        do
        {
            Console.WriteLine("\n===== PRODUCT INVENTORY SYSTEM =====");
            Console.WriteLine("1. Add Product");
            Console.WriteLine("2. Display Products");
            Console.WriteLine("3. Search Product");
            Console.WriteLine("4. Update Stock");
            Console.WriteLine("5. Generate Bill");
            Console.WriteLine("6. Exit");

            Console.Write("Enter Choice: ");
            choice = Convert.ToInt32(Console.ReadLine());

            switch (choice)
            {
                case 1:
                    AddProduct();
                    break;

                case 2:
                    DisplayProducts();
                    break;

                case 3:
                    SearchProduct();
                    break;

                case 4:
                    UpdateStock();
                    break;

                case 5:
                    GenerateBill();
                    break;

                case 6:
                    Console.WriteLine("Thank You!");
                    break;

                default:
                    Console.WriteLine("Invalid Choice");
                    break;
            }

        } while (choice != 6);
    }

    static void AddProduct()
    {
        Console.Write("Enter Product ID: ");
        string id = Console.ReadLine();

        if (products.ContainsKey(id))
        {
            Console.WriteLine("Product ID already exists!");
            return;
        }

        Console.Write("Enter Product Name: ");
        string name = Console.ReadLine();

        Console.Write("Enter Price: ");
        double price = Convert.ToDouble(Console.ReadLine());

        Console.Write("Enter Stock: ");
        int stock = Convert.ToInt32(Console.ReadLine());

        products.Add(id, new Product(id, name, price, stock));

        Console.WriteLine("Product Added Successfully.");
    }

    static void DisplayProducts()
    {
        if (products.Count == 0)
        {
            Console.WriteLine("No Products Available.");
            return;
        }

        foreach (var product in products.Values)
        {
            product.Display();
        }
    }

    static void SearchProduct()
    {
        Console.Write("Enter Product ID: ");
        string id = Console.ReadLine();

        if (products.ContainsKey(id))
        {
            products[id].Display();
        }
        else
        {
            Console.WriteLine("Product Not Found.");
        }
    }

    static void UpdateStock()
    {
        Console.Write("Enter Product ID: ");
        string id = Console.ReadLine();

        if (products.ContainsKey(id))
        {
            Console.Write("Enter New Stock: ");
            int stock = Convert.ToInt32(Console.ReadLine());

            products[id].Stock = stock;

            Console.WriteLine("Stock Updated Successfully.");
        }
        else
        {
            Console.WriteLine("Product Not Found.");
        }
    }

    static void GenerateBill()
    {
        Console.Write("Enter Product ID: ");
        string id = Console.ReadLine();

        if (!products.ContainsKey(id))
        {
            Console.WriteLine("Product Not Found.");
            return;
        }

        Console.Write("Enter Quantity: ");
        int qty = Convert.ToInt32(Console.ReadLine());

        Product p = products[id];

        if (qty > p.Stock)
        {
            Console.WriteLine("Insufficient Stock.");
            return;
        }

        double total = qty * p.Price;

        p.Stock -= qty;

        Console.WriteLine("\n======= BILL =======");
        Console.WriteLine($"Product : {p.ProductName}");
        Console.WriteLine($"Price   : {p.Price}");
        Console.WriteLine($"Qty     : {qty}");
        Console.WriteLine($"Total   : {total}");
        Console.WriteLine("====================");
    }
}
```

---
# LAB - 5 Implement a Role-Based Permission System using HashSet<string> to manage user permissions. Support role assignment, permission checking, and merging of permissions.



Permissions = Union of all assigned roles

``` csharp 
using System;
using System.Collections.Generic;

class Role
{
    public string RoleName { get; set; }
    public HashSet<string> Permissions { get; set; }

    public Role(string roleName)
    {
        RoleName = roleName;
        Permissions = new HashSet<string>();
    }
}

class User
{
    public string UserName { get; set; }
    public List<Role> Roles { get; set; }

    public User(string userName)
    {
        UserName = userName;
        Roles = new List<Role>();
    }

    // Merge permissions from all roles
    public HashSet<string> GetPermissions()
    {
        HashSet<string> mergedPermissions = new HashSet<string>();

        foreach (Role role in Roles)
        {
            mergedPermissions.UnionWith(role.Permissions);
        }

        return mergedPermissions;
    }

    // Check permission
    public bool HasPermission(string permission)
    {
        return GetPermissions().Contains(permission);
    }
}

class Program
{
    static Dictionary<string, Role> roles =
        new Dictionary<string, Role>();

    static Dictionary<string, User> users =
        new Dictionary<string, User>();

    static void Main()
    {
        InitializeRoles();

        int choice;

        do
        {
            Console.WriteLine("\n===== ROLE BASED PERMISSION SYSTEM =====");
            Console.WriteLine("1. Create User");
            Console.WriteLine("2. Assign Role");
            Console.WriteLine("3. Check Permission");
            Console.WriteLine("4. Display User Permissions");
            Console.WriteLine("5. Exit");

            Console.Write("Enter Choice: ");
            choice = Convert.ToInt32(Console.ReadLine());

            switch (choice)
            {
                case 1:
                    CreateUser();
                    break;

                case 2:
                    AssignRole();
                    break;

                case 3:
                    CheckPermission();
                    break;

                case 4:
                    DisplayPermissions();
                    break;

                case 5:
                    Console.WriteLine("Thank You!");
                    break;

                default:
                    Console.WriteLine("Invalid Choice!");
                    break;
            }

        } while (choice != 5);
    }

    static void InitializeRoles()
    {
        Role admin = new Role("Admin");
        admin.Permissions.Add("READ");
        admin.Permissions.Add("WRITE");
        admin.Permissions.Add("DELETE");
        admin.Permissions.Add("UPDATE");

        Role manager = new Role("Manager");
        manager.Permissions.Add("READ");
        manager.Permissions.Add("WRITE");
        manager.Permissions.Add("UPDATE");

        Role employee = new Role("Employee");
        employee.Permissions.Add("READ");

        roles.Add(admin.RoleName, admin);
        roles.Add(manager.RoleName, manager);
        roles.Add(employee.RoleName, employee);
    }

    static void CreateUser()
    {
        Console.Write("Enter Username: ");
        string username = Console.ReadLine();

        users[username] = new User(username);

        Console.WriteLine("User Created Successfully.");
    }

    static void AssignRole()
    {
        Console.Write("Enter Username: ");
        string username = Console.ReadLine();

        if (!users.ContainsKey(username))
        {
            Console.WriteLine("User Not Found.");
            return;
        }

        Console.Write("Enter Role (Admin/Manager/Employee): ");
        string roleName = Console.ReadLine();

        if (!roles.ContainsKey(roleName))
        {
            Console.WriteLine("Role Not Found.");
            return;
        }

        users[username].Roles.Add(roles[roleName]);

        Console.WriteLine("Role Assigned Successfully.");
    }

    static void CheckPermission()
    {
        Console.Write("Enter Username: ");
        string username = Console.ReadLine();

        if (!users.ContainsKey(username))
        {
            Console.WriteLine("User Not Found.");
            return;
        }

        Console.Write("Enter Permission: ");
        string permission = Console.ReadLine();

        if (users[username].HasPermission(permission))
        {
            Console.WriteLine("Permission Granted.");
        }
        else
        {
            Console.WriteLine("Permission Denied.");
        }
    }

    static void DisplayPermissions()
    {
        Console.Write("Enter Username: ");
        string username = Console.ReadLine();

        if (!users.ContainsKey(username))
        {
            Console.WriteLine("User Not Found.");
            return;
        }

        HashSet<string> permissions =
            users[username].GetPermissions();

        Console.WriteLine("\nMerged Permissions:");

        foreach (string permission in permissions)
        {
            Console.WriteLine(permission);
        }
    }
}

```

# LAB - 6  - Collection Classes

# 1. Develop a menu-driven console application to manage student records using List<Student>. Implement Add, Display, Search, Update, and Delete functionalities. 
# 2. Implement a Shopping Cart system using List<CartItem> that supports adding items, removing items, viewing cart details, and calculating total amount with discount.
# 3. Create a Student Grade Book using Dictionary<int, Student> where each student record contains subject-wise marks. Implement features to add/update student details, calculate percentage, and display reports.
# 4. Develop a Product Catalog and Inventory Management System using Dictionary<string, Product> with features for adding products, updating stock, searching, and generating bills.
# 5. Implement a Role-Based Permission System using HashSet<string> to manage user permissions. Support role assignment, permission checking, and merging of permissions.
# 6. Develop core features for a Social Networking application using HashSet<T> to manage user interests and friends. Implement mutual friends finding, interest-based suggestions, and duplicate tag removal.