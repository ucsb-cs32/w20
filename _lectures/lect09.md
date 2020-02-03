---
num: "Lecture 9"
desc: "Testing"
ready: true
lecture_date: 2020-02-10 12:30:00.00-7:00
---

# Testing

## Complete Test
* Testing every possible path in every possible situation.
* Complete tests are infeasible!
* The best we can hope for is trying to approximate a Complete Test by testing various types of cases.
* The more rigorous testing a program can “pass”, the more confidence is gained that the program is “bulletproof” (handles every error as expected).

## Unit Testing
* Testing individual pieces (units) of a program (small and large) to ensure correct behavior.
* Good software practice is structuring your program into modular units.
* Good tests generally cover interesting cases including:
	* Normal Cases: Cases where the input is generally what we expect.
	* Boundary Cases: Cases that test the edge values of possible inputs.
	* Error Cases: Invalid cases (like passing a string instead of an int).
		* C++ catches most of these types of errors during the compilation process.
		* Other languages, such as Python, may need to rigorously check invalid input cases.

## Test Suite
* A program containing various tests confirming certain behavior.
* A good way to <i>automate</i> tests.
	* Very important for large-scale projects where other engineers may make a change that affects functionality of other existing (or new) functionality.
* We've seen testing programs in several of our lab assignments testing students' submissions throughout the quarter.

## Example: Writing our own simple program using tddFuncs, which tests a function that takes four integers and returns the largest

```
#include <iostream>
#include <string>
#include "tddFuncs.h"

using namespace std;

int biggest (int a, int b, int c, int d) {
	int biggest = 0;
	if (a >= b && a >= c && a >= d)
		return a;
	if (b >= a && b >= c && b >= d)
		return b;
	if (c >= a && c >= b && c >= d)
		return c;
	return d;
}

int isPositive(int a) {
	return a >= 0;
}

int main() {
	ASSERT_EQUALS(4, biggest(1,2,3,4));
	ASSERT_EQUALS(4, biggest(1,2,4,3));
	ASSERT_EQUALS(4, biggest(1,4,2,3));
	ASSERT_EQUALS(4, biggest(4,1,2,3));

	ASSERT_EQUALS(4, biggest(4,4,4,4));
	ASSERT_EQUALS(-1, biggest(-1,-2,-3,-4));
	ASSERT_EQUALS(0, biggest(-1,0,-3,-4));

	ASSERT_TRUE(isPositive(1));
	ASSERT_TRUE(isPositive(2));
	ASSERT_TRUE(isPositive(0));
	ASSERT_TRUE(!isPositive(-1));
	ASSERT_TRUE(!isPositive(-20));

	return 0;
}
```

## Test-Driven Development
* Write test cases that describe what the intended behavior of a unit of software should BEFORE implementing the functionality.
	* Defines the requirements of your piece of software.
* Implement the details of the functionality with the intention of passing the tests.
* Repeat until the tests pass.
* Imagine large software products where dozens of engineers are trying to add new features / implement optimizations all at the same time.
	* Having a “suite” of tests before deploying software to the public is essential.
	* Someone may modify changes that work for a current version, but breaks functionality in another version
	* Rigorous tests enable confidence in the stability in software.
