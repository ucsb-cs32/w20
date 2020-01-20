---
num: "Lecture 4"
desc: "Namespaces, Structs, Padding"
ready: true
lecture_date: 2020-01-15 12:30:00.00-7:00
---

# Namespaces
* The purpose of a namespace is to avoid <b>naming collisions</b>.

A high-level scenario:
* std::string is a class in the standard library
* We've seen how we can specify the this by using `std::string` or `using namespace std`.
* But what if we are writing a sewing simulator and we want to write our own class called `string` that represents an actual string with a length.
* The C++ compiler might be confused if we don't specify between our string implementation and the standard library's string implementation.

## Example

```
// F1.h
#ifndef F1_H
#define F1_H
#include <iostream>
namespace CS_32_F1 {
	void someFunction() {
		std::cout << "F1.someFunction()" << std::endl;
	}
}
#endif
```
```
// F2.h
#ifndef F2_H
#define F2_H
#include <iostream>
namespace CS_32_F2 {
	void someFunction() {
		std::cout << "F2.someFunction()" << std::endl;
	}
}
#endif
```
```
#include "F1.h"
#include "F2.h"
int main() {
	using namespace CS_32_F1;
	someFunction(); // F1.someFunction()
	return 0;
}
```
```
# Makefile
CXX=g++

# note F1.h and F2.h are not required, but can list them
# to make sure the files exist.
main: main.o F1.h F2.h
	${CXX} -o main -std=C++11 main.o
clean:
	rm -f *.o main
```

<b>Notes:</b>
* using namespace CS_32_F1 prints “F1.someFunction()”
* using namespace CS_32_F2 prints “F2.someFunction()”
* using namespace CS_32_F1; using namespace CS_32_F2;
// ERROR - naming collision!
* using CS_32_F1::someFunction prints “F1.someFunction()”
* using CS_32_F2::someFunction prints “F2.someFunction()”

## Global Namespace
* By default, all variables and functions are defined in a global namespace if no namespace is explicitly defined.

## Appending to existing namespaces
* Namespace definitions can span multiple files (it adds on to the namespace if it already exists).
* Imagine if the standard library had to be defined in a single namespace block!!

## Example - Namespaces spanning multiple files

```
// F1.h
#ifndef F1_H
#define F1_H
#include <iostream>

namespace CS_32_F1 {
    class string {
        public:
        void someFunction();
    };
}
#endif
```
```
// F2.h
#ifndef F2_H
#define F2_H
#include <iostream>

namespace CS_32_F2 {
    class string {
        public:
        void someFunction();
    };
}
#endif
```
```
// F1.cpp
#include <iostream>
#include "F1.h"

using namespace std;

namespace CS_32_F1 {
	void string::someFunction() {
		cout << "F1.someFunction()" << endl;
	}
}
```
```
// F2.cpp
#include <iostream>
#include "F2.h"

using namespace std;

namespace CS_32_F2 {
	void string::someFunction() {
		cout << "F2.someFunction()" << endl;
	}
}
```
```
// main.cpp
#include "F1.h"
#include "F2.h"

int main() {
	CS_32_F2::string x;
	x.someFunction(); // prints “F2.someFunction()”
	return 0;
}
```
```
# Makefile

#CXX=g++

main: main.o Person.o F1.o F2.o
	${CXX} -o main -std=C++11 main.o Person.o F1.o F2.o

clean:
	rm -f *.o main
```

## Specifying something in the global namespace.
* It’s possible that local definitions have namespace conflicts if using namespace ... has a naming conflict.
* You can specify the global namespace by prepending “::” to indicate the global namespace.

## Example

```
// main.cpp
using namespace CS_32_F1;

void someFunction() {
	cout << "in some function (global namespace)" << endl;
}

int main() {
	// someFunction(); // which one?? – won’t compile.
	::someFunction(); // knows it’s someFunction in global namespace
}
```

# Structs
* Even in C, there was a way to represent complex data types called a “Structure” or struct
* A struct brings together a set of heterogeneous members, similar to a class.
* Structs consist of zero or more members of various types.
	* In C, having a struct without any members is illegal, but in C++ it’s fine.
	* The sizeof an empty struct is 1 byte... Why?
		* Because it ensures two different objects will have different addresses when dealing with pointer arithmetic
	* <b>Note:</b> No difference between class / struct except struct defaults members to public and classes default to private.
	* Once a struct is defined, you can declare variables, use them as return types, pass them in parameters, assign them to another struct of the same type... basically treat them like any type!

# Memory Organization of Classes / Structs
* Strictly typed languages enable the compiler to allocate the appropriate memory for functions and types before runtime.
* In memory, the members are stored sequentially and space is allocated based on the types it contains.
* Therefore, the size of the struct seems like it should be the summation of sizes for all of its members... but not exactly.

## Memory Padding
* The concept of leaving <b>blank space</b> when allocating memory to a struct / class in order to improve access speeds when reading data.
* Data is aligned by the types being stored (see figures below):
    * char types (1 byte) - aligns with 1-byte boundaries
    * short types (2 bytes) - aligns with 2-byte boundaries
    * int types (4 bytes) - aligns with 4-byte boundaries
    * double types (8 bytes) - aligns with 8-byte boundaries
* Time vs. space tradeoff
    * Sacrificing a little bit of memory for faster access times.
    * Depending on your application, simply ordering your members in the struct / class can save memory. May be important for applications running on hardware where memory is scarce.

## Example
```
class X {
    int a;      // 4 bytes
    short b;    // 2
    char c;     // 1
    char d;     // 1
};

int main() {
    X x;
    cout << sizeof(x) << endl;                                  // 8
    cout << "&x = " << &x << endl;                              // 0x7ffee6a167c8
    cout << "&x.a = " << &x.a << endl;                          // 0x7ffee6a167c8
    cout << "&x.b = " << &x.b << endl;                          // 0x7ffee6a167cc
    cout << "&x.c = " << reinterpret_cast<void*>(&x.c) << endl; // 0x7ffee6a167ce
    cout << "&x.d = " << reinterpret_cast<void*>(&x.d) << endl; // 0x7ffee6a167cf
}
```
Class X:

![Padding X](PaddingX.png)

```
class Y {
	char a;     // 1 byte
	int b;      // 4
	char c;     // 1
	short d;    // 2
};

int main() {
    Y y;
    cout << sizeof(y) << endl;                                  // 12
    cout << "&y = " << &y << endl;                              // 0x7ffeed3d4768
    cout << "&y.a = " << reinterpret_cast<void*>(&y.a) << endl; // 0x7ffeed3d4768
    cout << "&y.b = " << &y.b << endl;                          // 0x7ffeed3d476c
    cout << "&y.c = " << reinterpret_cast<void*>(&y.c) << endl; // 0x7ffeed3d4770
    cout << "&y.d = " << &y.d << endl;                          // 0x7ffeed3d4772
}
```
Class Y:

![Padding Y](PaddingY.png)

```
class Z {
    public:
    char a;     // 1 byte
    int b;      // 4
    char c;     // 1
    double d;   // 8
};

int main() {
    Z z;
    cout << sizeof(z) << endl;                                  // 24
    cout << "&z = " << &z << endl;                              // 0x7ffee07c1700
    cout << "&z.a = " << reinterpret_cast<void*>(&z.a) << endl; // 0x7ffee07c1700
    cout << "&z.b = " << &z.b << endl;                          // 0x7ffee07c1704
    cout << "&z.c = " << reinterpret_cast<void*>(&z.c) << endl; // 0x7ffee07c1708
    cout << "&z.d = " << &z.d << endl;                          // 0x7ffee07c1710
}
```
Class Z:

![Padding Z](PaddingZ.png)

* Note that the size of a class and how it's padded is determined by the largest member it contains. For example, a class containing one char will have a size of 1 byte (with no padding):

```
class A {
    public:
    char a;     // 1 byte
};

int main() {
    A a;
    cout << sizeof(a) << endl; // 1 byte
}
```

* For additional reference, here's a good article that explains this well:
 
 [https://www.geeksforgeeks.org/structure-member-alignment-padding-and-data-packing/](https://www.geeksforgeeks.org/structure-member-alignment-padding-and-data-packing/)