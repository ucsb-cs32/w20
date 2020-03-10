---
num: "Lecture 16"
desc: "Threads"
ready: true
lecture_date: 2020-03-09 12:30:00.00-7:00
---

# Threads

* A decent tutorial covering a lot of details on C++ threads: [https://thispointer.com/c-11-multithreading-part-1-three-different-ways-to-create-threads/](https://thispointer.com/c-11-multithreading-part-1-three-different-ways-to-create-threads/)

## Recall our discussion on <b>processes</b>
* A computer program in execution.
* A process contains the memory and state of the program being run on a computer.
* Threads are different than processes such that:
	* A program unit that is executed independently of other parts of your program.
	* A process may create many threads
		* An OS manages threads similar to a process, BUT
			* threads shares the memory space of the main process.
	* A C++ application starts with a single main thread.

## Threads and hardware
* For multi-core processors, you can design your program such that several parts of it work concurrently.
* Each processor (core) runs instructions in parallel.
	* For single-core architectures, it provides the illusion that something is working concurrently, but the processor is shared among all programs.
	* A a single thread can only be executed on a single core at any given time.

## Concurrency
* Concurrency is important in computer science.
	* For example, working on "parts of a problem" concurrently can improve performance if the parts can be done independently.
	* Many applications use concurrency for performance reasons, for example:
		* Web servers process requests concurrently
		* We browsers fetch / render images concurrently
	* Embarrassingly Parallel problems are usually a good fit for concurrent computing
		* Monte-carlo simulations are embarrassingly parallel – the result is the average (expected) scenario based on independent executions.

# Example of creating a thread

```
#include <iostream>
#include <thread>
#include <unistd.h>
#include <string>

using namespace std;

void callback1(string val, int num) {
	cout << "----" << endl;
	cout << "Thread with str val: " << val << endl;
	cout << "num: " << num << endl;
	cout << "Thread id: " << this_thread::get_id() << endl;
	sleep(5);
	cout << "----" << endl;
}

int main() {
	std::thread t1(callback1, "one", 1); // add args to function here
	cout << "t1 id: " << t1.get_id() << endl;
	cout << "Done." << endl;

	return 0;
}
```

* The above program terminates abnormally because when a thread is destructed, it must either be <b>joined</b> back to the parent thread, or the parent thread needs to <b>detach</b> it.
* By default, the main thread does not wait for any thread to finish execution.
* A thread may wait for another thread to terminate execution before continuing its own execution by calling <b>.join()</b> on the thread.
* A thread may also call <b>.detach</b> on a thread to allow it to run independently.
	* In the example above, main terminates before t1 finishes
		* Since main terminates and t1 is destructed from the stack, the process terminates (std::terminate) because t1 was not joined back to the main thread, or t1 was not detached from the main thread.
* The code below shows how to either join or detach the thread in order to avoid abnormal termination:

```
int main() {
	std::thread t1(callback1, "one", 1); // add args to function here
	cout << "t1 id: " << t1.get_id() << endl;

	// main thread waits for t1 to finish execution before resuming
	// its own execution
	//t1.join();
	
	// or main thread detaches t1 and does not wait for t1 to finish.
	// Since t1 is not detached, program will not terminate abnormally.
	//t1.detach();

	cout << "Done." << endl;

	return 0;
}
```

# Example of a thead creating a thread

* See if you can trace the output and understand the order of execution:

```
void callback2(bool create) {
	cout << "***" << endl;
	cout << "Starting thread: " << this_thread::get_id() << endl;
	sleep(5);

	if (create) {
		thread subt1(callback2, false);
		cout << "Creating thread: " << subt1.get_id() << endl;
		subt1.join();
	}
	cout << "Finishing thread: " << this_thread::get_id() << endl;
	cout << "***" << endl;
}

int main() {
	thread t2(callback2, true);
	cout << "main::t2 id: " << t2.get_id() << endl;
	t2.join();
	cout << "Done." << endl;
	return 0;
}
```

# Race Conditions

* Careful programming has to be taken into consideration when dealing with multiple threads
* If threads share memory with each other (for example, an object), read / write order conflicts can happen.
* A high-level scenario:
	* A multi-threaded bank software program depends on <b>atomic</b> transactions
		* Atomic transaction means either everything is executed or nothing is.
	* Imagine a joint bank account contains a $100 balance
		* Person x deposits $10 and Person y withdraws $10 at the same time.
		* Person y obtains the current balance of $100 before withdrawing $10.
		* The OS switches to Person x after Person y obtains the current balance of $100.
		* Person x reads the current balance of $100 and deposits $10 (current balance = $110)
		* The OS switches back to Person y and finishes withdrawing $10 using Person y's current balance ($100), and updates the current balance to $90.
		* Customer loses money ... :(

## Example of a Race Condition

```
# Makefile
bank:
	g++ -o main -std=c++11 main.cpp bank.cpp
```
```
// Bank.h

class Bank {
public:
	Bank();
	void deposit(double amount);
	double getBalance();
private:
	double totalBalance;
};
```
```
// Bank.cpp

#include "Bank.h"

Bank::Bank() {
	totalBalance = 0;
}

void Bank::deposit(double amount) {
	totalBalance += amount;
}

double Bank::getBalance() {
	return totalBalance;
}
```
```
#include <iostream>
#include <thread>
#include "Bank.h"

using namespace std;

void run(Bank* b) {
	for (int i = 0; i < 10000; i++) { b->deposit(10); }
}

int main() {
	Bank* bank = new Bank();
	thread t1(run, bank);
	thread t2(run, bank);
	t1.join();
	t2.join();
	cout << "---" << endl;
	cout << bank->getBalance() << endl;
	return 0;
}
```

* Expected balance is $200000 by the end of execution, but generally is not (unless we get lucky...).

## Mutex (mutual exclusion)
* a multi-threaded locking mechanism that only allows a single thread to execute code.
	* Only one thread may acquire the lock at any given time.
	* If a thread obtains a lock on a mutex, no other thread can access the code until the lock is released (unlocked).
		* One way to provide atomic transactions and avoid race conditions between threads.
		* Another common way is to use Semaphores, but we won't cover this construct in this class.

## Updated code using a mutex object

```
// Bank.h
#include <mutex>

class Bank {
public:
	Bank();
	void deposit(double amount);
	double getBalance();
private:
	double totalBalance;
	std::mutex mutex;
};
```
```
// Bank.cpp

#include "Bank.h"

Bank::Bank() {
	totalBalance = 0;
}

void Bank::deposit(double amount) {
	mutex.lock();
	totalBalance += amount;
	mutex.unlock();
}

double Bank::getBalance() {
	return totalBalance;
}
```
```
#include <iostream>
#include <thread>
#include "Bank.h"

using namespace std;

void run(Bank* b) {
	for (int i = 0; i < 10000; i++) { b->deposit(10); }
}

int main() {
	Bank* bank = new Bank();
	thread t1(run, bank);
	thread t2(run, bank);
	t1.join();
	t2.join();
	cout << "---" << endl;
	cout << bank->getBalance() << endl;
	return 0;
}
```

* Using the mutex locking object, we now consistently have an ending balance of $200000.

## Deadlock
* A situation where two (or more) threads are waiting on each other to release a resource, where each of the threads have a lock on the dependent resources.
* Execution is halted and cannot move forward in this situation.

![Deadlock](deadlock.png)
