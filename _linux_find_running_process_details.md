# Linux Users - Find Running Process Details

The `ps` command is used to, according to its man page, display information about a selection of the active process. 

The given Linux command is a pipeline command, also know as a one-liner, that is used to retrieve information about a running processes and format the output to display specific details. 

`running-process-details.txt`

```shell
## EXAMPLE 1

## find out more about processes for java

$ ps -ef | grep -i java |grep -v 'grep' | awk '{print $2}' |xargs -I {} ps -o pid -o ruser -o cmd fp {}
  PID RUSER    CMD
14569 appuser  java <sanitized details>
  PID RUSER    CMD
14621 appuser  java <sanitized details>
$

## find the path to the executable
$ which java
/usr/bin/java

## find out if the path is the absolute path or a sym link pointer, to another path

$ ls -l /usr/bin/java 
lrwxrwxrwx 1 root root 00 May 00  0000 /usr/bin/java -> /etc/alternatives/java

## diplay the update-alternatives config

$ update-alternatives --list
...
java	auto	/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-1.el7_6.x86_64/jre/bin/java
...
$

## EXAMPLE 2

## find out more about processes for chronyd
$ ps -ef | grep -i chronyd |grep -v 'grep' | awk '{print $2}' |xargs -I {} ps -o pid -o ruser -o cmd fp {}
    PID RUSER    CMD
  54515 chrony   /usr/sbin/chronyd -F 2

$ which chronyd
/usr/sbin/chronyd 

$ ls -l /usr/sbin/chronyd
-rwxr-xr-x 1 root root 469576 Jun  0 06:16 /usr/sbin/chronyd
$ 

```

Let's break down the command step by step:

`ps -ef | grep -i java |grep -v 'grep' | awk '{print $2}' |xargs -I {} ps -o pid -o ruser -o cmd fp {}`

- `ps -ef`: This command is used to display a snapshot of the currently running processes on the system. The options `-ef` specify that you want to display information about all processes in a long format.

- `|`: The pipe symbol (`|`) is used to redirect the output of the preceding command as input to the next command in the pipeline.

- `grep -i java`: This command filters the output of the previous command by searching for lines containing the word "java" (case-insensitive). It helps to narrow down the processes to those related to Java applications.

- `|`: Another pipe symbol is used to redirect the output of the previous `grep` command to the next command.

- `grep -v 'grep'`: This command further filters the output by excluding lines containing the word "grep". This is often used to remove the `grep` command itself from the list of processes since it's often included when searching for a process.

- `|`: Another pipe symbol is used again to redirect the output.

- `awk '{print $2}'`: This command uses the `awk` utility to extract the second column (the process ID) from each line of the filtered output. `awk` is a text processing tool that operates on columns or fields in a line.

- `|`: Another pipe symbol is used again to redirect the output.

- `xargs -I {} ps -o pid -o ruser -o cmd fp {}`: The `xargs` command is used to execute another command using the input provided by the previous command. In this case, the input is a list of process IDs generated by the previous steps. For each process ID, `xargs` runs the `ps` command to display specific information about that process:
   - `-o pid -o ruser -o cmd`: These options specify the format of the output from the `ps` command. It will display the process ID (`pid`), the real user owning the process (`ruser`), and the command that started the process (`cmd`).
   - `fp {}`: This part is a format specifier for `ps`, where `{}` is replaced by the process ID provided by `xargs`.



