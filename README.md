Download Link: https://assignmentchef.com/product/solved-cse-333-operating-systems-programming-assignment-2-solved
<br>



This programming assignment is related with writing a simple shell by considering the outline program given in the lab sessions.

The main()  function of your program presents the command line prompt  “<strong>myshell: ”</strong> and then invokes setup() function which waits for the user to enter a command. The <em>setup function</em> (given in the outline program of your textbook) reads the user’s next command and parses it into separate tokens that are used to fill the argument vector for the command to be executed. It can also detect background processes. This program is terminated when the user enters <strong>^D  (&lt;CONTROL&gt;&lt;D&gt;); </strong>and setup function then invokes exit. The contents of the command entered by the user is loaded into the <em>args</em> array. You may assume that a line of input will contain no more than 128 characters or more than 32 distinct arguments.

Necessary functionalities and components of your shell is listed below:

<ol>

 <li>It will take the command as input and will execute that in a new process. When your program gets the program name, it will create a new process using <strong><em>fork()</em></strong> system call, and the new process (child) will execute the program. The child will use the execl() function in the below to execute a new program.</li>

</ol>

<ul>

 <li>Use <strong><em>execv()</em></strong> instead of <strong><em>execvp(),</em></strong> which means that you will have to read the <strong>PATH</strong> environment variable, then search each directory in the <strong>PATH</strong> for the command file name that appears on the command line.</li>

 <li><strong><u>Important Notes:</u></strong>

  <ol>

   <li>Using the “system()” function is not allowed for part A!</li>

   <li>In the project, you need to handle foreground and background processes. When a process run in foreground, your shell should wait for the task to complete, then immediately prompt the user for another command.</li>

  </ol></li>

</ul>

<h1>myshell: gedit</h1>

A background process is indicated by placing an ampersand (’&amp;’) character at the end of an input line. When a process run in background, your shell should not wait for the task to complete, but immediately prompt the user for another command.

<h1> myshell: gedit &amp;</h1>

With background processes, you will need to modify your use of the wait() system call so that you check the process id that it returns.

<ol>

 <li>It must support the following internal (<em>built-in</em>) commands. <em>Note that an internal command is the one for which no new process is created but instead the functionality is built directly into the shell itself. </em>

  <ul>

   <li><strong><em>history – </em></strong>seethe list of and execute the last 10 commands. See the following example for the use of these commands.</li>

  </ul></li>

</ol>

<strong><em>Example:</em></strong>

<strong>myshell&gt; history</strong>

<ul>

 <li><strong>ps</strong></li>

 <li><strong>ls</strong></li>

 <li><strong>ls -l</strong></li>

 <li><strong>who</strong></li>

 <li><strong>ps -a</strong></li>

 <li><strong>chmod 777 a.txt</strong></li>

 <li><strong>ls | wc</strong></li>

 <li><strong>ps – ef</strong></li>

 <li><strong>ps</strong></li>

 <li><strong>ls -almyshell&gt; ls /bin myshell&gt; history</strong></li>

 <li><strong>ls /bin</strong></li>

 <li><strong>ps</strong></li>

 <li><strong>ls</strong></li>

 <li><strong>ls -l</strong></li>

 <li><strong>who</strong></li>

 <li><strong>ps -a</strong></li>

 <li><strong>chmod 777 a.txt</strong></li>

 <li><strong>ls | wc</strong></li>

 <li><strong>ps – ef</strong></li>

 <li><strong>psmyshell&gt; history -i 9</strong></li>

</ul>

<strong>PID TTY          TIME CMD         6052 pts/0    00:00:00 ps myshell&gt; history</strong>

<ul>

 <li><strong>ps</strong></li>

 <li><strong>ls /bin</strong></li>

 <li><strong>ps</strong></li>

 <li><strong>ls</strong></li>

 <li><strong>ls -l</strong></li>

 <li><strong>who</strong></li>

 <li><strong>ps -a</strong></li>

 <li><strong>chmod 777 a.txt</strong></li>

 <li><strong>ls | wc</strong></li>

 <li><strong>ps – ef</strong></li>

</ul>

In the first line, after issuing <strong>history </strong>command, the lastly executed 10 commands are printed on th screen. After executing <strong>ls /bin</strong>, you can see that the history is updated. With <strong>history -i num</strong>, we can execute the command at num index. After executing the command at some index, the history table is updated again.

To implement this part, you cannot use history command of Linux. You have to implement the functionality by yourself, creating data structures to store commands as necessary. When we want to execute a command at an index, execute it using the A part of the homework.

<ul>

 <li><strong><em>^Z </em></strong><em><sub>– </sub></em>Stop the currently running foreground process, as well as any descendants of that process (e.g., any child processes that it forked). If there is no foreground process, then the signal should have no effect.</li>

 <li><strong><em>path</em></strong> – path is a command not only to show the current pathname list, but also to append or remove several pathnames. In your shell implementation, you may keep a variable or data structure to deal with pathname list (referred to as “path” variable below). This list helps searching for executables when users enter specific commands.

  <ul>

   <li><strong><em>path (without arguments)</em></strong> – displays the pathnames currently set. It should show pathnames separated by colons. ex) “/bin:/usr/bin”</li>

   <li><strong><em>path + /foo/bar</em></strong> –  appends the pathname to the “path” variable. Only one pathname will be given to each invocation of the path command. It’s okay to add a pathname to the “path” variable even if it already contains the same pathname, i.e., duplicates in the “path” variable are fine.</li>

   <li><strong><em>path</em></strong> <strong><em>­ /foo/bar</em></strong> – removes the pathname from the “path” variable. It should remove all duplicates of the given pathname. Only one pathname will be given to each invocation of the path command.</li>

  </ul></li>

 <li><strong><em>fg %num </em>– </strong>Move the background process with process id num to the background.. Note that for this, you have to keep track of all the background processes.</li>

 <li><strong><em>exit ­</em> </strong>Terminate your shell process. If the user chooses to exit while there are background processes, notify the user that there are background processes still running and do not terminate the shell process unless the user terminates all background processes.</li>

</ul>

<ol>

 <li><strong>I/O Redirection</strong></li>

</ol>

The shell must support I/O-redirection on either or both <em>stdin </em>and/or <em>stdout</em> and it can include arguments as well. For example, if you have the following commands at the command line:

<ul>

 <li><strong>myshell: <em>myprog [args] &gt; file.out </em></strong></li>

</ul>

Writes the standard output of <strong><em>myprog </em></strong>to the file <strong><em>file.out. file.out</em></strong> is created if it does not exist and truncated if it does.

<ul>

 <li><strong>myshell: <em>myprog [args] &gt;&gt; file.out </em></strong></li>

</ul>

Appends the standard output of <strong><em>myprog </em></strong>to the file <strong><em>file.out. file.out</em></strong> is created if it does not exist and appended to if it does.

<ul>

 <li><strong>myshell: <em>myprog [args] &lt; file.in</em></strong></li>

</ul>

Uses the contents of the file  <strong><em>file.in</em></strong>  as the standard input to program <strong><em>myprog</em></strong>.

<ul>

 <li><strong><em>myshell: myprog [args] 2&gt; file.out</em></strong></li>

</ul>

Writes the standard error of <strong><em>myprog </em></strong>to the file <strong><em>file.out</em></strong>.

<ul>

 <li><strong><em>myshell:</em> <em>myprog [args] &lt; file.in &gt; file.out</em></strong></li>

</ul>

Executes the command  <strong><em>myprog which</em></strong> will read input from <strong><em>file.in </em></strong> and stdout of the command is directed to the file  <strong><em>file.out</em></strong>


