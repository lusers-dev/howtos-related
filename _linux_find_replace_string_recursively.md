# Linux Users - Find and Replace Recursively


```shell

find ./ -type f -exec sed -i 's/<old-string>/<new-string>/g' {} +

```


The Linux command `find ./ -type f -exec sed -i 's/<old-string>/<new-string>/g' {} +` is used to find files in the current directory and its subdirectories, then perform an in-place search and replace operation for a string using `sed`. 
Here is an explanation of each part of the command:

- `find ./ -type f`: This part of the command initiates a search starting from the current directory (`./`) for files (`-type f`).
- `-exec`: This flag allows for the execution of a command on the files found by `find`.
- `sed -i 's/<old-string>/<new-string>/g' {} +`: 
  - `sed`: Invokes the stream editor for filtering and transforming text.
  - `-i`: Modifies files in place.
  - `'s/<old-string>/<new-string>/g'`: Specifies the substitution operation where `<old-string>` is replaced with `<new-string>`. The `g` flag ensures that all occurrences on each line are replaced, not just the first.
  - `{}`: Represents the placeholder for the file names found by `find`.
  - `+`: Indicates that multiple file names can be passed to a single invocation of `sed`, making it more efficient than using `\;`.

This command essentially searches for files in the current directory and its subdirectories, then uses `sed` to replace all occurrences of `<old-string>` with `<new-string>` in those files.


