Download Link: https://assignmentchef.com/product/solved-cs537-project-1b
<br>
<h5 id="cs537-spring-2020-project-1a"><span style="font-size: 2em; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif;">Administrivia</span></h5>

<ul>

 <li>Questions: We will be using Piazza for all questions.</li>

 <li>Collaboration: The assignment has to be done by yourself. Copying code (from others) is considered cheating. <a href="http://pages.cs.wisc.edu/~remzi/Classes/537/Spring2018/dontcheat.html">Read this</a> for more info on what is OK and what is not. Please help us all have a good semester by not doing this.</li>

 <li>This project is to be done on the <a href="https://csl.cs.wisc.edu/services/instructional-facilities">lab machines</a>.</li>

 <li>Some tests will be soon provided at <em>~cs537-1/tests/p1b</em>. Once they are available, read more about the tests, including how to run them, by executing the command <code>cat ~cs537-1/tests/p1b/README</code> on any lab machine. Note these test cases are not complete, and you are encouraged to create more on your own.</li>

</ul>

<h2 id="How to XV6"><b>XV6 Basics</b></h2>

You will be using the current version of xv6. Begin by copying this directory <b>~cs537-1/xv6-sp20</b>. If, for development and testing, you would like to run xv6 in an environment other than the CSL instructional Linux cluster, you may need to set up additional software. You can read these instructions for the MacOS build environment (You’ll still need to copy the version of xv6 in the previously mentioned directory). <a href="https://github.com/remzi-arpacidusseau/ostep-projects/blob/master/INSTALL-xv6.md">(link)</a> Note that we will run all of our tests and do our grading on the instructional Linux cluster so you should always ensure that the final code you handin works on those machines.

After you have obtained the source files, you can run <b>make qemu-nox</b> to compile all the code and run it using the QEMU emulator. Test out the unmodified code by running a few of the existing user-level applications, like ls and forktest.

To quit the emulator, type <b>Ctl-a x</b>. This means that you should press Control and A at the same time, release them, and then press x.

For additional information about xv6, we strongly encourage you to look through the code while reading <a href="https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf">this book</a> by the xv6 authors. We are using a slightly different xv6 version, but most of the information is still accurate

Or, if you prefer to watch videos, the last ten minutes of the <a href="https://www.youtube.com/watch?v=5H5esXbVkC8">first video</a> plus a <a href="https://www.youtube.com/watch?v=vR6z2QGcoo8&amp;feature=youtu.be">second video</a> from a previous year are relevant. You can also wait for the video from the discussion section this week Jan 30th!

Note that the version of xv6 we are using may be slightly different then that in the video. We always recommend that you look at the actual code yourself while either reading or watching (perhaps pausing the video as needed).

<h2 id="Assignment">Assignment</h2>

We’ll be doing kernel hacking projects in <b>xv6</b>, a port of a classic version of Unix to a modern processor, Intel’s x86. It is a clean and small kernel.

This first project is intended to be a warmup, and thus relatively light. You will not write many lines of code for this project. Instead, a lot of your time will be spent learning where different routines are located in the existing source code.

Learning Objectives:

<ul>

 <li>Gain comfort looking through more substantial code bases written by others in which you do not need to understand every line</li>

 <li>Obtain familiarity with the xv6 code base in particular</li>

 <li>Learn how to add a system call to xv6</li>

 <li>Become familiar with a few of the data structures in xv6 (e.g., process table and file)</li>

 <li>Use the gdb debugger on xv6</li>

</ul>

<h3 id="Learning to Debug w/ GDB">Learning to Debug w/ GDB</h3>

The first part of this assignment it to learn and demonstrate your ability to use gdb to debug xv6 code. To do this we ask you to use gdb to show the integer value that <strong>fdalloc</strong> function returns the first time it is called after a process has been completely initiaized

To do this follow these steps:

<ol>

 <li>Before you start, modify the ~/.gdbinit file to contain the directory where your xv6 directory is. For example if xv6-sp20 is in hour home directory, after your edit you should see something like<pre><code># cat ~/.gdbinitadd-auto-load-safe-path /afs/cs.wisc.edu/u/F/S/LOGIN/xv6-sp20/.gdbinit</code></pre>where F refers to the first letter of your CS username, S refers to the second letter of your CS username and LOGIN refers to your CS username.</li>

 <li>In one window, type <strong>make qemu-nox-gdb</strong>.</li>

 <li>In a second window on the same machine, cd to the <strong>same directory</strong> and type <strong>gdb</strong> (it will attach to the qemu process).</li>

 <li>Once it is attached type <strong>continue</strong> in the gdb window. You will see xv6 finish its bootup process and you’ll see it’s shell prompt.</li>

 <li>Interupt gdb and set a <b>breakpoint</b> at the <b>fdalloc</b> routine.</li>

 <li>Continue <b>gdb</b></li>

 <li>In the xv6 shell, run the <b>stressfs</b> user application. Your gdb process should now have stopped in fdalloc.</li>

 <li>Now, <b>step</b>(or probably <b>next</b>) through the C code until gdb reaches the point just before fdalloc() returns and <b>print</b> the value that will be returned (i.e. the value of fd).</li>

 <li>Now immediately quit gdb and run <b>whoami</b> to display your login name.</li>

 <li>Take a screenshot showing your gdb session with the returned value of fd printed and your login name displayed. Submit this screenshot to Canvas.</li>

</ol>

To sanity check your results, you should think about the value you expect fdalloc() to return in these circumstances to make sure you are looking at the right information. What is the first fd number returned after stdin, stdout, and stderr have been set up?

If gdb gives you the error message that fd has been optimized out and cannot be displayed, make sure that your Makefile uses the flag “-ggdb” instead of “-O2”.    Debugging is also a lot easier with a single CPU, so if isnt already set: in your <b>Makefile</b> find where the number of CPUS is set and change this to be 1.

If you get an error message saying that “… .gdbinit auto-loading has been declined by your `auto-load safe-path’…” then copy the path listed, open up the file ~/.gdbinit, paste the path, and try opening a fresh gdb instance. You may need to open a fresh window. For example the if the xv6-sp20 directory is in your home directory the contents of ~/.gdbinit would look like

<pre><code># cat ~/.gdbinitadd-auto-load-safe-path /afs/cs.wisc.edu/u/F/S/LOGIN/xv6-sp20/.gdbinit</code></pre>

with F, S referring to the first letter, second letter of your CS username and and LOGIN referring to your CS username.

<h3 id="XV6 System Calls">XV6 System Calls</h3>

For this assingment you’ll be adding one system call to xv6:

<ul>

 <li><b>int getfilenum(int pid)</b> which returns the number of files that the process identified by pid currently has open.</li>

</ul>

You must use the name of the system call EXACTLY as specified!

<h3 id="Details">Implementation Details</h3>

The primary files you will want to examine in detail include <b>syscall.c</b>, <b>sysproc.c</b>, <b>proc.h</b>, and <b>proc.c</b>.

To add a system call, find some other very simple system call that also takes an integer parameter, like sys_kill, copy it in all the ways you think are needed, and modify it so it doesn’t do anything and has the new name. Compile the code to see if you found everything you need to copy and change. You probably won’t find everything the first time you try.

Then think about the changes that you will need to make so your system calls act like required.

<ul>

 <li>How will you find out the pid that has been passed to your system call? This is the same as what <b>sys_kill()</b> does.</li>

 <li>How will you find the data structures for the specified process? You’ll need to look through the <b>ptable.proc</b> data structure to find the process with the matching pid.</li>

</ul>

To test your system call, you can make a simple user program that opens some files and calls your new system call. Similar to figuring out how to hook up your system call in all the right places, you can follow a user program such as ls and do the same.

We will also be releasing a simple test program and details about this will be posted on Piazza.

You may find the command <b>grep</b> helpful when you are trying to find all the places in the code base that you need to modify

Good luck! While the xv6 code base might seem intimidating at first, you only need to understand very small portions of it for this project. This project is very doable!

<h2 id="Handin"><b>Handin</b></h2>

There are 2 steps

<ol>

 <li style="list-style-type: none;">

  <ol>

   <li>For your xv6 code, your handin directory is <b>~cs537-1/handin/LOGIN/p1b</b> where <b>LOGIN</b> is your CS login. Please create a subdirectory <b>~cs537-1/handin/LOGIN/p1b/src</b>. Copy all of your source files (but not .o files, please, or binaries!) into this directory. A simple way to do this is to copy everything into the destination directory, then type “make” to make sure it builds, and then type “make clean” to remove unneeded files.</li>

   One way to do this is to navigate to your solution’s working directory and execute the following command:

  </ol></li>

</ol>

<pre><code>mkdir -p ~cs537-1/handin/LOGIN/p1b/src </code></pre>

<pre><code>cp -r . ~cs537-1/handin/LOGIN/p1b/src </code></pre>

When executing <code>ls -l</code> in the <code>~cs537-1/handin/LOGIN/p1b/src</code> directory the contents should be similar to what is shown below:

<pre><code>$ ls -ltotal 28-rw-r----- 1 &lt;user-name&gt; &lt;user-name&gt;  223 Feb 27  2012 FILESdrwxr-x--- 2 &lt;user-name&gt; &lt;user-name&gt; 2048 Jan 24 16:04 include/drwxr-x--- 2 &lt;user-name&gt; &lt;user-name&gt; 6144 Jan 28 12:01 kernel/-rw------- 1 &lt;user-name&gt; &lt;user-name&gt; 4823 Jan 23 14:51 Makefile-rw-r----- 1 &lt;user-name&gt; &lt;user-name&gt; 1793 Feb 27  2012 READMEdrwxr-x--- 2 &lt;user-name&gt; &lt;user-name&gt; 2048 Jan 28 12:01 tools/drwxr-x--- 2 &lt;user-name&gt; &lt;user-name&gt; 4096 Jan 28 12:01 user/-rw-r----- 1 &lt;user-name&gt; &lt;user-name&gt;   22 Feb 27  2012 version</code></pre>

<ol>

 <li>Upload your debugging <b>screenshot</b> to to Canvas</li>

</ol>

<h3 id="acknowledgments">Acknowledgments</h3>

The assignment borrows content from assignment 2 of Prof. Andrea Arpaci-Dusseau’s course in Fall 2019.