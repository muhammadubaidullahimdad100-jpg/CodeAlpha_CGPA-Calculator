# 🎓 CGPA Calculator — CodeAlpha Internship Task 3

A console-based **CGPA Calculator** built in C++ that takes course details as input, calculates semester GPA, displays a formatted result, and saves it to a text file.

---

## 📌 Task Requirements

- Take input for the number of courses taken by the student
- For each course, input the grade and the credit hours
- Calculate the total credits and total grade points (grade × credit hours)
- Compute the GPA for the semester and then the overall CGPA
- Display individual course grades and the final CGPA to the user

---

## ⚙️ Concepts Used

| Concept | Where Applied |
|---|---|
| **Aggregation** | `GPA` class holds a pointer to externally created `Course` objects |
| **Dynamic Memory Allocation** | `Course` array allocated on heap using `new` at runtime |
| **Exception Handling** | Input validation, division-by-zero guard, file error handling |
| **File Handling** | Results saved to `GPA_Result.txt` using `ofstream` |

---

## 🏗️ Class Design

### `Course` (Data Class)
A simple data-holding class that stores three fields per course:
- `Credit_Hours` — number of credit hours
- `Grade` — letter grade entered by user (e.g., A+, B-, C)
- `Grade_Points` — numeric grade point converted from the letter grade

### `GPA` (Logic Class)
Handles all the heavy lifting — input, calculation, display, and file saving.

```
GPA "has-a" Course  →  This is Aggregation
```

The `GPA` class receives a pointer to an externally created `Course` array. It does **not** create or destroy the array — ownership stays in `main()`. This is the defining characteristic of **aggregation** over composition.

---

## 🧠 My Logic / Approach

1. Ask the user how many courses they have
2. Dynamically allocate a `Course` array on the heap (`new Course[n]`)
3. Pass the array into the `GPA` class — establishing the aggregation relationship
4. Loop through each course and collect credit hours and grade via `Input()`
5. Convert the letter grade to grade points using `Grade_to_Points()`
6. Calculate GPA as: `Total Weighted Grade Points ÷ Total Credit Hours`
7. Display results to console via `Display()`
8. Save results to `GPA_Result.txt` via `Save_To_File()`
9. Free heap memory using `delete[]`

---

## 🔒 Exception Handling

Three layers of exception handling are used throughout the program:

**Credit Hours Validation**
- If the user enters a non-numeric value, `cin.fail()` triggers an `invalid_argument` exception
- If the value is zero or negative, another `invalid_argument` is thrown
- `cin.clear()` and `cin.ignore()` reset and flush the input buffer before retrying

**Grade Validation**
- `Grade_to_Points()` throws `invalid_argument` if the entered grade doesn't match any valid option
- The calling loop catches it and re-prompts the user with a list of valid grades

**Division by Zero Guard**
- `GPA_Calculation()` throws `runtime_error` if total credit hours equals zero

**File Error Guard**
- `Save_To_File()` throws `runtime_error` if the output file cannot be created

**Memory Allocation Failure**
- `main()` separately catches `bad_alloc` in case `new` fails due to insufficient heap memory

---

## 💾 Dynamic Memory Allocation

```cpp
courses = new Course[n];   // Heap allocation — size known only at runtime
// ... work done ...
delete[] courses;           // Memory freed — prevents memory leaks
```

A static array like `Course courses[5]` can't be used here because the size `n` is only known after the user provides input at runtime. `delete[]` is called in all exit paths — including inside `catch` blocks — to ensure no memory leaks occur on error.

---

## 📁 File Handling

Results are written to `GPA_Result.txt` using `ofstream`:

```cpp
ofstream file("GPA_Result.txt");
if (!file)
    throw runtime_error("File could not be created.");
// ... write data ...
file.close();
```

The file contains a full formatted report including per-course details, total credit hours, total grade points, and the final GPA.

---

## 🔡 Supported Grades

| Grade | Points |
|-------|--------|
| A+ / A | 4.0 |
| A- | 3.7 |
| B+ | 3.3 |
| B | 3.0 |
| B- | 2.7 |
| C+ | 2.3 |
| C | 2.0 |
| D | 1.0 |
| F | 0.0 |

> Grades are case-insensitive (e.g., `b+` and `B+` are both accepted).

---

## 🖥️ How to Run

```bash
# Compile
g++ -o cgpa_calculator main.cpp

# Run
./cgpa_calculator
```

Output will be displayed in the console and also saved to `GPA_Result.txt` in the same directory.

---

## 📂 Output File Sample

```
==========================================
              GPA CALCULATOR
==========================================

Course Details
------------------------------------------

Course: 1
Credit Hours: 3
Grade: A
Grade Point: 4.00

Course: 2
Credit Hours: 2
Grade: B+
Grade Point: 3.30

==========================================
                 Result
==========================================
Total Credit Hours: 5
Total Grade Points: 18.60
Semester GPA: 3.72
Total CGPA: 3.72
```

---

## 🛠️ Tech Stack

- **Language:** C++
- **Compiler:** G++ (C++11 or later)
- **Libraries:** `iostream`, `fstream`, `stdexcept`, `iomanip`, `limits`, `string`

---

*Submitted as part of the **CodeAlpha C++ Programming Internship** — Task 3*
