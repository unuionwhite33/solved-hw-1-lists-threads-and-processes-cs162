Download Link: https://assignmentchef.com/product/solved-hw-1-lists-threads-and-processes-cs162
<br>
In this homework, you will gain familiarity with threads and processes from the perspective of a user program, which will help you understand how to support these concepts in the operating system. Along the way, you will gain experience with the list data structure used widely in Pintos, but in a user program running on Linux. We hope that completing this assignment will prepare you to begin Project 1, where you will work with the implementations of these constructs in the Pintos kernel. Our goal is to give you experience with how to use them in userspace, to understand the abstractions they provide in an environment where bugs are relatively easy to debug, before having to work with them in Pintos. It will also help you to see how you can do a lot of your development and testing of project code in a contained user setting, before you drop it into the Pintos kernel.

<h1><a name="_Toc6929"></a>1           Getting Started</h1>

Log in to your Vagrant Virtual Machine and run:

$ cd ~/code/personal/

$ git pull staff master $ cd hw1

Run make to build the code. Five binaries should be created: pthread, words, pwords, lwords, and fwords.

<h2><a name="_Toc6930"></a>1.1         Overview of Source Files</h2>

Below is an overview of the starter code:

words.c, word_helpers.c, word_helpers.h

This files contain a simple driver for parsing words, computing their aggregate count, and then writing the count to a stream, as in the previous homework.

word_count.h, word_count.c

These files provide the interface of and the implementation of the word_count abstraction using a conventional singly-linked data structure for the representation. They are, in effect, the preferred solution to your C refresher problem in Homework 0. You should study these files carefully and compare them to your previous work from Homework 0. You must <strong>not </strong>modify these files.

list.c, list.h

These files are the list library used in Pintos, which is based on the list library in Linux. You should be able to understand how to use this library based on the API given in list.h. You must <strong>not </strong>modify these files. If you’re interested in learning about the internals of the list library, feel free to read list.c and the list_entry macro in list.h. You can find a good explanation of the list_entry macro <a href="https://stackoverflow.com/questions/15832301/understanding-container-of-macro-in-the-linux-kernel">here</a><a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>.

word_count_l.c

This file is the starter code for your implementation of the word_count interface specified in word_count.h, using the Pintos list data structure. We have already provided the type declarations for this in word_count.h. You must use those. Notice how the list element is embedded into the struct, rather than the next pointer. Also, the Makefile provides the #define PINTOS_LIST as a flag to cc. words.c is unchanged, as is its behavior. This exercise will cement your understanding of how to traverse and manipulate the kinds of lists that are used throughout Pintos. Your implementation of the word_count interface in word_count_l.c, when linked with the driver in words.c, should result in an application, lwords that behaves identically to words, but internally uses the Pintos list data structure to keep track of word counts.

pwords.c

This file is starter code to implement the pwords application. This is a version of the words application, where each file is processed in a separate thread. You will need to modify it to spawn the threads and coordinate their work.

word_count_p.c

This file is starter code to implement a version of word_count_l.c that not only uses the Pintos list data structure, but also provides proper synchronization when accessing the word_count data structure concurrently from multiple threads. This implementation of the word_count interface will be linked with your code in pwords.c to produce the pwords application. You will need to complete it.

fwords.c

This file implements a version of the lwords application where each file is processed in a separate process. It uses word_count_l.c to implement the word_count abstraction.

pthread.c

This file implements an example application that creates multiple threads and prints out certain memory addresses and values. In this assignment, you will answer some questions about this program and its output.

<h1><a name="_Toc6931"></a>2           Check Yourself on Word Count</h1>

Read the basic words files and compare them with your Homework 0 solution. You will be building on this one, so make sure you understand it.

<h1><a name="_Toc6932"></a>3           Warm Up (Optional)</h1>

Write a simple application that reads in the output produced by words and creates the corresponding word_count data structure. Print it out to check.

<h1><a name="_Toc6933"></a>4           Using Pintos Lists to Count Words</h1>

The Pintos operating system makes heavy use of a particular linked list library taken directly from Linux. Familiarity with this library will make it easier to understand Pintos, and you will need to use it in your solution for the projects. The objective of this exercise is to build familiarity with the Pintos linked list library in userspace, where issues are easier to debug than in the Pintos kernel.

First, read list.h to understand the API to library.

Then, complete word_count_l.c so that it properly implements the new word_count data structure with the Pintos list representation. You <strong>MUST </strong>use the functions in list.h to manipulate the list.After you finish making this change, lwords should work properly.

The wordcount_sort function sorts the wordcount list according to the comparator passed as an argument. Although words and lwords sort the output using the less_count comparator defined in word_helpers.c, the wordcount_sort function that you write should work with any comparator passed to it as the argument less. For example, passing the less_word function in word_helpers.c as the comparator should also work. If you’re having trouble with function pointer syntax when implementing this, <a href="https://www.geeksforgeeks.org/function-pointer-in-c/">here</a><a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> is a good tutorial.

<em>Hint #1</em>: We provide a Makefile that will build lwords based on these source files. It compiles your program with the -DPINTOS_LIST flag, which is equivalent to putting a #define PINTOS_LIST at the top of the file. This selects a definition of the word count structure that uses Pintos lists. We recommend reading the word_count.h file to understand the new structure definition so you can see how the Pintos list structure is being used.

<em>Hint #2</em>: The provided Makefile uses the same words.c file to provide the main() function in both the words and lwords programs. So that the same main() function works for lwords, you should ensure that your implementation of each function in word_count_l.c is functionally equivalent to the corresponding function in word_count.c. In other words, both word_count_l.c and word_count.c must implement the same interface, namely the one in word_count.h.

<h1><a name="_Toc6934"></a>5           Observing a Multi-Threaded Program</h1>

The pthread application is an example application that uses multiple threads. First, read pthread.c carefully. Then, run pthread multiple times and observe its output. Answer the following questions in a file called hw1.txt:

<ol>

 <li>Is the program’s output the same each time it is run? Why or why not?</li>

 <li>Based on the program’s output, do multiple threads share the same stack?</li>

 <li>Based on the program’s output, do multiple threads have separate copies of global variables?</li>

 <li>Based on the program’s output, what is the value of void *threadid? How does this relate to the variable’s type (void *)?</li>

 <li>Using the first command line argument, create a large number of threads in pthread. Do all threads run before the program exits? Why or why not?</li>

</ol>

<h1><a name="_Toc6935"></a>6           Using Multiple Threads to Count Words</h1>

The words program operates in a single thread, opening, reading, and processing each file one after another. In this exercise, you will write a version of this program that opens, reads, and processes each file in a separate thread.

First, read and understand pwords.c, which is a first cut at a program that intends to use multiple threads to count words.

Your task is to properly implement the pwords application. You will make changes to pwords.c and word_count_p.c to complete this task. It will need to spawn threads, open and process each file in a separate thread, and properly synchronize access to shared data structures when processing files. <strong>Your synchronization must be fine-grained. </strong>Different threads should be able to open and read their respective files concurrently, serializing only their modifications to the shared data structure. In particular, it is unacceptable to use a global lock around the call to count_words() in pwords.c, as such a lock would prevent multiple threads from reading the files concurrently. Instead, you should only synchronize access to the word count list data structure in word_count_p.c. You will need to ensure all such modifications are complete before printing the result or terminating the process.

We recommend that you start by just implementing the thread-per-file aspect, without synchronizing updates to the word count list.Can you even detect the errors? Multithreaded programs with synchronization bugs may appear to work properly much of the time. But, the bugs are latent, ready to cause problems.

To help you find subtle synchronization bugs in your program, we have provided a somewhat large input for your words program in the gutenberg/ directory. To generate these files, we selected some stories from among the <a href="https://www.gutenberg.org/ebooks/search/?sort_order=downloads">most popular books made freely available by Project Gutenberg</a><a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a>, making sure to choose short stories so that the word count program does not take too long to run. You should compare the result of running your pwords program on the Gutenberg dataset to the result of running words on the Gutenberg dataset and ensure they are same. This does not guarantee that your code is correct, but it might alert you to subtle concurrency bugs that may not manifest for smaller inputs.

<em>Hint #1</em>: The Makefile that we provide will compile your pwords.c program with the two flags

-DPINTOS_LIST -DPTHREADS, which select a definition of the word count structure that not only uses Pintos lists, but also includes a mutex that you may find useful for synchronization. Unlike the lwords exercise, in which the word_count_t structure was typedef’d to the Pintos list structure directly, the word_count_t structure now <em>contains </em>the Pintos list structure and a mutex. We expect your code in word_count_p.c to be similar to your code in word_count_l.c, with syntactic changes according to the new word_count_t structure and added synchronization to allow concurrent use of the word_count API as needed for pwords.

<em>Hint #2</em>: The man pages for pthread_mutex_lock and related functions are, unfortunately, not included in the class VM by default. To install them, run sudo apt install glibc-doc.

<em>Hint #3</em>: The multiple threads should aggregate their results without reading from or writing to any intermediate files. <strong>Attempting to open or read from any files other than the ones passed as input to your program may cause autograder tests to fail or not be run.</strong>

<h1><a name="_Toc6936"></a>7           Using Multiple Processes to Count Words</h1>

In this exercise, you will write a version of the words program that opens, reads, and processes each file in a separate process.

First, read and understand fwords.c, which is your skeleton code for this exercise. It is designed to fork a separate child process to process each file and them merge the output of each process into the final output for the program.

Complete the fwords application by modifying fwords.c. Our provided Makefile links it with the code in word_count_l.c, NOT word_count_p.c. Each file must be opened, read, and processed from a separate process. You should not use pthreads (e.g., do not call pthread_create).

Notice the difference in the thread-based and process-based approaches. Are each of these processes able to use the word_counts variable declared in main? Do they interact with each other when they do? How is synchronization accomplished?)

<em>Hint #1</em>: Because each process operates in a separate address space, you do not need a mutex for synchronization.

<em>Hint #2</em>: The pipe system call and fdopen library call might be useful. For information about them, run man 2 pipe and man 3 fdopen.

<em>Hint #3</em>: The multiple processes should aggregate their results without reading from or writing to any intermediate files. <strong>Attempting to open or read from any files other than the ones passed as input to your program may cause autograder tests to fail or not be run.</strong>

<h1><a name="_Toc6937"></a>8           Additional Questions</h1>

Answer the following additional questions in hw1.txt:

<ol start="6">

 <li>What are the pros and cons of using threads, as opposed to processes, to process each file?</li>

 <li>Briefly compare the performance of lwords, pwords, and fwords when run on the Gutenberg dataset. How might you explain the performance differences?</li>

 <li>Under what circumstances would pwords perform better than lwords? Under what circumstances would lwords perform better than pwords? Is it possible to use multiple threads in a way that always performs better than lwords?</li>

</ol>

<h1><a name="_Toc6938"></a>9           Stretch (Optional)</h1>

Now that you are familiar with threads and processes, you can further improve your solution to pwords by reducing the synchronization overhead. Can you synchronize your thread-based solution in pwords using a similar approach to fwords. What use do you make of thread-local storage?

Once you’ve done the above exercise, consider implementing pwords in Go. Using goroutines instead of threads and channels instead of pipes to synchronize in a similar way to fwords.

<h1><a name="_Toc6939"></a>10          Autograder and Submission</h1>

To submit and push to autograder, first commit your changes, then do:

$ git push personal master

Within a few minutes you should receive an email from the autograder. (If you haven’t received an email within half an hour, please notify the instructors via a private post on Piazza.) Please do not print anything extra for debugging, as this can interfere with the autograder.

Your work in hw1.txt will not be graded by the autograder. It will be graded manually based on effort.

<strong>Important Autograder Change: </strong>Starting with this assignment, your final score in the autograder will no longer be the maximum of your attempts, but rather your score for only your latest build. Any non-autograded components, like style and written portions, will be graded based on your last build for the assignment. Running a build after the deadline will consume slip days even if it doesn’t change your score.

<a href="#_ftnref1" name="_ftn1">[1]</a> https://stackoverflow.com/questions/15832301/understanding-container-of-macro-in-the-linux-kernel

<a href="#_ftnref2" name="_ftn2">[2]</a> https://www.geeksforgeeks.org/function-pointer-in-c/

<a href="#_ftnref3" name="_ftn3">[3]</a> https://www.gutenberg.org/ebooks/search/?sort order=downloads