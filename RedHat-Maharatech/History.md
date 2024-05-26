# `history` Command in Linux

## Overview
The `history` command is used to display the command history list of the current user in a sequential order.

## Basic Usage

### Display Command History
To display the command history, simply type:
```bash
history
```

### Limit Displayed Commands
By default, the `history` command displays the entire command history list. To limit the number of commands displayed, use the `-n` option followed by the desired number of commands:
```bash
history -n <num>
```
Replace `<num>` with the number of commands you want to display.

### Clear Command History
To clear the command history, use the `-c` option:
```bash
history -c
```
This command clears the entire command history list.

### Run a Command from History
Suppose you want to rerun a command from history, you can use the `!` operator followed by the command number. For example, to rerun the command with number 5 from history, you can type:
```bash
!<num>
```
Replace `<num>` with the index of the command you want to run.
This will execute the command indexed `<num>` in the command history.

## Command Options

- `-c`: Clear the history list by deleting all of the entries.
- `-d <offset>`: Delete the history entry at position `<offset>`.
- `-a`: Append the new history lines (history lines entered since the beginning of the current Bash session) to the history file.
- `-n <num>`: Display the last `<num>` lines of the history list.
- `-r`: Read the current history file and append its contents to the history list.
- `-w`: Write the current history list to the history file.
- `-p`: Perform history expansion on each ARG and display the result without storing it in the history list.
- `-s <string>`: Append the `<string>` to the history list as a single entry.
- `-w`: Append the new history lines from memory to the history file when the shell exits.

## Examples

### Display Command History
```bash
history
```
This command will display the entire command history list.

### Limit Displayed Commands
```bash
history -n 10
```
This command will display the last 10 commands in the history list.

### Clear Command History
```bash
history -c
```
This command will clear the entire command history list.

### Run a Command from History
Suppose you want to rerun a command from history, you can use the `!` operator followed by the command number. For example, to rerun the command with number 5 from history, you can type:
```bash
!5
```
This will execute the command indexed as 5 in the command history.

## Conclusion
The `history` command is a useful utility for viewing and managing the command history in Linux. It provides various options for displaying, limiting, and clearing the command history list, making it an essential tool for command-line users.

For more detailed information, refer to the `history` man page by typing:
```bash
man history
```
