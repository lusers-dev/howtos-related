
# Linux Users - Vim Windows Line Fix 

This command replaces instances of double newlines (`\n\n`) with a carriage return (`\r`), 
effectively removing the extra newline characters inserted by Windows.

## Vim Command for Removing Windows Double Return Lines

When editing files in Windows and moving them to Unix/Linux environments, 
you might encounter double return lines. This issue is typically due to the difference in how Windows and Unix/Linux handle line endings.

To fix this in Vim, you can use the following command:

```
## vim remove Windows double return lines
:%s/\n\n/\r/g

```


