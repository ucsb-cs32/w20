---
num: "Lecture 8"
desc: "Midterm 1 Review"
ready: true
lecture_date: 2020-02-03 12:30:00.00-7:00
---

```
Logistics
- Bring studentID and writing utensil
- No electronic devices
- No notes, and no books

Format
- Will be a mix of questions based on lectures, homeworks,
readings, and labs
- Short answer
    - briefly define, describe, state, ...
- Write code (including Makefiles)
- Fill in the blank / complete tables / ...
- Given code, write the output
- Given some statement, tell me if it's T/F
    - If False, briefly explain why.
- Expect the exam to take ~ 1 hour
    - but we have the entire class lectures
- Will cover a broad range of topics, but probably not everything

Topics
- Will cover everything up to last Thursday's lecture (1/30)

Makefiles
- Know how to create a Makefile when multiple files are
linked to form an executable
- Know the build process
    - Preprocessor, Compilation, Linker
- Creating and using variables in Makefiles
- Know special characters like $@ and $^
- Know the default rules, flags for compilation, ...

Standard Template Library 
- Reviewed some details about vectors
- Operators like [], .at, front, back, push_back, size, constructors, ...
- Iterators
    - std::vectors, map, set (unordered_map, unordered_set)
    - .begin(), .end(), ++, <, *, !=, ...
- Using iterators

Class Design
- Abstract Data Types
- How to design an interface (.h) and its implementation (.cpp)
- public vs. private
- Accessors (getters) vs. Mutators (setters)
- Constructors (default, overloaded, copy)
- Header guards
- Scope Resolution Operator (::) and .cpp method definitions
- Shallow vs. Deep copy
- Big Three: Copy constructor, destructor, assignment operator

Structs and Classes
- What are the difference(s)
- Memory padding (how are things stored in memory, what's the size,
what's the benefit)

Namespaces
- Avoid naming collisions
- How to create namespaces
- How to use namespaces
- Global namespace

Quadratic Sorting Algorithms
- Bubblesort (optimized)
- selectionsort
- insertionsort
- Know the algorithms discussed in lecture
- Know the running times in various cases

Hash Table Topics
- Know the basics of hash functions and performance of various
operations
- open-address hashing / double-hashing / chained-hashing
- std::map vs std::unordered_map (and sets)
    - Understand underlying data structures being used for each
    - Note, that hash tables are used when implementing sets, but
    sets do not have a corresponding value for each key
- Performance of various operations

Mergesort / Quicksort
- Know the algorithms and how to implement it
- Understand main ideas and how the array is manipulated as the
algorithm executes
- Know the runtime analysis (best / avg / worst) and space
requirements
```