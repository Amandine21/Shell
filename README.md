# Shell

  +The purpose of this assignment is to create a terminal that could run bash commands in
a variety of situations. When designing this terminal the starter code provided two classes, the
tokenizer and command class was used to implement the Shell.cpp. The tokenizer class is
used to tokenize the input into a vector made out of the Command pointer objects. The
Command class is used to store information about the commands being passed into the input.\
  +When designing the shell the first thing that was necessary was to address the output of
the prompt for the user. The getenv() system call was used to get the username, the time()
function was used to get the current time the system was recording, and the getcwd() was used
to get the current working directory where the user was located. The prompt was then output to
display in order the time, username, and current directory\ 
  The whole shell program operated in an infinite for loop which will run until the user
typed the “exit” command. The loop waited for the user to type in an input using the getline()
function which will store the input into a string. A tokenizer object was created to tokenize the
input, and there was an error check associated with it. Following that, the background
processes will be checked in a for loop using iterators to traverse a vector holding the process
IDs to see if there are any child processes that are finished using the waitpid() system call. The
system call will be used in a nonblocking fashion, and If there is a process that is finished it will
erase the process from the vector.\
  After that, the cd or change directory cases were addressed in an if statement. If the first
command in the vector from the tokenizer object was a “cd” a function will be executed defined
outside of the main. The function will take in the arguments following the cd, an empty character
array representing the previous working directory, and the function will contain three conditions.
The first condition will address if the directory has a size of one and the first character is a dot,
the current directory will remain the same. The second condition will check to see if the directory
being passed is a “-”, and if it is it will navigate to the previous directory using the chdir() system
call. The final condition will change the directory to whatever the user has passed using the
chdir() system call, and will store the return value into an integer to check for an error.\
  Following the cd cases, the input/output redirection will be addressed by checking if the
current command is passed as an input or output using member functions from the command
class. If any of these cases are true, there will be a series of independent if statements that will
execute functions defined outside of the main function. These functions will address the file
descriptors by opening the file with a series of flags and depending on if it's a read or write file
descriptor the function will use the dup2() system call to switch the file descriptors, along with
some error checks.\
  Finally, this code was completed by piping meaning the prompt had the potential to run
multiple processes which could transfer information to each other. The way the code was
constructed was in an if-else statement nested within a for loop iterating through the passed
commands. The child process ran the code mentioned above except the cd commands which
were outside of the loop going through the commands. The parent process was to keep track of
the child's process which was conducted in the else statement. The pipe was created in an
array with a length of two and was passed into the pip() system call. Then a fork was created
which was assigned to a process id with a variable named pid. If the pid was zero it created the
child process, and otherwise, it was a parent process. In the child process, the read end of the
pipe was closed while the dup2() system call was used to point the standard out to the write end
of the pipe. At the end of the child process and execvp will run which will execute the command
and clear out dynamic memory according to a TA. In the parent process, the write end of the
pipe will be closed and dup2() will be used to give the read end of the process the standard
input. Finally, if the loop is at the last command it will commit a waitpid(), if not it will append the
command to the background process. Finally, a temporary variable will be used to reset the file
descriptors which used the dup() system call with respect to the standard read
