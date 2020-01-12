---
num: "Lecture 2"
desc: "STL: Vectors, Iterators, Sets, and Maps"
ready: true
lecture_date: 2020-01-08 12:30:00.00-7:00
---

# Standard Template Library

It’s really rare if a programming language provides all of the necessary tools to accomplish a task "out of the box"
* Programmers usually use provided building blocks to create something specific to fit their needs
* Programmers are also able to build additional building blocks to build upon as well

These libraries usually come standard with the language
* You don’t have to download separate components
* The libraries should be cross-platform compatible (you shouldn’t have to code differently based on running it with Windows, Mac, or Linux)

Implementing and maintaining libraries come with a cost
* Python has a dedicated organization called the Python Software Foundation
* Java was developed and maintained by Sun Microsystems, which has been bought by Oracle
* C++ isn't "owned" by anyone really

Since C++ isn’t a product of a large organization, and is kinda organized like opensource
* <http://isocpp.org/>, <http://www.open-std.org/>
* Individual c++ compilers are then implemented based on the specifications
	* g++ / clang++ for Unix
	* Visual Studio for Microsoft
	* ... there isn't any guarantee that these behave EXACTLY the same, but do for the most part based on the specifications
* In this class, we'll assume we're using the C++11 specification unless stated otherwise.

# Standard Libarary Containers
There are many implementations of containers.
* Containers are data abstractions where you can store a sequence of elements.
… and iterators...
* Iterators are a common part of these containers, which allow you to "iterate" through the components
* Depending on the container, you can even read / write from / to these elements

## std::Vector
* A vector is a sequence of objects that are conceptually stored one after the other
* Vectors are implemented with templates, so you can store one kind of object type in the vector container

```
# Makefile
CXX=g++

main: main.o
        ${CXX} -o main -std=C++11 main.o

clean:
        rm -f *.o main
```
```
// main.cpp
#include<vector>

using namespace std;

int main() {
	vector<int> v; // a vector containing int elements
return 0;
}
```
Under-the-hood, vectors are implemented using arrays and behave similar to arrays.
* Vectors can be indexed starting from 0 to size – 1
Yet they’re different than arrays…
* Vectors are dynamically-resizable
* Vectors have a size associated with it.
	*Arrays do not know their size and the programmer must be aware of it.

## Adding to a vector example
```
// main.cp
#include<vector>

using namespace std;

template <class T>
void printVector(vector<T> &v) {
	for (int i = 0; i < v.size(); i++) {
		cout << "v[" << i << "] = " << v[i] << endl;

	// range-based for loop example
	// int index = 0;
	// for (int i : v) {
	// 	cout << "v[" << index << "] = " << i << endl;
	// }

	}
}

int main() {
	vector<int> v;
	for (int i = 0; i < 5; i++) // it could be any reasonable size…
		v.push_back(i);

	printVector(v);
	return 0;
}
```
* Like arrays, if you index a vector element that is out of range, you will probably get junk data or make your program crash.
* You can also use the .at() function to access an element.
* Unlike [ ], if .at() references an element that the vector doesn’t contain, an exception is thrown (more on exceptions later).

## Example
```
cout << v.at(4) << endl;
cout << v.at(5) << endl; // EXCEPTION THROWN
cout << v1[5] << endl; // JUNK
```

Other supported operations are:
* front() – returns the first element
* back() – returns the last element
* pop_back() – delete the last element

```
cout << "v.front() = " << v.front() << endl;
cout << "v.back() = " << v.back() << endl;
v.pop_back();
printVector(v);
```

## Vector Initialization
`push_back()` is one way to create elements in a vector. Though it’s not the only way
* You can declare a vector with a size initially
* You can also initialize a vector with a size and default values.

## Example:
```
vector<int> v1(100); // initializes vector with 100 elements.
vector<int> v2(100, 1); //initializes vector with 100 elements = 1
```

## Example creating a vector on the heap with a pointer reference to the vector contents on the heap
```
vector<int>* v = new vector<int>(10,1); // vector on heap with 10 elements initialized to 1
cout << v->size() << endl; // 10
cout << sizeof(v) << endl; // 8 bytes since v is a pointer
printVector(*v);

vector<int> w;
cout << sizeof(w) << endl; // 24 since w is a vector object
```

* A vector maintains the state of a dynamic array on the heap (which is why we do not need to specify a size when initializing a vector like we do with an array).
* Regardless of the contents of the array, the size of the vector w on the stack will always be constant.
    * The vector class has fields such as a pointer to an array on the heap, values to keep track of the size and capacity, etc.
    * Therefore, all vectors constructed on the stack will have the same memory footprint on the stack.

# Iterators
* An iterator is an abstraction for a position in a collection of objects.
* Container classes in the C++ standard library support iterators.
* It’s common to think of an iterator as a pointer to an element’s position
* Though technically it’s not a pointer, but most likely uses a pointer in its implementation.
* Even though iterators are supported between different types of containers, an iterator can only be used with its own container type.

## Example
```
vector<string> v2;

v2.push_back("Hello.");
v2.push_back("My");
v2.push_back("name");
v2.push_back("is");
v2.push_back("Batman");

for (vector<string>::iterator i = v2.begin(); i < v2.end(); i++) {
	cout << *i << " "; // string value
	cout << i->size() << endl; // prints the size of the strings
}
```
In the above example, we’ve seen vector functions that deal specifically with iterators.
* begin() – returns an iterator that points to the first element
* end() – returns an iterator that points just past the last element
* ++ increments the iterator to the next element
* < compares positions of the iterator
* `*` dereferences an iterator to get the object

## Example (Showing different ways to index elements using iterators):
```
vector<string>::iterator i = v2.begin();
cout << v2[4] << endl; 		// Batman
cout << i[4] << endl;		// Batman
cout << *(i + 4) << endl;	// Batman
```

In order to erase items in the vector, there is an `erase` method that requires iterators to do this

## Example of erasing elements
```
// Removing 2nd index of the vector
// v2.erase(v2.begin() + 2); // remove "name"
// printVector(v2);

// Removing 1st and 2nd index - [1,3)
v2.erase(v2.begin() + 1, v2.begin() + 3);
printVector(v2);
```

# Sets

* A set is a collection of unique values containing no duplicates.
* Sets support iterators
	* Items in a set are in sorted order when iterating through them.

## Example

```
#include <set>

using namespaces std;

int main() {
	set<string> s;
	s.insert("Harry");
	s.insert("Hermione");
	s.insert("Ron");
	s.insert("Harry"); // duplicate (only stored once)
	s.insert("Snape");
	// print out the contents
	for (set<string>::iterator i = s.begin(); i != s.end(); i++) {
		cout << *i << endl;
	}
}
```

# Finding an element in a set

* find() returns an iterator to the item in a set if it exists.
	* Otherwise, find() returns an iterator == set.end()

## Example

```
if (s.find("Harry") != s.end()) {
	cout << "Harry exists!" << endl; // prints this
} else {
	cout << "Harry DNE" << endl;
}

if (s.find("Hagrid") != s.end()) {
	cout << "Hagrid exists!" << endl;
} else {
	cout << "Hagrid DNE" << endl;	// prints this
}
```

# Maps

* A map is an associated container containing a key / value mapping.
	* Like a set, the keys are unique.
	* Unlike a set, there is a value associated with each key.

## Example

```
#include <map>
map<int, string> students; // mapping studentIDs to studentNames

// Use bracket notation for creation
students[0] = "Richert";
students[1] = "John Doe";
students[2] = "Jane Doe";
cout << "students[1] = " << students[1] << endl;
```

## Example using find()

* Similar to a set, .find will look for a specific key and return map.end() if the key does not exist.

```
// Check if a student id exists
if (students.find(1) == students.end()) {
	cout << "Can’t find id = 1" << endl;
} else {
	cout << "Found student id = 1, Name = " << students[1] << endl;
}
```

## Example using <string, double> types

```
map<string, double> stateTaxes;

stateTaxes["CA"] = 0.88;
stateTaxes["NY"] = 1.65;

if (stateTaxes.find("OR") == stateTaxes.end()) {
	cout << "Can't find OR" << endl;
} else {
	cout << "Found state OR" << endl;
}
```

## Example between insert vs. []

* .insert will add a key / value pair to the map.
	* If the key already exists, then .insert() will not replace the existing value.
* [] will map a key to a specific value.
	* If the key already exists, then [] will replace the existing value.

```
#include <utility> // for std::pair

// ...

students.insert(pair<int, string>(2, "Chris Gaucho")); // does not replace
students[2] = "Chris Gaucho"; // replaces
cout << students[2] << endl;
```

# Erasing using iterators

* The .erase() can either erase an item in a map using an iterator location OR a specific key value.

## Example

```
// Erasing by iterator
map<int, string>::iterator p = students.find(2);
students.erase(p); // erases "Jane Doe"

// Erasing by key
students.erase(0); // erases "Richert"

// print out the entire map…
for (map<int, string>::iterator i = students.begin(); i != students.end(); i++) {
	cout << i->first << ": " << i->second << endl;
}
```

