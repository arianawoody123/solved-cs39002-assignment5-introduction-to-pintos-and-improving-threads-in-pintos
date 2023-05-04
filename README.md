Download Link: https://assignmentchef.com/product/solved-cs39002-assignment5-introduction-to-pintos-and-improving-threads-in-pintos
<br>
PintOS (Ben Pfaff et al.) [http://pintos-os.org/SIGCSE2009-Pintos.pdf] is a popular minimal operating used at many Universities – in India and overseas – to train students in the internal details of the Unix-like operating systems. PintOS is minimal since many OS capabilities are missing and you will be asked to implement them by modifying the codebase.

The primary document that we use in this subject was originally written at the Stanford University [https://web.stanford.edu/class/cs140/projects/pintos/pintos.pdf]. The more detailed version of PintOS and the projects used in Stanford is here: <a href="https://web.stanford.edu/class/cs140/projects/pintos/pintos.html">https://web.stanford.edu/class/cs140/projects/pintos/pintos.html</a> (This should suffice to give you a basic overview of PintOS).

This warming-up assignment has two parts: In the first one we ask you to install pintOS on a virtual machine with all the dependencies. In the second one we ask you to improve the thread implementation supported by PintOS.




<strong>Part-1:</strong> <strong>Installation of PintOS (20 marks)</strong>




We can install PintOS in many operating systems (like Windows, linux, mac) with slightly different set-up instructions for each platform. However, to ensure the uniformity of implementation (as well as not limit our ability to help you) we chose to use virtual boxes for our PintOS assignments.




For installing virtual box you need two things: (i) an operating system image and (ii) a software like oracle virtual box which can load the image and create virtual operating system (technically a Type-2 hypervisor).

<ul>

 <li>Download Oracle virtual box from here: <a href="https://www.virtualbox.org/wiki/Downloads">https://www.virtualbox.org/wiki/Downloads</a></li>

 <li>Download the image from here: <a href="https://www.osboxes.org/ubuntu/#ubuntu-14_04-vbox">https://www.osboxes.org/ubuntu/#ubuntu</a><a href="https://www.osboxes.org/ubuntu/#ubuntu-14_04-vbox">14_04</a><a href="https://www.osboxes.org/ubuntu/#ubuntu-14_04-vbox">–</a><a href="https://www.osboxes.org/ubuntu/#ubuntu-14_04-vbox">vbox</a> (We will use the “Ubuntu 14.04.6 Trusty Tahr” version)</li>

</ul>




Open the image using virtualbox, you will be asked to create a virtual machine, we suggest to assign it more than 2 GB or memory and more than 4 GB of physical storage.  You need to run PintOS in this virtual machine. Within your virtual machine PintOS will run in a simulator, we will use QEMU for this purpose. In your virtual box follow the following steps:




Installing PintOS with QEMU:

<ol>

 <li>Install qemu</li>

</ol>

<h1>&gt; sudo apt-get install qemu</h1>




<ol start="2">

 <li>Add a softlink</li>

</ol>

&gt; sudo ln -s /usr/bin/qemu-system-x86_64 /usr/bin/qemu




<ol start="3">

 <li>Download PintOS <a href="http://www.stanford.edu/class/cs140/projects/pintos/pintos.tar.gz">http://www.stanford.edu/class/cs140/projects/pintos/pintos.tar.gz</a> Extract PintOS to home folder (say $HOME)</li>

</ol>




<ol start="4">

 <li>Setup GDBMACROS</li>

</ol>

Open the script ‘pintos-gdb’ (in $HOME/pintos/src/utils. Find the variable GDBMACROS and set it to point to ‘$HOME/pintos/src/misc/gdb-macros’.




<ol start="5">

 <li>Compile utilities</li>

</ol>

<h1>&gt; cd $HOME/pintos/src/utils</h1>

Edit “Makefile” in the current directory and replace “LDFLAGS = -lm” with “LDLIBS = -lm”

<h1>&gt; make</h1>




<ol start="6">

 <li>Adjust Make.vars</li>

</ol>

Open the file “$HOME/pintos/src/threads/Make.vars” and change the last line to: SIMULATOR = –qemu




<ol start="7">

 <li>Compile PintOS Kernel</li>

</ol>

<h1>&gt; cd $HOME/pintos/src/threads/ &gt; make</h1>




<ol start="8">

 <li>Make these three changes in “$HOME/pintos/src/utils/pintos”

  <ol>

   <li>Change line no. 103 to “$sim = “qemu” if !defined $sim;” to enforce</li>

  </ol></li>

</ol>

QEMU as simulator

<ol>

 <li>On line no. 259, replace “kernel.bin” to path pointing to kernel.bin file at “$HOME/pintos/src/threads/build/kernel.bin”.</li>

 <li>Comment out all the lines in subroutine SIGVTALRM(lines 927 to 935)</li>

</ol>

<ol start="9">

 <li>Open “$HOME/pintos/src/utils/Pintos.pm” and replace “loader.bin” at line no. 362 to point to bin located at</li>

</ol>

“$HOME/pintos/src/threads/build/loader.bin”




<ol start="10">

 <li>Update PATH</li>

</ol>

Add below mentioned line at the end of $HOME/.bashrc file:

export PATH=$HOME/pintos/src/utils:$PATH

Restart the terminal or execute

&gt; source $HOME/.bashrc




<ol start="11">

 <li>Finally, it’s time to check if you have done everything ok: Run pintOS</li>

</ol>

&gt; cd $HOME/pintos/src/utils/

Verify installation of pintos using following command.

<h1>&gt; pintos run alarm-multiple</h1>

This should create 5 threads and sleep for some predefined times.




<strong>Deliverables:</strong>

You need to show your assigned TA that you installed pintos correctly and the “pintos run alarm-multiple” is working as expected.




<strong>Part 2: Implement an Alarm clock (80 marks)</strong>




<strong>Problem description:  </strong>

By default, PintOS kernel provides busy waiting when a thread needs to block. You need to change the behavior and block the thread till a fixed amount of timer ticks pass. You will work primarily in the “threads” directory of the PintOS distribution.




Before you start coding, go through this link:

<a href="https://web.stanford.edu/class/cs140/projects/pintos/pintos_2.html#SEC18">https://web.stanford.edu/class/cs140/projects/pintos/pintos_2.html#SEC18</a> .

it will provide you an understanding of the gist and functionalities of each relevant file.




<strong>Task: </strong>

Reimplement timer_sleep(), defined in devices/timer.c. Currently the implementation “busy waits” that is it spins in a loop checking the current time and calling thread_yield() until enough time has gone by. Here is the description of the function.




<u>Function:</u> void <strong>timer_sleep</strong> (int64_t ticks)

Suspends execution of the calling thread until time has advanced by at least x timer ticks. Unless the system is otherwise idle, the thread need not wake up after exactly x ticks. Just put it on the ready queue after they have waited for the right amount of time.




Reimplement the function to avoid busy waiting.


