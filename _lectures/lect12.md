---
num: "Lecture 12"
desc: "Function Pointers"
ready: true
lecture_date: 2020-02-24 12:30:00.00-7:00
---

# Function Pointers

* Consider the <algorithm> library, which has a "std::transform" function that can iterate through a container and apply a conversion function to each element.
* For example, in Lab04 we wanted to convert a string to all upper case characters.
	* There are many ways to do this...
	* And since strings are iterable, we can apply a function to each item (i.e. char) in the string.

## Example (using `std::transform`)

```
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

int main() {

	string s = "abcd";
	cout << s << endl;
	transform(s.begin(), s.end(), s.begin(), ::toupper);
	cout << s << endl;
}
// Output:
// abcd
// ABCD
```

* First thing to observe is what the transform function does:
	* 1st parameter: The starting position of where to iterate through.
	* 2nd parameter: The end position where to stop iterating.
	* 3rd parameter: The start position of where to replace content in the container.
	* 4th parameter: The function to apply on each element when iterating.

```
transform(s.begin() + 2, s.end(), s.begin(), ::toupper);
// Output: 
// abcd
// CDcd
```

* The second thing to observe is the `toupper` function.
	* Takes in a single char in its parameter, which is what the iterator of a string is referencing.
	* If `toupper` took in a type different than a char (or had additional required parameters), then this would not work.
	* `::toupper` is required in this case since `toupper` exists in the `std` namespace.
		* Using `::toupper` looks for the toupper function in the global namespace, which is what `transform` is expecting.

## Another example defining our own function for the transform function

```
// Our defined function
int multiplyBy100(int x) { return 100 * x; }

// Function to print contents of the vector
void printVector(vector<int> &v) {
	for (int i = 0; i < v.size(); i++) 
		cout << "v[" << i << "] = " << v[i] << endl;
	cout << "---" << endl;
}

int main() {
	vector<int> v;
	for (int i = 10; i <= 15; i++) {
		v.push_back(i);
	}
	printVector(v);
	transform(v.begin(), v.end(), v.begin(), multiplyBy100); // uses our function
	printVector(v);
	return 0;
}
```

* So we are passing functions in parameters and calling them.
	* <b>callbacks</b> is a term referring to passing an executable segment of code to another segment of code.
	* Under-the-hood, functions are stored in the executable and can be located in a specific memory address.
	* We can find the memory address of a function by using `reinterpret_cast`

```
cout << multiplyBy100 << endl; // warning: prints 1 (but does compile correctly)
cout << reinterpret_cast<void*>(multiplyBy100) << endl; // prints an address of where the multiplyBy100 exists in memory
```

* We can start defining our own <b>pointers to functions</b> and assign these pointers to functions' memory address.

## Example defining function pointers

```
int main() {
	// Pointer to a function that has an int parameter and returns an int.
	int (*someFunctionPointer)(int); 

	// Assign the address of multiplyBy100 to the function pointer.
	someFunctionPointer = multiplyBy100;

	// ERROR!!, trying to assign an int to a function pointer
	// someFunctionPointer = multiplyBy100(10); 

	// Using the function pointer to call a function.
	// Dereferences the pointer as a function, and then passes the parameter
	// 2 to the function.
	cout << (*someFunctionPointer)(2) << endl;

	// syntactically OK to use the function pointer directly without
	// dereferencing it
	cout << someFunctionPointer(2) << endl; // OK

	// ERROR!! Order of operations dereferences after attempting to call the
	// function
	// cout << *someFunctionPointer(2) << endl;
}
```

## Passing a function to another function as a parameter

* Similar to the transform function – a function can take a function pointer as one of its parameters and use whatever function the function pointer is referring to.

## Example: Writing a template filter function for a vector

```
template <class T>
void printVector(vector<T> &v) {
	for (int i = 0; i < v.size(); i++) 
		cout << "v[" << i << "] = " << v[i] << endl;
	cout << "---" << endl;
}

template <class T>
bool notEqual(T x, T y) {
	return x != y;
}

template<class T>
vector<T> filterVector(vector<T> v, T value, bool (*filterFunc)(T, T)) {
	vector<T> result;

	for (int i = 0; i < v.size(); i++) {
		if (filterFunc(v[i], value)) {
			result.push_back(v[i]);
		}
	}
	return result;
}

int main() {
	// vector of strings
	vector<string> v1;
	v1.push_back("s1");
	v1.push_back("s2");
	v1.push_back("s3");
	v1.push_back("s2");
	v1.push_back("s1");
	string remove = "s2";
	vector<string> result1 = filterVector(v1, remove, notEqual);
	printVector(result1);

	// vector of ints
	vector<int> v2;
	v2.push_back(1);
	v2.push_back(2);
	v2.push_back(3);
	v2.push_back(2);
	v2.push_back(1);
	vector<int> result2 = filterVector(v2, 1, notEqual);
	printVector(result2);
	return 0;
}
```
