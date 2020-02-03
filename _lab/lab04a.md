---
layout: lab
num: lab04a
ready: true
desc: "Hash Table Testing"
assigned: 2020-02-06 08:00:00.00-7
due: 2020-02-12 23:59:00.00-7
---

# Goals

<b>PLEASE READ THE INSTRUCTIONS CAREFULLY</b>

The purpose of this lab is to implement a simple version of a hash table using vectors to chain collisions in an array. Specifically, the hash table logic is based on counting occurences of unique words.

The hash key is a `std::string` and the value is an `int` variable representing the number of times that word has been added to the hash table. The size of the hash table and the hash function is given - your goal is to complete the WordCount.cpp functions following the specifications.

This lab is different than previous labs since no test cases are provided. Your job is to test your code using our tddFuncs (or some other testing framework if you wish). Be sure to think of test cases to ensure your code works for various cases. Gradescope will test your code with hidden test cases. Your Gradescope submission will show if your code compiles correctly, but you will not know the test results or score since they will be hidden until after the deadline has passed.

# Step by Step 

## Step 0: Getting Started

* This lab <b>must be done solo</b>. Pair programming <b>is not</b> allowed for this lab.
* There will be <b>NO</b> opportunity for late submissions due to making tests visible after the deadline has passed. The submission window will be closed on the deadline and we will not accept any submissions for this part post-deadline.
* I <b>STRONGLY</b> encourage you to start early and not wait until the deadline is near. By starting early, you can seek assistance and guidance from our TAs, tutors, or instructor during lab sections and our office / open lab hours.

## Step 1: Copying some programs from my directory 

Visit the following web link—you may want to use "right click" (or "control-click" on Mac) to bring up a window where you can open this in a new window or tab:

<http://cs.ucsb.edu/~richert/cs32/misc/lab04a/>

You should see a listing of several C++ files. We are going to copy those into your<tt>~/{{site.course | downcase}}/{{page.num}}</tt> github repo all at once with the following command:

<div>
<tt>cp ~richert/public_html/cs32/misc/lab04a/* ~/cs32/lab04a</tt>
</div>

Note: If you get the error message:

<div>
<tt>cp: target '/cs/student/youruserid/{{site.course | downcase}}/{{page.num}}' is not a directory</tt>
</div>

then it probably means you didn't create a <tt>~/{{site.course | downcase}}/{{page.num}}</tt> directory yet. So do that first.

The `*` symbol in this command is a "wildcard" — it means that we want all of the files from the source directory copy be copied into the destination directory namely <tt>~/{{site.course | downcase}}/{{page.num}}</tt>.

After doing this command, if you `cd` into <tt>~/{{site.course | downcase}}/{{page.num}}</tt> and use the `ls` command, you should see several files in your <tt>~/{{site.course | downcase}}/{{page.num}}</tt> directory&mdash;the same ones that you see if you visit the link <http://cs.ucsb.edu/~richert/cs32/misc/lab04a/>

If you don't see those files, go back through the instructions and make sure you didn't miss a step. If you still have trouble, ask your TA or mentor for assistance.

## Step 2 Getting the code to pass the tests

In this week's lab, you have the following files:

* `WordCount.cpp`
* `WordCount.h`
* `tddFuncs.cpp`
* `tddFuncs.h`

Your job is to modify `WordCount.cpp` based on the specifications and thoroughly test your code. Do not modify `WordCount.h`, and the given array of vectors structure must be used to implement your hash table. No Makefile or test applications are provided for this lab and you must create your own. `tddFuncs.*` are provided for testing your code. Even though Gradescope will not run the tests you create, you must submit your test file(s) to Gradescope. Name this file `lab04Test.cpp` (if you wrote multiple test files, you can name each one with `lab04Test01.cpp`, `lab04Test02.cpp`, etc.).

## Some notes about the hash table you will be implementing

* There are many ways to implement a hash table as we discussed in lecture. For this lab, we will implement a hash table that is an array containing vectors of `std::pair<std::string, int>`, where the string is the hash table key and the `int` value is the number of times the key has been inserted into the table. In the case of collisions, you must insert the new pair at the end of the vector for the given index based on the string key.
* A simple hash function is given to you. Do not modify this hash function.
* Hash keys are case insensitive
	* "KeY" and "kEy" are considered the same key. When inserting into your hash table, you must convert all valid keys into all *lower* case characters before hashing the key and updating the hash table.
* Be sure to carefully read the comments in `WordCount.h`. Write your tests to make sure your functionality abides by the specification.
* Be sure to initialize variables. It's possible that your code passes all of your tests locally but Gradescope may not pass your tests if you do not initialize variables (you may get in a lucky state on your computer, but the variables may have a different initial value if you forget to initialize variables).

## Step 3: Submitting via Gradescope

The lab assignment "Lab04a" should appear in your Gradescope dashboard in CMPSC 32. If you haven't submitted anything for this assignment yet, Gradescope will prompt you to upload your files.

You will submit your `WordCount.cpp` implementation along with a `lab04Test.cpp` file containing your test application(s) (as previously stated, if you wrote multiple test applications, you can name each one with `lab04Test01.cpp`, `lab04Test02.cpp`, etc.).  For this lab, you are <b>required</b> to submit your files with your github repo.

As mentioned earlier, you will not know Gradescope's score until AFTER the deadline has passed. When submitting your file, if you pass the compilation checks:

```
Checking stdout from make -B -f Makefile.check (1.0/1.0)
Checking stderr from make -B -f Makefile.check (1.0/1.0)
```

then that means your code compiled correctly and the test executables have been generated. If Gradescope doesn't pass this test, then your code did not compile correctly and you must fix the issues before resubmitting.

This lab (Lab04a) is worth a total of 50 points. You will have an opportunity to fix your code and pass all tests if you had any tests fail for this lab next week (Lab04b). Lab04b will be worth 50 points (100 points total for both Lab04 parts). If you passed all tests this week, all you need to do is resubmit your code to get all the points.

