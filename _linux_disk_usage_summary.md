# Linux Users - Disk Usage Summary

`disk-usage-summary-sample-1.txt`

```shell
## SAMPLE 1
## (lower s)

cd /; sudo  du -sh * | sort -rh | head


```

The above command, `cd /; sudo du -sh * | sort -rh | head`, serves the purpose of displaying a summary of disk usage for all files and directories in the root directory ("/") of a Linux system. Let's break down the command step by step:

- `cd /`: This part of the command changes the current working directory to the root directory ("/"). This step is not directly related to disk usage analysis but ensures that the subsequent command operates from the root directory.

- `sudo`: The `sudo` command is used to execute the following command with superuser privileges. This is necessary for the `du` (disk usage) command when analyzing files and directories that may require elevated permissions.

- `du -sh *`: Here, the `du` command is used to estimate the disk usage of files and directories in the root directory. The options used are:
   - `-s`: Summarize the total disk usage for each argument (in this case, for files and directories in the root).
   - `-h`: Print sizes in human-readable format (e.g., "K" for kilobytes, "M" for megabytes).

   The `*` is a wildcard character that represents all files and directories in the root directory.

- `|`: This is a pipe symbol, which is used to pass the output of the previous command (`sudo du -sh *`) as input to the next command.

- `sort -rh`: This part of the command sorts the output of the `du` command in reverse order based on disk usage. The options used are:
   - `-r`: This option tells `sort` to sort the output in reverse order, so the largest sizes will appear first.
   - `-h`: This option tells `sort` to interpret the sizes in a human-readable format, which ensures that sizes are compared correctly, considering suffixes like KB, MB, GB.

- `|`: Another pipe symbol is used here to pass the sorted output as input to the next command.

- `head`: Finally, the `head` command is used to display only the first few lines (by default, the first 10 lines) of the sorted output. This effectively shows you the top N largest files or directories in terms of disk usage in the root directory.



`disk-usage-summary-sample-2.txt`

```shell
## SAMPLE 2
## (upper S)

cd /home/; sudo du -Sh * | sort -rh | head


```

The above command, `cd /; sudo du -Sh * | sort -rh | head`, serves the purpose of displaying a summary of disk usage for all files and directories in the directory ("/home/") of a Linux system. Let's break down the command step by step:

- `cd /home/`: This command changes the current working directory to `/home/`. This is the directory where the subsequent operations will take place.

- `sudo`: (same as example above)

- `du -Sh *`: This command is used to calculate the disk usage of files and directories in the current directory (`/home/`) and display the results in a human-readable format. Here's what each option means:
   - `-S`: This option tells `du` not to include the sizes of subdirectories in the total size. It calculates the size of individual files and the immediate contents of directories.
   - `-h`: This option tells `du` to display sizes in a human-readable format, using units like KB, MB, GB, etc.

- `sort -rh | head `: (same as example above)



