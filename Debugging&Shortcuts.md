# Visual Studio Code (VS Code) — Debugging & Keyboard Shortcuts

---

## Table of Contents

- [What is Debugging?](#what-is-debugging)
- [What is a Breakpoint?](#what-is-a-breakpoint)
- [How to Debug an ASP.NET Core Web API](#how-to-debug-an-aspnet-core-web-api)
- [Debug Toolbar](#debug-toolbar)
- [Most Useful Debugging Shortcuts](#most-useful-debugging-shortcuts)
- [Editing Shortcuts](#editing-shortcuts)
- [Navigation Shortcuts](#navigation-shortcuts)
- [Terminal Shortcuts](#terminal-shortcuts)
- [File Explorer Shortcuts](#file-explorer-shortcuts)
- [Tips for Beginners](#tips-for-beginners)

---

## What is Debugging?

Debugging means **finding and fixing errors (bugs)** in your application.

Instead of checking only the output, debugging allows you to:

- ⏸ Pause the program
- 🔍 Check variable values
- 🔁 Execute code line by line
- 🧭 Understand the flow of execution

### Example

```csharp
int a = 10;
int b = 20;

int sum = a + b;

Console.WriteLine(sum);
```

During debugging, you can pause at:

```csharp
int sum = a + b;
```

Then inspect the values in real time:

```
a = 10
b = 20
sum = 30
```

### Debugging Overview

**Before**
![Debugging Overview](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/Debugging%20Overview-Before.png)

**After**
![Debugging Overview](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/Debugging%20Overview-After.png)

---

## What is a Breakpoint?

A **Breakpoint** tells Visual Studio Code to **stop the execution** of your program at a specific line.

After the program pauses, you can:

- Check variables
- Understand program flow
- Find bugs easily

### Breakpoint Red Dot

![Breakpoint Red Dot](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/BreakPoint.png)

### How to Add a Breakpoint

- Click beside the **line number** in the editor gutter.
- **OR** press **`F9`** on the desired line.

**Example:**

```csharp
int a = 10;
int b = 20;

int sum = a + b;  // 🔴 Breakpoint set here

Console.WriteLine(sum);
```

![Breakpoint in Action](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/BreakpointIn%20Action.gif)

---

## How to Debug an ASP.NET Core Web API

### Step 1 — Open Project

Open your API project in VS Code.

### Step 2 — Open a Controller

Open any Controller file.

**Example:**

```csharp
[HttpGet]
public IActionResult Get()
{
    string message = "Hello";

    return Ok(message);
}
```

### Step 3 — Add a Breakpoint

Click beside:

```csharp
string message = "Hello";  // 🔴 Click here to add breakpoint
```

### Step 4 — Start Debugging

Press:

```
F5
```

The project will build and start the debugger.

### Step 5 — Trigger the API

Open **Swagger** or **Postman** and call the API endpoint.

The program will **pause at the breakpoint** you set.

### Step 6 — Inspect the Debug Panels

Check the following panels in VS Code:

| Panel             | Purpose                             |
| ----------------- | ----------------------------------- |
| **Variables**     | See current values of all variables |
| **Watch**         | Monitor specific expressions        |
| **Call Stack**    | Trace execution path                |
| **Debug Console** | Run expressions interactively       |

---

## Debug Toolbar

When debugging is active, the toolbar appears at the top of VS Code.

### Debug Toolbar Screenshot

![Debug Toolbar](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/DebugToolBar.png)

| Button       | Shortcut            | Description                                  |
| ------------ | ------------------- | -------------------------------------------- |
| ▶ Continue   | `F5`                | Resume execution until next breakpoint       |
| ⏭ Step Over | `F10`               | Execute current line, stay in current method |
| ⏬ Step Into | `F11`               | Enter inside the called method               |
| ⏫ Step Out  | `Shift + F11`       | Exit current method and return to caller     |
| 🔄 Restart   | `Ctrl + Shift + F5` | Restart debugging session                    |
| ⏹ Stop       | `Shift + F5`        | Stop debugging completely                    |

---

## Most Useful Debugging Shortcuts

| Shortcut            | Description                |
| ------------------- | -------------------------- |
| `F5`                | Start / Continue Debugging |
| `Shift + F5`        | Stop Debugging             |
| `Ctrl + Shift + F5` | Restart Debugging          |
| `F9`                | Toggle Breakpoint on/off   |
| `F10`               | Step Over                  |
| `F11`               | Step Into                  |
| `Shift + F11`       | Step Out                   |

---

## Editing Shortcuts

| Shortcut          | Description                          |
| ----------------- | ------------------------------------ |
| `Ctrl + C`        | Copy                                 |
| `Ctrl + V`        | Paste                                |
| `Ctrl + X`        | Cut                                  |
| `Ctrl + Z`        | Undo                                 |
| `Ctrl + Y`        | Redo                                 |
| `Ctrl + S`        | Save                                 |
| `Ctrl + A`        | Select All                           |
| `Ctrl + /`        | Comment / Uncomment Line             |
| `Alt + ↑`         | Move Line Up                         |
| `Alt + ↓`         | Move Line Down                       |
| `Shift + Alt + ↑` | Multi Cursor Upward (Multi Cursor)   |
| `Shift + Alt + ↓` | Multi Cursor Downward (Multi Cursor) |

---

## Navigation Shortcuts

| Shortcut                                     | Description                                                                  | Screenshot                                                                                       |
| -------------------------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `Ctrl + P`                                   | **Quick Open** – Open any file by name.                                      | ![Ctrl+P](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/CTRL%2BP.png)             |
| `Ctrl + G`                                   | **Go to Line** – Jump to a specific line number.                             | ![Ctrl+G](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/CTRL%2BG.png)             |
| `Ctrl + Tab`                                 | **Switch Files** – Move between open files.                                  | -                                                                                                |
| `Ctrl + Shift + F`                           | **Search in Project** – Search across all files in the project.              | ![Ctrl+Shift+F](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/CTRL%2BSHF%2BF.png) |
| `Ctrl + F`                                   | **Find** – Search text in the current file.                                  | ![Ctrl+F](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/CTRL%2BF.png)             |
| `Ctrl + H`                                   | **Find & Replace** – Find and replace text in the current file.              | ![Ctrl+H](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/CTRL%2BH.png)             |
| `Ctrl + .`                                   | **Quick Fix** – Show suggestions and auto-import missing files.              | ![Ctrl+.](https://github.com/sejalgupta001/BRFAPI_PVT/blob/main/Photos/CTRL%2B..png)             |
| `Ctrl + Click` <br> **or** <br> `Ctrl + F12` | **Go to Definition** – Open the source code of a class, method, or variable. | -                                                                                                |

## Terminal Shortcuts

| Shortcut       | Description                        |
| -------------- | ---------------------------------- |
| `` Ctrl + ` `` | Open / Toggle Terminal             |
| `Ctrl + C`     | Stop the currently running command |

---

## File Explorer Shortcuts

| Shortcut           | Description               |
| ------------------ | ------------------------- |
| `Ctrl + N`         | New File                  |
| `Ctrl + O`         | Open File                 |
| `Ctrl + W`         | Close Current File        |
| `Ctrl + Shift + N` | Open New VS Code Window   |
| `Ctrl + Shift + E` | Focus File Explorer Panel |

---

## Tips

### Use Breakpoints Instead of Console.WriteLine

Instead of writing many `Console.WriteLine()` statements to trace values, simply set a **breakpoint** and inspect all variables at once.

---

### Step Through Code with F10

Use **`F10` (Step Over)** to execute your code **line by line** and understand exactly what happens at each step.

---

### Inspect Variables

Always open the **Variables** panel during debugging to confirm whether your variables contain the expected values.

---

### Step Into Methods with F11

Use **`F11` (Step Into)** to dive inside any method call and understand how it works internally.

---

### Stop Debugging When Done

When you finish testing, always stop the debugger by pressing:

```
Shift + F5
```

This frees up resources and avoids leaving unnecessary processes running.

---
