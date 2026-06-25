

## What is a Collection?

- A **Collection** is a class that allows us to store and manage multiple objects dynamically.

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

- FIFO means First In First Out and is used by a queue.

### What is LIFO?

- LIFO means Last In First Out and is used by a stack.

---

# Types of Collections

## 1. Non-Generic Collections

Definition:
- Non-Generic Collections are collections that store data as the object type. They can store different types of data in the same collection.

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
using System;
using System.Collections;

class Program
{
    static void Main()
    {
        // Syntax
        // Collections_class Obj = new Collections();
           ArrayList         list = new ArrayList();

        // Add
        list.Add(10);
        list.Add("Daksh");
        list.Add(true);

        // Display
        Console.WriteLine("Original List:");
        foreach (var item in list)
        {
            Console.WriteLine(item);
        }

            //Original List:
            //10
            //Daksh
            //True

        // Insert
        list.Insert(1, "Rajkot");

        // Remove
        list.Remove(true);

        // Contains
        Console.WriteLine("\nContains Daksh: " + list.Contains("Daksh"));

          //Contains Daksh: True

        // IndexOf
        Console.WriteLine("Index of Daksh: " + list.IndexOf("Daksh"));

          //Index of Daksh: 1

        // Count
        Console.WriteLine("Count: " + list.Count);

          //Count: 3

        // Reverse
        list.Reverse();


        Console.WriteLine("\nAfter Reverse:");
        foreach (var item in list)
        {
            Console.WriteLine(item);
        }

        //After Reverse:
        //Daksh
        //Rajkot
        //10

        // Clear
        list.Clear();

        Console.WriteLine("\nCount after Clear: " + list.Count);

        //Count after Clear: 0
    }
}
```

---

## 2. Generic Collections

Definition:
- Generic Collections are collections that store data of a specific type. They provide type safety, better performance, and avoid boxing/unboxing.

### < T > = DATA TYPE 

| Collection                           | Purpose                        | Short Explanation                                                     |
| ------------------------------------ | ------------------------------ | --------------------------------------------------------------------- |
| `List<T>`                            | Dynamic array                  | Dynamic array that can grow or shrink at runtime.                     |
| `Dictionary<TKey, TValue>`           | Key-Value pairs                | Stores data as Key-Value pairs for fast lookup.                       |
| `SortedDictionary<TKey, TValue>`     | Sorted key-value pairs         | Key-Value pairs automatically sorted by key.                          |
| `SortedList<TKey, TValue>`           | Sorted list of key-value pairs | Sorted Key-Value pairs stored in an indexed list.                     |
| `HashSet<T>`                         | Unique elements only           | Stores only unique values; duplicates are not allowed.                |
| `SortedSet<T>`                       | Unique sorted elements         | Stores unique values in sorted order.                                 |
| `Queue<T>`                           | FIFO (First In First Out)      | First item added is removed first.                                    |
| `Stack<T>`                           | LIFO (Last In First Out)       | Last item added is removed first.                                     |
| `LinkedList<T>`                      | Doubly linked list             | Collection of nodes linked together; fast insertion and deletion.     |
| `PriorityQueue<TElement, TPriority>` | Priority-based processing      | Elements are processed based on priority rather than insertion order. |


### Example

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Create List
        // Syntax 
        //Collection_Class<DataType> Obj = new Collection<DataType>();

        List<string> students = new List<string>();

        // Add()
        students.Add("Daksh");
        students.Add("Raj");
        students.Add("Meet");
        // List: Daksh, Raj, Meet

        // Display
        foreach (string student in students)
        {
            Console.WriteLine(student);
        }
        /*
        Output:
        Daksh
        Raj
        Meet
        */

        // Count
        Console.WriteLine(students.Count);
        // Output: 3

        // Insert()
        students.Insert(1, "Jay");
        // List: Daksh, Jay, Raj, Meet

        // Contains()
        Console.WriteLine(students.Contains("Raj"));
        // Output: True

        // IndexOf()
        Console.WriteLine(students.IndexOf("Raj"));
        // Output: 2

        // Remove()
        students.Remove("Raj");
        // List: Daksh, Jay, Meet

        // RemoveAt()
        students.RemoveAt(0);
        // List: Jay, Meet

        // AddRange()
        students.AddRange(new List<string> { "Krunal", "Dev" });
        // List: Jay, Meet, Krunal, Dev

        // Sort()
        students.Sort();
        /*
        Output:
        Dev
        Jay
        Krunal
        Meet
        */

        // Reverse()
        students.Reverse();
        /*
        Output:
        Meet
        Krunal
        Jay
        Dev
        */

        // Display Final List
        foreach (string student in students)
        {
            Console.WriteLine(student);
        }

        /*
        Output:
        Meet
        Krunal
        Jay
        Dev
        */

        // Clear()
        students.Clear();

        Console.WriteLine(students.Count);
        // Output: 0
    }
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
# Type Casting - Boxing and Unboxing in C# 

<img width="1536" height="785" alt="ChatGPT Image Jun 23, 2026, 10_10_03 PM" src="https://github.com/user-attachments/assets/69c77f0e-bac5-4ba2-82b0-6ff2fed9465a" />

## Boxing
Definition:
- Boxing is the process of converting a value type (e.g., int, double, char, bool) into a reference type (object).

### or

Boxing is the implicit conversion of a value type to an object type. The value is copied from the stack to the heap

``` csharp
using System;

class Program
{
    static void Main()
    {
        int num = 10;      // Value Type
        object obj = num;  // Boxing

        Console.WriteLine(num);
        // Output: 10

        Console.WriteLine(obj);
        // Output: 10

      // How it work
       //int num = 10;      Stored in Stack
     //object obj = num;   Value copied to Heap

    }
}
 ```
### What Happens?
- Memory is allocated on the Heap.
- The value is copied from the Stack to the Heap.
- obj references the boxed value.
---

# Unboxing
Definition:
- Unboxing is the process of converting a boxed object back to its original value type.

### or 

- Unboxing is the explicit conversion of a boxed object back to its original value type
``` csharp
using System;

class Program
{
    static void Main()
    {
        int num = 10;      // Value Type

        object obj = num;  // Boxing

        int n = (int)obj;  // Unboxing

        Console.WriteLine(n);
        // Output: 10
    }
}
 ```

#### 
#### What Happens?
- CLR checks whether the object contains the correct value type.
- Value is copied from the Heap back to the Stack.


## common Example 
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

Definition

- List< T > is a generic collection used to store elements of the same data type.

### Example

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Create List
        // Collection_Class<DataType> Obj = new Collection_Class<DataType>();
 
        List<string> names = new List<string>();

        // Add()
        names.Add("Daksh");
        names.Add("Raj");
        names.Add("Meet");
        // Output: Daksh, Raj, Meet

        // Display List
        Console.WriteLine("Original List:");
        foreach (string name in names)
        {
            Console.WriteLine(name);
        }
        /*
        Output:
        Daksh
        Raj
        Meet
        */

        // Insert()
        names.Insert(1, "Jay");
        // Output: Daksh, Jay, Raj, Meet

        // AddRange()
        names.AddRange(new List<string> { "Krunal", "Dev" });
        // Output: Daksh, Jay, Raj, Meet, Krunal, Dev

        // Contains()
        Console.WriteLine(names.Contains("Raj"));
        // Output: True

        // IndexOf()
        Console.WriteLine(names.IndexOf("Meet"));
        // Output: 3

        // Count
        Console.WriteLine(names.Count);
        // Output: 6

        // Remove()
        names.Remove("Raj");
        // Output: Daksh, Jay, Meet, Krunal, Dev

        // RemoveAt()
        names.RemoveAt(0);
        // Output: Jay, Meet, Krunal, Dev

        // Sort()
        names.Sort();
        /*
        Output:
        Dev
        Jay
        Krunal
        Meet
        */

        // Reverse()
        names.Reverse();
        /*
        Output:
        Meet
        Krunal
        Jay
        Dev
        */

        // Find()
        string result = names.Find(x => x.StartsWith("J"));
        Console.WriteLine(result);
        // Output: Jay

        // Clear()
        names.Clear();

        Console.WriteLine(names.Count);
        // Output: 0
    }
}
```

### Common Methods
| Method       | Example                    | Purpose           |
| ------------ | -------------------------- | ----------------- |
| `Add()`      | `list.Add(10);`            | Add item          |
| `Insert()`   | `list.Insert(1,"Rajkot");` | Insert item       |
| `Remove()`   | `list.Remove(10);`         | Remove item       |
| `RemoveAt()` | `list.RemoveAt(0);`        | Remove by index   |
| `Contains()` | `list.Contains("Daksh")`   | Check item exists |
| `IndexOf()`  | `list.IndexOf("Daksh")`    | Find index        |
| `Count`      | `list.Count`               | Total items       |
| `Reverse()`  | `list.Reverse()`           | Reverse list      |
| `Clear()`    | `list.Clear()`             | Remove all items  |



---

# Dictionary<TKey, TValue>

Definition

- A dictionary stores data in key-value pairs.

### Example

```csharp
using System;
using System.Collections.Generic;
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Create Dictionary
        Dictionary<int, string> students =
            new Dictionary<int, string>();

        // Add()
        students.Add(101, "Daksh");
        students.Add(102, "Raj");
        students.Add(103, "Meet");
        // Output:
        // 101-Daksh
        // 102-Raj
        // 103-Meet

        // Access Value using Key
        Console.WriteLine(students[101]);
        // Output: Daksh

        // Count
        Console.WriteLine(students.Count);
        // Output: 3

        // ContainsKey()
        Console.WriteLine(students.ContainsKey(102));
        // Output: True

        // ContainsValue()
        Console.WriteLine(students.ContainsValue("Raj"));
        // Output: True

        // Update Value
        students[102] = "Jay";
        Console.WriteLine(students[102]);
        // Output: Jay

        // Remove()
        students.Remove(103);
        // Output: Removes Meet

        // TryGetValue()
        if (students.TryGetValue(101, out string name))
        {
            Console.WriteLine(name);
        }
        // Output: Daksh

        // Display All Data
        foreach (KeyValuePair<int, string> item in students)
        {
            Console.WriteLine(item.Key + " : " + item.Value);
        }

        /*
        Output:
        101 : Daksh
        102 : Jay
        */

        // Keys
        foreach (int key in students.Keys)
        {
            Console.WriteLine(key);
        }

        /*
        Output:
        101
        102
        */

        // Values
        foreach (string value in students.Values)
        {
            Console.WriteLine(value);
        }

        /*
        Output:
        Daksh
        Jay
        */

        // Clear()
        students.Clear();

        Console.WriteLine(students.Count);
        // Output: 0
    }
}
```

| Method            | Example                           | Purpose            |
| ----------------- | --------------------------------- | ------------------ |
| `Add()`           | `students.Add(101,"Daksh")`       | Add key-value pair |
| `Count`           | `students.Count`                  | Total records      |
| `ContainsKey()`   | `students.ContainsKey(101)`       | Check key exists   |
| `ContainsValue()` | `students.ContainsValue("Daksh")` | Check value exists |
| `Remove()`        | `students.Remove(101)`            | Remove record      |
| `TryGetValue()`   | `students.TryGetValue()`          | Get value safely   |
| `Keys`            | `students.Keys`                   | Get all keys       |
| `Values`          | `students.Values`                 | Get all values     |
| `Clear()`         | `students.Clear()`                | Remove all records |


---

# Queue<T>

Definition
- Queue follows FIFO (First In First Out).

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
        // Create Queue
        Queue<string> queue = new Queue<string>();

        // Enqueue()
        queue.Enqueue("A");
        queue.Enqueue("B");
        queue.Enqueue("C");
        // Queue: A, B, C

        // Count
        Console.WriteLine(queue.Count);
        // Output: 3

        // Peek()
        Console.WriteLine(queue.Peek());
        // Output: A
        // Returns first element without removing it

        // Dequeue()
        Console.WriteLine(queue.Dequeue());
        // Output: A
        // Queue: B, C

        // Contains()
        Console.WriteLine(queue.Contains("B"));
        // Output: True

        // Display Queue
        foreach (string item in queue)
        {
            Console.WriteLine(item);
        }

        /*
        Output:
        B
        C
        */

        // ToArray()
        string[] arr = queue.ToArray();

        foreach (string item in arr)
        {
            Console.WriteLine(item);
        }

        /*
        Output:
        B
        C
        */

        // Clear()
        queue.Clear();

        Console.WriteLine(queue.Count);
        // Output: 0
    }
}
```
Important Queue Methods

| Method       | Example               | Purpose           |
| ------------ | --------------------- | ----------------- |
| `Enqueue()`  | `queue.Enqueue("A")`  | Add item          |
| `Dequeue()`  | `queue.Dequeue()`     | Remove first item |
| `Peek()`     | `queue.Peek()`        | View first item   |
| `Contains()` | `queue.Contains("A")` | Check item exists |
| `Count`      | `queue.Count`         | Total items       |
| `ToArray()`  | `queue.ToArray()`     | Convert to array  |
| `Clear()`    | `queue.Clear()`       | Remove all items  |


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

using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Create Stack
        Stack<int> stack = new Stack<int>();

        // Push()
        stack.Push(10);
        stack.Push(20);
        stack.Push(30);
        // Stack: 30, 20, 10 (Top to Bottom)

        // Count
        Console.WriteLine(stack.Count);
        // Output: 3

        // Peek()
        Console.WriteLine(stack.Peek());
        // Output: 30
        // Returns top element without removing it

        // Pop()
        Console.WriteLine(stack.Pop());
        // Output: 30
        // Stack: 20, 10

        // Contains()
        Console.WriteLine(stack.Contains(20));
        // Output: True

        // Display Stack
        foreach (int item in stack)
        {
            Console.WriteLine(item);
        }

        /*
        Output:
        20
        10
        */

        // ToArray()
        int[] arr = stack.ToArray();

        foreach (int item in arr)
        {
            Console.WriteLine(item);
        }

        /*
        Output:
        20
        10
        */

        // Clear()
        stack.Clear();

        Console.WriteLine(stack.Count);
        // Output: 0
    }
}
```
Important Stack Methods
| Method       | Example              | Purpose           |
| ------------ | -------------------- | ----------------- |
| `Push()`     | `stack.Push(10)`     | Add item to top   |
| `Pop()`      | `stack.Pop()`        | Remove top item   |
| `Peek()`     | `stack.Peek()`       | View top item     |
| `Contains()` | `stack.Contains(20)` | Check item exists |
| `Count`      | `stack.Count`        | Total items       |
| `ToArray()`  | `stack.ToArray()`    | Convert to array  |
| `Clear()`    | `stack.Clear()`      | Remove all items  |


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
        // Create Hashtable
        Hashtable ht = new Hashtable();

        // Add()
        ht.Add(1, "Daksh");
        ht.Add(2, "Raj");
        ht.Add(3, "Meet");
        // Output:
        // 1-Daksh
        // 2-Raj
        // 3-Meet

        // Access Value
        Console.WriteLine(ht[1]);
        // Output: Daksh

        // Count
        Console.WriteLine(ht.Count);
        // Output: 3

        // ContainsKey()
        Console.WriteLine(ht.ContainsKey(2));
        // Output: True

        // ContainsValue()
        Console.WriteLine(ht.ContainsValue("Raj"));
        // Output: True

        // Update Value
        ht[2] = "Jay";
        Console.WriteLine(ht[2]);
        // Output: Jay

        // Remove()
        ht.Remove(3);
        // Removes key 3

        // Display Hashtable
        foreach (DictionaryEntry item in ht)
        {
            Console.WriteLine(item.Key + " : " + item.Value);
        }

        /*
        Output:
        1 : Daksh
        2 : Jay
        */

        // Keys
        foreach (var key in ht.Keys)
        {
            Console.WriteLine(key);
        }

        /*
        Output:
        1
        2
        */

        // Values
        foreach (var value in ht.Values)
        {
            Console.WriteLine(value);
        }

        /*
        Output:
        Daksh
        Jay
        */

        // Clear()
        ht.Clear();

        Console.WriteLine(ht.Count);
        // Output: 0
    }
}
```
Important Hashtable Methods
| Method            | Example                     | Purpose            |
| ----------------- | --------------------------- | ------------------ |
| `Add()`           | `ht.Add(1,"Daksh")`         | Add key-value pair |
| `Remove()`        | `ht.Remove(1)`              | Remove item        |
| `ContainsKey()`   | `ht.ContainsKey(1)`         | Check key exists   |
| `ContainsValue()` | `ht.ContainsValue("Daksh")` | Check value exists |
| `Count`           | `ht.Count`                  | Total records      |
| `Keys`            | `ht.Keys`                   | Get all keys       |
| `Values`          | `ht.Values`                 | Get all values     |
| `Clear()`         | `ht.Clear()`                | Remove all items   |


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

#### Task - 1. Develop a menu-driven console application to manage student records using List<Student>. Implement Add, Display, Search, Update, and Delete functionalities.


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
#### Task - 3 Create a Student Grade Book using Dictionary<int, Student> where each student record contains subject-wise marks. Implement features to add/update student details, calculate percentage, and display reports


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


---
#### Task - 5 Implement a Role-Based Permission System using HashSet<string> to manage user permissions. Support role assignment, permission checking, and merging of permissions.



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
#### 
#### 1. Develop a menu-driven console application to manage student records using List<Student>. Implement Add, Display, Search, Update, and Delete functionalities. 
#### 2. Implement a Shopping Cart system using List<CartItem> that supports adding items, removing items, viewing cart details, and calculating the total amount with a discount.
#### 3. Create a Student Grade Book using Dictionary<int, Student> where each student record contains subject-wise marks. Implement features to add/update student details, calculate percentages, and display reports.
#### 4. Develop a Product Catalog and Inventory Management System using Dictionary<string, Product> with features for adding products, updating stock, searching, and generating bills.
#### 5. Implement a Role-Based Permission System using HashSet<string> to manage user permissions. Support role assignment, permission checking, and merging of permissions.
#### 6. Develop core features for a Social Networking application using HashSet<T> to manage user interests and friends. Implement mutual friends finding, interest-based suggestions, and duplicate tag removal.
