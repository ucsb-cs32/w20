---
num: "Lecture 11"
desc: "Exceptions"
ready: true
lecture_date: 2020-02-19 12:30:00.00-7:00
---


# Exceptions

* <i>Exception handling</i> is a mechanism for specifying and handling error conditions that can occur.
* The code that deals with the error condition (called the <i>exception handler</i>) is said to catch the exception.

## Example

```
int main() {
	int value;
	try {
		cout << "Enter a positive number: ";
		cin >> value;

		if (value < 0)
			throw value;

		cout << "The number entered is: " << value << endl;
	} catch (int e) {
		cout << "The number entered is not positive." << endl;
	}
	
	cout << "Statement after try/catch block" << endl;
	return 0;
}
```

* Everything executes within the try block if execution is normal. Every try block must be accompanied by one or more catch blocks.
    * Only when value < 0 would we throw something in the example above.
	* Statements after throwing an exception within the try block is ignored, and code within the catch block of the exception type is executed.
	* Regardless if an exception was thrown or not, execution resumes after the try/catch block.
* We can throw any value (basic types, structs, classes, ...).
* The code within the catch block is called the <i>exception handler</i>
    * It's possible for try / catch statements to be embedded (try/catch statements in a try block and/or catch block)
    * Observe the output when running the following code:

```
int main() {
    int value;
    try {
        try {
            cout << "in try block within try block" << endl;
            throw 100;
        } catch (int d) {
            cout << "In catch block within try block: " << d << endl;
        }
        cout << "Enter a positive number: ";
        cin >> value;

        if (value < 0)
            throw value;

        cout << "The number entered is: " << value << endl;
    } catch (int e) {
        cout << "The number entered is not positive." << endl;
        try {
            cout << "In try block within catch" << endl;
            string message = "some error message";
            throw message;
        } catch (string s) {
            cout << "in catch block within catch block:" << s << endl;
        }
        cout << "exiting catch (int e) block" << endl;
    }
	
    cout << "Statement after try/catch block" << endl;
    return 0;
}
```

# Throwing / Catching Multiple Exceptions

## Example

```
class NegativeValueException {};
class EvenValueException {};

// …
if (value < 0)
	throw NegativeValueException();
if (value % 2 == 0)
	throw EvenValueException();
// …

catch(NegativeValueException e) {
	cout << "The number entered is not positive." << endl;
} catch (EvenValueException e) {
	cout << "The number entered is even." << endl;
}
```

* The exceptions are checked one-by-one until the first match occurs.
* If we don't want to (or wouldn't know how to) handle the exception where it occurred, we can "pass it up" to the caller and let them handle the exception.

## Another Example

```
class DivideByZeroException {};

// Note that the throw (DivideByZeroException) is not required
// since C++ does not support "checked" exceptions.
double divide(int numerator, int denominator) throw (DivideByZeroException) {
	if (denominator == 0)
		throw DivideByZeroException();
	return numerator / (double) denominator;
}

int main() {
	try {
		cout << divide(1,1) << endl;
		cout << divide(1,0) << endl;
		cout << divide(2,2) << endl;
	} catch (DivideByZeroException) { // variable name is optional
		cout << "Error: cannot divide by zero" << endl;
	}
}
```

* The program terminates if a thrown exception isn't caught / handled before it is passed by main().

# Inheritance and Exceptions

* We can create a hierarchy of exceptions that inherit from a base class.
* If an object is thrown and a catch block with one of its base classes is reached...
	* then the code within the base class' catch block is executed.

## Example:

```
class A {};
class B : public A {};
class C {};

int main() {
	int value;
	try {
		cout << "Enter a positive number: ";
		cin >> value;
	
		if (value < 0)
			throw B();
	} catch (C) {
		cout << "Exception of type C caught." << endl;
	} catch (A) {
		cout << "Exception of type A caught." << endl;
	} catch (B) {
		cout << "Exception of type B caught." << endl;
	}
	return 0;
}
```

Note: Exceptions are checked top-to-bottom. The compiler may provide a warning if A exists before B since an exception of type B will never get caught if thrown in this case.
