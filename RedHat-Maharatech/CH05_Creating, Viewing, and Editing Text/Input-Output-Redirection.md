# Input/Output Redirection in Linux

## Overview
In Linux, input/output redirection is used to control where commands get their input and where they send their output. This allows users to manipulate the flow of data between files, commands, and devices.
![image](https://github.com/user-attachments/assets/c1f0aea9-5104-44ed-9cac-2e1c562723e3)

## Standard Input, Output, and Error

- **Standard Input (stdin)**: Input stream (keyboard or file) — file descriptor `0`.
- **Standard Output (stdout)**: Output stream (usually the terminal) — file descriptor `1`.
- **Standard Error (stderr)**: Error messages stream — file descriptor `2`.
  ![image](https://github.com/user-attachments/assets/3199ff49-f3f1-4077-9991-d4338e7198de)

## Redirection Symbols

### Redirecting Standard Output

To redirect the output of a command to a file:
```bash
command > file
```
This will overwrite the file. For example:
```bash
echo "Hello, World!" > hello.txt
```
This writes `Hello, World!` to `hello.txt`. If the file exists, its content will be overwritten.

To append instead of overwriting:
```bash
command >> file
```
For example:
```bash
echo "This is a new line" >> hello.txt
```
This appends the text `This is a new line` to `hello.txt` without overwriting existing content.

### Redirecting Standard Input

To use a file as input for a command:
```bash
command < file
```
For example:
```bash
sort < unsorted.txt
```
This command sorts the contents of `unsorted.txt` and outputs the result to the terminal.

### Redirecting Standard Error

To redirect error messages to a file:
```bash
command 2> errorfile
```
For example:
```bash
ls nonexistentfile 2> error.txt
```
This command tries to list a nonexistent file, and the error message is saved to `error.txt`.

To append error messages instead of overwriting:
```bash
command 2>> errorfile
```
For example:
```bash
ls nonexistentfile 2>> error.txt
```
This appends the error message to `error.txt` without removing previous error logs.

### Redirecting Both Output and Error

To redirect both stdout and stderr to the same file:
```bash
command > file 2>&1
```
For example:
```bash
find / -name "*.conf" > output.txt 2>&1
```
This command searches for all `.conf` files starting from the root directory and writes both the output and any error messages to `output.txt`.

### Redirecting Output to `/dev/null`

To discard the output or error:
```bash
command > /dev/null
```
For example:
```bash
ping -c 5 google.com > /dev/null
```
This runs a `ping` command and discards the output, so nothing is displayed.

To discard both output and error:
```bash
command > /dev/null 2>&1
```
For example:
```bash
ping -c 5 google.com > /dev/null 2>&1
```
This discards both the output and any error messages from the `ping` command.

## Examples

### Redirect Output to a File

```bash
ls -l > filelist.txt
```
This command saves the output of `ls -l` (detailed listing of files) into `filelist.txt`. If the file exists, it will be overwritten.

### Append Output to a File

```bash
echo "new line" >> filelist.txt
```
This appends the text `new line` to `filelist.txt` without deleting previous content.

### Redirect Error Messages

```bash
ls nonexistentfile 2> errorlog.txt
```
This command tries to list a file that doesn't exist, and the error message is saved to `errorlog.txt`.

### Redirect Both Output and Error

```bash
find / -name "*.conf" > results.txt 2>&1
```
This command searches for all `.conf` files and saves both the output and any error messages to `results.txt`.

## Pipelines and Chaining

You can combine commands using pipelines (`|`), passing the output of one command as input to another:
```bash
command1 | command2
```
For example:
```bash
cat file.txt | grep "pattern"
```
This command displays the contents of `file.txt` and filters the lines containing the word "pattern" using `grep`.

### Example: Sorting and Removing Duplicates

```bash
cat file.txt | sort | uniq
```
This command sorts the contents of `file.txt` and then removes any duplicate lines using `uniq`.

## Conclusion

Input/output redirection is an essential feature in Linux for managing how data flows between commands, files, and devices. It enhances the flexibility of command-line operations, enabling users to control input, output, and error streams with precision.
