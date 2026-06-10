# Lab No. 02 | Loops, Strings & Arrays in C #

## Objective

To understand and implement looping constructs, string manipulation methods, and array operations in C# through console applications.

---

# Why Are We Doing This Lab?

### What?

In this lab, we will work with loops (`for`, `foreach`), built-in string methods, and arrays to solve practical problems in C#.

### Why?

Loops, strings, and arrays are the backbone of almost every program. Mastering them is essential before moving into OOP, collections, or any real-world application development.

### When?

Whenever we need to process a list of values, manipulate text input, or repeat an action — which is basically every program we will ever write.

### How?

We will implement 8 tasks covering character case toggling, substring checks, array traversal, a calculator, and string character replacement — all using C# console applications.

---

# Concept Overview

## Key Concepts

- `for` and `foreach` loops
- `char` methods — `char.IsUpper()`, `char.IsLower()`, `char.ToUpper()`, `char.ToLower()`
- String methods — `Contains()`, `IndexOf()`, `LastIndexOf()`, `ToCharArray()`
- Arrays — declaration, traversal, element tracking
- `switch` statement vs `else if` ladder
- String immutability in C#

## Quick Theory

### Loops

| Loop Type | Use When |
|---|---|
| `for` | You know the number of iterations |
| `foreach` | You want to iterate over every element in a collection |
| `while` | You iterate until a condition becomes false |

### Strings in C #

Strings in C# are **immutable** — you cannot modify a character directly using `str[i] = 'x'`. You must convert to a `char[]`, modify it, then create a new string.

```text
string  →  ToCharArray()  →  char[]  →  modify  →  new string(chars)
```

### Arrays

```text
Declaration:   int[] arr = new int[5];
Access:        arr[0], arr[1], ... arr[n-1]
Length:        arr.Length
Traversal:     for loop or foreach loop
```

---

# Console Application Setup

## What Is a Console Application?

A **console application** is a program that runs in a text-based window called the console or terminal. It does not use buttons, forms, or graphical screens. Instead, it takes input from the keyboard and displays output as text.

In C#, console applications are useful for learning basic programming because they make it easy to practice input, output, conditions, loops, strings, arrays, and methods.

Example:

```text
Enter your name: Dhairya
Hello, Dhairya
```

Here, the program asks for input, reads what the user types, and then prints a response.

## Basic Console Input and Output

### `Console.WriteLine()`

`Console.WriteLine()` prints text on the console and moves the cursor to the next line.

```csharp
Console.WriteLine("Hello World");
```

Output:

```text
Hello World
```

### `Console.Write()`

`Console.Write()` prints text but keeps the cursor on the same line.

```csharp
Console.Write("Enter your name: ");
```

Output:

```text
Enter your name:
```

### `Console.ReadLine()`

`Console.ReadLine()` reads a complete line typed by the user and stores it as a string.

```csharp
Console.Write("Enter your name: ");
string name = Console.ReadLine();
Console.WriteLine("Hello, " + name);
```

If the user types `Dhairya`, the output will be:

```text
Enter your name: Dhairya
Hello, Dhairya
```

### `Console.ReadKey()`

`Console.ReadKey()` reads a single keypress from the keyboard. In this lab, we use `Console.ReadKey().KeyChar` to read one character.

```csharp
char ch = Console.ReadKey().KeyChar;
```

## Steps to Create a Console Application

1.  Create new project
<img width="1600" height="237" alt="createnewproject" src="https://github.com/user-attachments/assets/ed91d477-ef25-4211-aed0-aa5960a339f1" />

2. Select Console App
<img width="1600" height="288" alt="consoleap" src="https://github.com/user-attachments/assets/24e19836-0203-4cf7-921d-35386943d975" />

3. Configure the app name
<img width="1600" height="899" alt="configure" src="https://github.com/user-attachments/assets/63f99508-3b8a-45fb-9115-ddcee324b05e" />

4. Additional settings
<img width="1600" height="890" alt="consoleadditionalinfo" src="https://github.com/user-attachments/assets/635b5afb-952c-4ff6-bd6d-152615dc97c9" />


### Expected Project Structure

```text
Lab02
│
└── Program.cs
```

---

# Lab Tasks

---

## Task 1 — Change Case of a Single Character

### Objective

Read a character from the user and toggle its case (uppercase → lowercase, lowercase → uppercase).

### Concept Diagram

```text
Input Character
      │
      ├── IsUpper? ──► ToLower() ──► Print lowercase
      │
      ├── IsLower? ──► ToUpper() ──► Print uppercase
      │
      └── Else ──────────────────► "Not an alphabet"
```

### Implementation

```csharp
using System;

class Task1
{
    static void Main()
    {
        Console.Write("Enter a character: ");
        char ch = Console.ReadKey().KeyChar;
        Console.WriteLine();

        if (char.IsUpper(ch))
            Console.WriteLine($"Lowercase: {char.ToLower(ch)}");
        else if (char.IsLower(ch))
            Console.WriteLine($"Uppercase: {char.ToUpper(ch)}");
        else
            Console.WriteLine("Not an alphabet.");
    }
}
```

### Explanation

- `Console.ReadKey().KeyChar` reads a single keypress and extracts the character.
- `char.IsUpper()` / `char.IsLower()` check the case.
- `char.ToLower()` / `char.ToUpper()` convert the case.

### Useful Things to Remember

- Use `ReadKey()` when only one character is needed.
- Use `if`, `else if`, and `else` when the program must choose between multiple conditions.
- Always handle non-alphabet input so the program does not give a wrong result.

### Expected Output

```text
Enter a character: G
Lowercase: g
```

```text
Enter a character: m
Uppercase: M
```

---

## Task 2 — Toggle Case of Every Character in a String

### Objective

Read a full string and toggle the case of every character in it.

### Concept Diagram

```text
Input String: "HeLLo"
      │
      ├── 'H' → IsUpper → 'h'
      ├── 'e' → IsLower → 'E'
      ├── 'L' → IsUpper → 'l'
      ├── 'L' → IsUpper → 'l'
      └── 'o' → IsLower → 'O'

Output: "hEllO"
```

### Explanation

- `Console.ReadLine()` reads the full line including spaces.
- `foreach` iterates over each character in the string.
- Non-alphabet characters (spaces, digits) are kept unchanged.

### Logic Steps

1. Read the string from the user.
2. Visit every character one by one.
3. If the character is uppercase, convert it to lowercase.
4. If the character is lowercase, convert it to uppercase.
5. Keep spaces, numbers, and symbols unchanged.

### Useful Things to Remember

- `ReadLine()` is better than `ReadKey()` when input may contain more than one character.
- `foreach` is useful when every item in a string or array must be checked.
- String output can be built character by character, but strings are immutable in C#.

### Expected Output

```text
Enter a string: HeLLo WoRLd
Toggled: hEllO wOrld
```

---

## Task 3 — Check if String 2 is Contained in String 1

### Objective

Take two strings as input. Check whether the second string exists inside the first.

### Concept Diagram

```text
s1 = "programming"
s2 = "gram"

s1.Contains(s2) → searches s1 for s2

  p r o g r a m m i n g
        g r a m           ← found at index 3

Result: "gram" IS contained in "programming"
```

### Explanation

- `string.Contains()` returns `true` if the substring exists, `false` otherwise.
- Alternatively, `s1.IndexOf(s2)` returns the starting index of `s2` in `s1`, or `-1` if not found.

### Logic Steps

1. Read the first string.
2. Read the second string.
3. Search for the second string inside the first string.
4. Print whether it was found or not.

### Useful Things to Remember

- `Contains()` is used when only a yes/no answer is needed.
- `IndexOf()` is used when the position of the substring is also needed.
- String comparison is case-sensitive by default, so `Hello` and `hello` are different.

### Expected Output

```text
Enter string 1: programming
Enter string 2: gram
"gram" IS contained in "programming"
```

```text
Enter string 1: hello
Enter string 2: world
"world" is NOT contained in "hello"
```

---

## Task 4 — Find the Second Largest Element in an Array

### Objective

Read `n` integers into an array and find the second largest unique value.

### Concept Diagram

```text
Array: [ 10, 45, 23, 45, 8 ]

Pass through array tracking:
  first  = int.MinValue
  second = int.MinValue

  10 → first=10, second=MIN
  45 → first=45, second=10
  23 → first=45, second=23  (23 > second and 23 != first)
  45 → first=45, second=23  (45 == first, skip)
   8 → first=45, second=23  (8 < second, skip)

Second Largest = 23
```

### Explanation

- `int.MinValue` is used as the initial sentinel value (smallest possible integer).
- The condition `num != first` ensures duplicates of the largest are not counted as second largest.

### Logic Steps

1. Read the number of array elements.
2. Store all values in an integer array.
3. Track the largest value in `first`.
4. Track the second largest unique value in `second`.
5. Ignore duplicate copies of the largest value.

### Useful Things to Remember

- Array indexes start from `0`, not `1`.
- `arr.Length` gives the total number of elements in the array.
- A duplicate largest value should not automatically become the second largest unique value.

### Expected Output

```text
Enter number of elements: 5
arr[0]: 10
arr[1]: 45
arr[2]: 23
arr[3]: 45
arr[4]: 8
Second largest: 23
```

---

## Task 5 — Simple Calculator (else-if Ladder & Switch Case)

### Objective

Build a calculator for two numbers supporting `+`, `-`, `*`, `/` — implemented twice: once using `else if`, once using `switch`.

### Explanation

| Feature | `else if` | `switch` |
|---|---|---|
| Readability | Moderate | Cleaner for fixed values |
| Flexibility | Supports range checks | Fixed value matching only |
| `break` required | No | Yes (each case) |
| `default` | `else` block | `default` case |

### Logic Steps

1. Read the first number.
2. Read the operator.
3. Read the second number.
4. Match the operator with the correct operation.
5. Check division by zero before using `/`.
6. Print the result or an error message.

### Useful Things to Remember

- Use `double` when decimal answers are possible.
- Use `switch` when checking one variable against fixed values like `+`, `-`, `*`, and `/`.
- Division by zero must be handled separately.

### Expected Output

```text
Enter num1: 10
Enter operator: /
Enter num2: 4
Result: 2.5
```

```text
Enter num1: 5
Enter operator: *
Enter num2: 3
Result: 15
```

---

## Task 6 — Sum of All Elements in an Array

### Objective

Read `n` integers and compute their sum.

### Explanation

- `sum += arr[i]` accumulates the total as elements are entered.
- Alternatively, you can use `Array.Sum()` from LINQ: `int sum = arr.Sum();` (add `using System.Linq;`).

### Logic Steps

1. Read the number of elements.
2. Create an array of that size.
3. Read each value and store it in the array.
4. Add each value to `sum`.
5. Display the final sum.

### Useful Things to Remember

- Initialize `sum` with `0` before adding values.
- `+=` means add the value on the right to the existing value on the left.
- This task can be done while reading input, so a second loop is not always required.

### Expected Output

```text
Enter number of elements: 4
arr[0]: 5
arr[1]: 10
arr[2]: 3
arr[3]: 7
Sum of all elements: 25
```

---

## Task 7 — Count Odd and Even Numbers in an Array

### Objective

Read `n` integers and count how many are odd and how many are even.

### Concept Diagram

```text
Array: [ 1, 2, 3, 4, 5, 6 ]

  1 % 2 = 1 → Odd
  2 % 2 = 0 → Even
  3 % 2 = 1 → Odd
  4 % 2 = 0 → Even
  5 % 2 = 1 → Odd
  6 % 2 = 0 → Even

Even count: 3  |  Odd count: 3
```

### Explanation

- `num % 2 == 0` — modulus operator gives remainder. If remainder is 0, the number is even.
- `foreach` is preferred here since we don't need the index, just the values.

### Logic Steps

1. Read the array elements.
2. Set `even` and `odd` counters to `0`.
3. Check each number using `% 2`.
4. Increase the even counter for even numbers.
5. Increase the odd counter for odd numbers.
6. Display both counters.

### Useful Things to Remember

- `%` gives the remainder after division.
- If a number divided by `2` has remainder `0`, it is even.
- Counters should be initialized before the loop starts.

### Expected Output

```text
Enter number of elements: 6
arr[0]: 1
arr[1]: 2
arr[2]: 3
arr[3]: 4
arr[4]: 5
arr[5]: 6
Even count: 3
Odd count:  3
```

---

## Task 8 — Find First & Last Occurrence of a Character and Replace with 'D'

### Objective

Find the first and last position of a given character in a string, then replace both with `'D'`.

### Concept Diagram

```text
String:    b a n a n a
Index:     0 1 2 3 4 5

Find: 'a'
  IndexOf('a')     → 1  (first occurrence)
  LastIndexOf('a') → 5  (last occurrence)

Replace index 1 and 5 with 'D':

  b D n a n D   →   "bDnanD"
```

### Explanation

- `IndexOf(ch)` — returns index of first occurrence, `-1` if not found.
- `LastIndexOf(ch)` — returns index of last occurrence.
- **Strings are immutable in C#** — `str[i] = 'D'` won't compile. You must use `ToCharArray()`, modify the array, then reconstruct with `new string(chars)`.
- If the character appears only once, `first == last`, so the same index gets replaced twice — still correct.

### Logic Steps

1. Read the string.
2. Read the character to search.
3. Find the first occurrence using `IndexOf()`.
4. Find the last occurrence using `LastIndexOf()`.
5. If the character is not found, display a message.
6. Convert the string to a character array.
7. Replace the first and last positions with `D`.
8. Convert the character array back to a string.

### Useful Things to Remember

- `IndexOf()` returns `-1` when the character is not found.
- Strings cannot be changed directly using an index.
- Convert the string to `char[]` when individual characters must be modified.

### Expected Output

```text
Enter a string: banana
Enter character to find: a
First occurrence: index 1
Last occurrence:  index 5
Modified string: bDnanD
```

```text
Enter a string: hello
Enter character to find: z
Character 'z' not found in the string.
```

---

# Concepts Summary

| Task | Key Concept | Key Method / Keyword |
|---|---|---|
| 1 | Character case toggle | `char.IsUpper()`, `char.ToLower()` |
| 2 | String iteration, case toggle | `foreach`, `char.IsLower()` |
| 3 | Substring check | `string.Contains()`, `IndexOf()` |
| 4 | Second largest in array | `int.MinValue`, single-pass tracking |
| 5 | Calculator logic | `else if` ladder, `switch` |
| 6 | Array sum | `+=` accumulator |
| 7 | Odd/even count | `%` modulus operator |
| 8 | Character replace in string | `IndexOf()`, `LastIndexOf()`, `ToCharArray()` |

---

# Common Errors

| Error | Cause | Solution |
|---|---|---|
| `Property or indexer cannot be assigned` | Trying to do `str[i] = 'x'` on a string | Use `ToCharArray()`, modify, then `new string(chars)` |
| `FormatException` | Non-numeric input passed to `int.Parse()` | Use `int.TryParse()` for safer parsing |
| `IndexOutOfRangeException` | Accessing `arr[n]` when valid range is `0` to `n-1` | Check loop bounds |
| `-1` returned from `IndexOf` | Character not found | Always check `if (first == -1)` before using the index |
| Wrong second largest | Not handling duplicate values | Add `num != first` condition |
| `ReadLine()[0]` crash | User presses Enter without typing | Add null/empty check before `[0]` |

---

# Student Tasks

## section A

1. Write a program to change the case of entered character.
2. Write a program to Replace lower case characters to upper case and Vice-versa.
3. Take 2 strings from the user, and validate 2nd string is contains by 1st or not.
4. Find the second largest element from an array.
5. Write a program to create a Simple Calculator for two numbers (Addition, Multiplication, Subtraction, Division) [using elseif ladder & Switch Case]

## section B

6. Find the sum of all elements in an array.
2. Count odd and even numbers in an array.
3. Write a program which find out the first and last occurrence of a character and then replace that character with ‘D’.

---

# Extra Task

Build a **Menu-Driven Array Manager** console application:

```text
1. Input Array
2. Display Sum
3. Count Odd and Even
4. Find Second Largest
5. Exit
```

Use a `while` loop with a `switch` statement to keep the menu running until the user selects Exit.

---
