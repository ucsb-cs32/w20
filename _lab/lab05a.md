---
layout: lab
num: lab05a
ready: true
desc: "Hash Table Sort Testing"
assigned: 2020-02-13 08:00:00.00-7
due: 2020-02-19 23:59:00.00-7
---

# Goals

<b>PLEASE READ THE INSTRUCTIONS CAREFULLY</b>

The purpose of this lab is to expand your Hash Table implementation from Lab04 where you can read a string, parse the string into words that are inserted into your hash table, and sort the values within your hash table.

There are three new functions you must implement for this lab. Methods from Lab04 will be used to test Lab05, so be sure your code is implemented correctly and you have a correct solution submitted for Lab04b.

Similar to Lab04, this lab is different than other labs since no test cases are provided. Your job is to test your code using our tddFuncs (or some other testing framework if you wish). Be sure to think of test cases to ensure your code works for various cases. Gradescope will test your code with hidden test cases. Your Gradescope submission will show if your code compiles correctly, but will not show the test results or score since they will be hidden until after the deadline has passed.

# Step by Step 

## Step 0: Getting Started

* This lab <b>must be done solo</b>. Pair programming <b>is not</b> allowed for this lab.
* There will be <b>NO</b> opportunity for late submissions due to making tests visible after the deadline has passed. The submission window will be closed on the deadline and we will not accept any submissions for this part post-deadline.
* I <b>STRONGLY</b> encourage you to start early and not wait until the weekend to work on this lab. By starting early, you can seek assistance and guidance from our TAs, mentors, or instructor during our office / open lab hours.

## Step 1: Copying some programs from my directory 

Visit the following web link—you may want to use "right click" (or "control-click" on Mac) to bring up a window where you can open this in a new window or tab:

<http://cs.ucsb.edu/~richert/cs32/misc/lab05a/>

You should see a listing of several C++ files. We are going to copy those into your<tt>~/{{site.course | downcase}}/{{page.num}}</tt> directory all at once with the following command:

<div>
<tt>cp ~richert/public_html/cs32/misc/lab05a/* ~/cs32/lab05a</tt>
</div>

Note: If you get the error message:

<div>
<tt>cp: target '/cs/student/youruserid/{{site.course | downcase}}/{{page.num}}' is not a directory</tt>
</div>

then it probably means you didn't create a <tt>~/{{site.course | downcase}}/{{page.num}}</tt> directory yet. So do that first.

The `*` symbol in this command is a "wildcard"—it means that we want all of the files from the source directory copy be copied into the destination directory namely <tt>~/{{site.course | downcase}}/{{page.num}}</tt>.

After doing this command, if you `cd` into <tt>~/{{site.course | downcase}}/{{page.num}}</tt> and use the `ls` command, you should see several files in your <tt>~/{{site.course | downcase}}/{{page.num}}</tt> directory&mdash;the same ones that you see if you visit the link <http://cs.ucsb.edu/~richert/cs32/misc/lab05a/>

If you don't see those files, go back through the instructions and make sure you didn't miss a step. If you still have trouble, ask your TA or mentor for assistance.

## Step 2 Getting the code to pass the tests

In this week's lab, you have the following files:

* `WordCount.cpp` // updated version
* `WordCount.h` // updated version
* `tddFuncs.cpp`
* `tddFuncs.h`

Your job is to modify `WordCount.cpp` based on the specifications and thoroughly test your code. You can replace the stubs with your solution from Lab04 where necessary. Do not modify `WordCount.h` - the given array of vector structure must be used to implement your hash table. No Makefile or test applications are provided for this lab and you must create your own. `tddFuncs.*` are provided for testing your code. Even though Gradescope will not run your tests, you must submit your test file(s) to Gradescope.

## Some notes about the functions you will be implementing

* There are three new functions you must implement in WordCount.cpp for this week's lab:

	* `void dumpWordsSortedByWord(std::ostream &out) const;`
	* `void dumpWordsSortedByOccurence(std::ostream &out) const;`
	* `void addAllWords(std::string text);`

* The specification for each of these methods are commented in WordCount.h. You may implement your solution with any standard C++11 library you see fit. You may only modify `WordCount.cpp`, and `WordCount.h` may not be modified.

* Be sure to carefully read the comments in `WordCount.h`. Write your tests to make sure your functionality abides by the specification.

## Step 3: Submitting via Gradescope

The lab assignment "Lab05a" should appear in your Gradescope dashboard in CMPSC 32. If you haven't submitted anything for this assignment yet, Gradescope will prompt you to upload your files.

You will submit your `WordCount.cpp` implementation along with a `lab05Test.cpp` file containing your test application(s) (if you wrote multiple test applications, you can name each one with `lab05Test01.cpp`, `lab05Test02.cpp`, etc.). For this lab, you are <b>required</b> to submit your files with your github repo.

As mentioned earlier, you will not know Gradescope's score until AFTER the deadline has passed. When submitting your file, if you pass the compilation checks:

```
Checking stdout from make -B -f Makefile.check (1.0/1.0)
Checking stderr from make -B -f Makefile.check (1.0/1.0)
```

then that means your code compiled correctly and the test executables have been generated. If Gradescope doesn't pass this test, then your code did not compile correctly and you must fix the issues before resubmitting.

This lab (Lab05a) is worth a total of 50 points. You will have an opportunity to fix your code and pass all tests if you had any tests fail for this lab next week (Lab05b). Lab05b will be worth 50 points (100 points total for both Lab05 parts). If you passed all tests this week, all you need to do is resubmit your code to get all the points next week.

