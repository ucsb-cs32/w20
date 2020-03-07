---
num: "Lecture 14"
desc: "Unix Processes, Fork / Exec"
ready: true
lecture_date: 2020-03-02 12:30:00.00-7:00
---

## Linux Command / Process Management
* `fork()`
    * Creates a new process that is EXACTLY the same as the parent process (few exceptions like current locking / semaphore state).
        * Returns a PID to parent of child’s PID (child’s PID == 0)
* `exec()`
    * Replaces the content of the parent process with another process.
        * Ex: exec ls –l (replaces bash process with “ls –l” process)
        * When process finishes, parent process is no longer valid and everything terminates.
    * Unix commands and applications use the fork / execute process.
        * Commands such as `ls –l`, `pwd`, …
    * The PPID is usually the shell that invokes the application or command.
* `exec` command runs a commmand with `exec()` in the current process space and then terminates.
* Example

```
MacBook-Pro-38:lecture Richert$ exec ls -l
total 1736
-rw-r--r--@  1 Richert  staff      92 May 28 18:58 Makefile
-rwxr-xr-x   1 Richert  staff    4248 May 28 19:42 main
-rw-r--r--@  1 Richert  staff     108 May 28 19:41 main.cpp
-rw-r--r--   1 Richert  staff     608 May 28 19:42 main.o
-rwxr-xr-x   1 Richert  staff  866700 May 17 13:35 my_googletest
drwxr-xr-x  17 Richert  staff     578 May 28 18:55 previous_examples

[Process completed]
```

* Terminal process is terminated and cannot enter more commands.
* Must open a new terminal process
* Flow of fork / exec running the `ls -l` command:

```
1. bash (fork) - A copy of the bash shell process is made with fork()
    2. bash_copy (exec `ls -l`) - `ls -l` command / application replaces bash_copy process
        3. `ls –l` terminates. OS removes bash_copy memory space. Control is resumed to original bash shell.
```

## Example of fork with a simple C++ program

```
forkIt: forkIt.o
	${CXX} -o forkIt -std=C++11 forkIt.o
```
```
// forkIt.cpp
#include <unistd.h> // sleep(), fork(), pid_t (in sys/types.h)
#include <iostream>
#include <string>

using namespace std;

int main() {
    cout << "Before fork, " << __FILE__ << " " <<
    __LINE__ << " "  << __FUNCTION__ << endl;
    sleep(10);

    pid_t result = fork(); // child_result == 0, parent_result == PID of child

    cout << "After fork, " << __FILE__ << " " <<
    __LINE__ << " "  << __FUNCTION__ << endl;

    sleep(10);

    cout << "After sleep, " << __FILE__ << " " <<
    __LINE__ << " "  << __FUNCTION__ << endl;

    return 0;
}
```

## Example with fork / exec

```
// hello.cpp
#include <unistd.h>
#include <iostream>

using namespace std;

int main() {
    cout << "Hello World!" << endl;
    sleep(15);
    return 0;
}
```
```
g++ -o hello hello.cpp
```
```
forkExec: forkExec.o
	${CXX} -o forkExec -std=C++11 forkExec.o
```
```
// forkExec.cpp
#include <unistd.h>
#include <iostream>

using namespace std;

int main() {
    // path to some executable
    char* const HELLO_EXECUTABLE = (char*) "/Users/richert/Desktop/32lecture/hello";
    
    cout << "Before fork, " << __FILE__ << ", " << __LINE__ << " " \
    << __FUNCTION__ << endl;

    // parent receives child PID, child_result == 0
    pid_t result = fork();

    cout << "After fork, " << __FILE__ << ", " << __LINE__ << " " \
    << __FUNCTION__ << endl;

    cout << "RESULT_PID = " << result << endl;
    cout << "PID: " << getpid() << endl;
    cout << "PPID: " << getppid() << endl;

    // Following if block executed ONLY by child process
    if (result == 0) {
        cout << "---" << endl;
        cout << "RESULT_PID = " << result << endl;
        cout << "PID: " << getpid() << endl;
        cout << "PPID: " << getppid() << endl;

        int execvResult;
        char* const path[] = { HELLO_EXECUTABLE };
        execvResult = execv(HELLO_EXECUTABLE, path); //if success, run then terminate

        // THIS LINE OF CODE NEVER REACHED IN CHILD
        // (unless execv returned an error)

        perror("execv seems to have failed");
        cerr << "execvResult=" << execvResult << endl;
        exit(1);
    }

    // parent process executes this
    // wait to check if no child process exists anymore
    // 	https://linux.die.net/man/2/waitpid
    while (waitpid(result, NULL, 0)) {
        if (errno == ECHILD) { // all children of process terminated
            cout << "pid: " << getpid() << " has no children" << endl;
            break;
        }
    }
    cout << "After waiting, " << __FILE__ ", " << __LINE__ << " " \
    << __FUNCTION__ << endl;
} // Play around with ps –l between sleep to see PPID and PID
```
