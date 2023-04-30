Download Link: https://assignmentchef.com/product/solved-ec440-project-1-simple-shell
<br>



<strong>Project Goals:</strong>

<ul>

 <li>To understand &amp; correctly use important Unix/POSIX system calls.</li>

 <li>To develop a simple shell application.</li>

</ul>

<strong>Collaboration policy:</strong>

<ul>

 <li>You are encouraged to discuss this project with your classmates/instructors but are required to turn in your own solution.</li>

 <li>You must be able to fully explain your solution during oral examination.</li>

</ul>

<strong>Deadline:</strong>

Project 1 is due on Monday, September 30, 16:30 EDT (no deadline extensions or late submissions).

<strong>Project Description:</strong>

The goal of this project is to implement a basic shell which is able to execute commands, redirect the standard input/output (stdin/stdout) of commands to files, pipe the output of commands to other commands, and carry out commands in the background.

Your shell should implement a simple REPL (read – eval – print – loop) paradigm. Your shell should use “my_shell$” (without the quotes) as prompt. At each prompt, the user should be able to type commands (e.g., ls, ps, cat) which should be executed by the shell. You can access these binaries by searching directories determined by the PATH environment variable that is passed to your shell.

As in reality, commands can have arguments that are separated by whitespace (one or more space characters). For example, if the user types cat x, your shell will need to invoke the cat binary and pass x as its argument. When the shell has received a line of input, it typically waits until all commands have finished. Only then, a new prompt is displayed (however, this behavior can be altered – see below for details).

Your shell must also be able to interpret and execute the following meta-characters: ‘&lt;‘, ‘&gt;’, ‘|’, and ‘&amp;’:

<ul>

 <li><em>command</em> ‘&lt;‘ <em>filename</em> In this case, a command takes its input from the file (not stdin). Note that spacing is irrelevant in this case. For example, cat&lt;file and cat &lt;file are valid inputs. Also, only one input redirection is allowed for a single command. (cat &lt;&lt;file is invalid)</li>

 <li><em>command</em> ‘&gt;’ <em>filename</em> An input following this template indicates that a command writes its output to the specified file (not stdout). Again, spacing is irrelevant (see case a) and only one input redirection is allowed for a single command.</li>

 <li>‘| ’ The pipe character allows several commands to be connected, forming a pileline: the output of the command before “|” is piped to the input of the command following “|”. Multiple pipe</li>

</ul>

signs are allowed on the command line. Spacing is irrelevant (described above).

Example:

“cat a| sort | wc” (without quotes) indicates that the output of the cat command is channeled to the sort and sort sends its output to the input of the  wc program.

<ul>

 <li>The ampersand character ‘&amp;’ should allow user to execute a command (commands) in the background. In this case, the shell immediately displays a prompt for the next line regardless of whether the commands on the previous line have finished).</li>

</ul>

For simplification purposes, you should assume that only one ‘&amp;’ character is allowed and can only appear at the end of the line. Also, if the input line consists of multiple commands, only the first command on the input line can have its input redirected, and only the last can have its output redirected. In case of a single command, standard rules apply (e.g., cat &lt; x &gt; y is valid, while cat f | cat &lt; g is not).

In case of errors (e.g., if the input does not follow the rules/assumptions described above, command is not found, etc.), your shell should display an error message (cannot exceed a single line), print the prompt, and wait for the next input. The error message should follow the template “ERROR:” (without quotes) + your_error_message. To facilitate automated grading, when you start your simple shell program with the argument ‘-n’, then your shell must not output any command prompt (no “my_shell$ “). Just read and process commands as usual.

To exit the shell, the user must type Ctrl-D (pressing the D button while holding control). You may assume that the maximum length of individual tokens (commands and filenames) is 32 characters, and that the maximum length of an input line is 512 characters. Your shell is supposed to collect the exit codes of all processes that it spawns. That is, you are not allowed to leave zombie processes around of commands that you start. Your shell should use the fork(2) system call and the execvp(2) system call (or one of its variants) to execute commands. It should also use waitpid(2) or wait(2) to wait for a program to complete execution (unless the program is in the background). You might also find the documentation for signals (and in particular SIGCHLD) useful to be able to collect the status of processes that exit when running in the background.

<strong>Some hints:</strong>

<ol>

 <li>A simple shell such as this needs at least a simple command-line parser to figure out what the user is trying to do. To read a line from the user, you may use fgets(3).</li>

 <li>If a valid command has been entered, the shell should fork(2) to create a new (child) process, and the child process should exec the command.</li>

 <li>Before calling exec to begin execution, the child process may have to close stdin (file descriptor 0) or stdout (file descriptor 1), open the corresponding file or pipe (with open(2) for files, and pipe(2) for pipes), and use dup2(2) or dup(2) to make it the appropriate file descriptor. After calling dup2(2), close the old file descriptor.</li>

 <li>The main challenge of calling execvp(2) is to build the argument list correctly. If you use execvp(2), remember that the first argument in the array is the name of the command itself, and the last argument must be a null pointer.</li>

 <li>The easiest way to redirect input and output is to follow these steps in order:

  <ul>

   <li>open (or create) the input or output file (or pipe).</li>

   <li>close the corresponding standard file descriptor (stdin or stdout).</li>

   <li>use dup2 to make file descriptor 0 or 1 correspond to your newly opened file.</li>

   <li>close the newly opened file (without closing the standard file descriptor).</li>

  </ul></li>

 <li>When executing a command line that requires a pipe, the pipe must be created before forking the child processes. Also, if there are multiple pipes, the command(s) in the middle may have both input and output redirected to pipes. Finally, be sure the pipe is closed in the parent process, so that termination of the process writing to the pipe will automatically close the pipe and send an EOF (end of file) to the process reading the pipe.</li>

 <li>Any pipe or file opened in the parent process may be closed as soon as the child is forked – this will not affect the open file descriptor in the child.</li>

 <li>While the project assignment talks about system calls, feel free to use the libc wrapper functions, documented in their corresponding beautiful man pages instead.</li>

</ol>