---
num: "Lecture 10"
desc: "Inheritance and Polymorphism (cont.)"
ready: true
lecture_date: 2020-02-12 12:30:00.00-7:00
---

# Polymorphism (Dynamic Binding) - Read PS 15.3

* When a function is called on a base type object pointing to a subtype, then the appropriate function of the subtype is called.
	* In simpler terms, objects behave appropriately given their constructed type, even if assigned to a base class type.

## Example: Adding a `toString` function to both Person and Student

```
class Person {
public:
...
	std::string toString();
...
};

string Person::toString() {
	return "Person - Name: " + name + ", age: " + to_string(age);
}

class Student {
public:
...
	std::string toString();
...
};

string Student::toString() {
    return "Student - name: " + getName() + ", age: " +
      to_string(getAge()) + ", " + to_string(studentId);
}

// main.cpp
Person p1("Richert", 32);
cout << p.toString() << endl;

Student s1("John Doe", 21, 12345678);
cout << s.toString() << endl;

Person* p2 = new Person("Someone", 18);
cout << p2->toString() << endl;

Student* s2 = new Student("Jane Doe", 19, 23456789);
cout << s2->toString() << endl;

Person* p3 = new Student("Person Student", 21, 11111111);
cout << p3->toString() << endl;//uses Person::toString(). Not polymorphic
```

The above example uses Person's toString function instead of Student's even though p3 points to a Student object.
* Polymorphism isn't used here because you need to explicitly state which functions are polymorphic using the keyword <b>virtual.</b>

```
// add the keyword "virtual" to the toString functions
class Person {
public:
	virtual std::string toString();
...
};

class Student {
public:
	virtual std::string toString();
...
};
```

<b>Notes:</b>
* Technically, we don't need to word "virtual" in Student since the virtual property of the function is inherited as well. However, it's good style to state any virtual function as virtual in the function declaration.
	* By declaring the toString functions as virtual, the previous example will use Student's toString function.
* <b>Constructors cannot be virtual.</b>
    * They never need to be. The compiler knows exactly what object you're creating and what constructor needs to be called.
* <b>Destructors can (and most likely should) be virtual.</b>
    * If a Person pointer is pointing to a Student, we want to call the destructor for the Student as well. If not, there can be potential memory leaks.

## Example of Object slicing

```
Person p4 = Student("Person Student 2", 21, 1000000);
cout << p4.toString() << endl; // which toString is called?
```

* In this case, even though toString is virtual and a Student object was constructed, `Person::toString()` is called.
1. a Student object is constructed.
2. the assignment operator is used in the Person class. In this case, a shallow copy of all relevant Person attributes in the space allocated for a Person object is done.
3. the Student object is destructed. Only the Person object in memory (stack) is left.
* Therefore, when `p4.toString()` is called, then `Person::toString()` method is used.

## Example of Polymorphism with Objects on the Stack

```
void f1(Person& p) {
    cout << p.toString() << endl;
}
```
```
int main () {
    Student r = Student("Richert", 21, 1234567);
    cout << r.toString() << endl; // Calls Student::toString

    Person s = Student("T", 12, 1111112);
    cout << s.toString() << endl; // calls Person::toString() (object sliced)

    Person* t = new Student("R", 10, 654321);
    cout << t->toString() << endl; // calls Student::toString (polymorphic)

    f1(r); // calls Student::toString()

    f1(s); // calls Person::toString() since a person is constructed in memory

    f1(*t); // calls Student::toString() since a student is constructed on heap
}
```

# Inheritance and Memory Layout Example

* Note that when base class fields are inherited by a subclass, it's as if the base class fields are part of the subclass' fields and memory padding is done accordingly.

```
class X {
    public:
    X() { a = 10; b = 20; }
    short a;    // change this to int
    int b;      // change this to short
                // observe how class Y is memory padded
};

class Y : public X {
    public:
    Y() : X() { c = 30; d = 40; }
    short c;
    short d;
};

int main() {
    X x;
    Y y;
    X z = y;

    cout << "&x = " << &x << endl;
    cout << "&x.a = " << &x.a << endl;
    cout << "&x.b = " << &x.b << endl;
    cout << "---" << endl;
    cout << "&y = " << &y << endl;
    cout << "&y.a = " << &y.a << endl;
    cout << "&y.b = " << &y.b << endl;
    cout << "&y.c = " << &y.c << endl;
    cout << "&y.d = " << &y.d << endl;
    cout << "---" << endl;
    cout << "&z = " << &z << endl;
    cout << "&z.a = " << &z.a << endl;
    cout << "&z.b = " << &z.b << endl;

    return 0;
}
```

# Pure Virtual Functions and Abstract Classes

There are times where it doesn't make sense for a base class to have a virtual function.

## Example

```
class Shape {
public:
	virtual double area();
};
```

* How would we know how to compute the area of a general Shape? We would need to know what type of shape it is...
* We can make Shape an <b><i>abstract class</i></b> since it can contain fields that all shapes have in common and can inherit from.
* An abstract class is defined as having one or more <i><b>pure virtual functions.</b></i>
* A <b>virtual function</b> can be overridden with an inheriting class by a function with the same signature.
* A <b>pure virtual function</b> is REQUIRED to be implemented by a derived class that is not abstract.
* You cannot create objects of abstract classes (for good reqson since it wouldn't make sense to call `area()` on a general Shape).

## Example (defining a pure virtual function)

```
class Shape {
public:
	virtual double area() = 0; // pure virtual function
};
```

You can have references or pointers to abstract classes (i.e. Shape* s is legal and can point to objects of any child class).

```
class Rectangle : public Shape {
public:
	Rectangle() {}
	Rectangle(double length, double width);
	virtual double area();
private:
	double length;
	double width;
};

Rectangle::Rectangle(double length, double width) {
	this->length = length;
	this->width = width;
}

double Rectangle::area() {
	cout << "In Rectangle::area()" << endl;
	return length * width;
}

class Square : public Shape {
public:
	Square(double side);
	virtual double area();
private:
	double side;
};

Square::Square(double side) {
	this->side = side;
}

double Square::area() {
	cout << "In Square::area()" << endl;
	return side * side;
}
```

# Array of Pointers

* The following won't work since abstract classes cannot be instantiated:

```
Shape shapes[2]; // ERROR, tries to construct a shape object.
```

* But we can <i>point</i> abstract classes to sub classes that inherit and implement the pure virtual function:

```
Shape* shapes[2]; // OK

shapes[0] = new Square(10);
shapes[1] = new Rectangle(3, 4);

cout << shapes[0]->area() << endl;
cout << shapes[1]->area() << endl;
```

The correct area() function will be called on a sub class object if that object is pointed to by Shape* even if Shape does not have an area() definition.

## Example with various scenarios

```
	Rectangle* r = new Rectangle[100]; // Rectangle array on the heap
	//r[0] = new Rectangle(2,2); // ERROR, expects objects, not pointers
	r[0] = Rectangle(2,2); // OK, r[0] is an object

	//Rectangle* r1 = new Rectangle*[100]; // ERROR
	Rectangle** r1 = new Rectangle*[100]; // OK, r1 is a pointer to an array of pointers
	r1[0] = new Rectangle(2,3); // r1[0] is a pointer
	cout << r1[0]->area() << endl;

	Shape* s = new Rectangle(5,5);
	cout << s->area() << endl; // Works as expected.

	Shape* s1[100]; // Array declared on the stack
	s1[0] = new Rectangle(6,6);
	cout << s1[0]->area() << endl; // Works as expected.

	delete [] r;	// OK, deletes the array of Rectangles on heap
	delete [] r1;
	// OK, deletes array of Rectangle pointers on heap
	// Be careful, Rectangle objects that elements in r1
	// point to may still be on the heap.
	// You may need to manually delete these as well.

	delete s1[0];
	// OK, deleting a Rectangle object on the heap
	// may receive a warning if Shape does not have a virtual destructor

	//delete [] s1;	// ERROR. Trying to delete something on the stack.
```
