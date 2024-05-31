# `grep` Command in Linux

## Overview

The `grep` command is used to search text using patterns. It stands for "Global Regular Expression Print" and is one of the most powerful and frequently used commands in Linux for text processing.

## Basic Usage

### Search for a Pattern in a File

To search for a specific pattern in a file, use:
```bash
grep "pattern" filename
```

Replace `"pattern"` with the text you are searching for and `filename` with the name of the file.

## Command Options

- `-i`: Ignore case distinctions.
- `-v`: Invert the match, displaying lines that do not match the pattern.
- `-c`: Count the number of matching lines.
- `-l`: Display the names of files with matching lines.
- `-n`: Display line numbers with output lines.
- `-r`: Read all files under each directory recursively.
- `-w`: Match whole words only.
- `-o`: Show only the part of a line matching the pattern.
- `--color=auto`: Highlight the matching text.

## Examples

### Case-Insensitive Search

To search for a pattern without considering case sensitivity, use:
```bash
grep -i "pattern" filename
```

### Invert Match

To display lines that do not match the pattern, use:
```bash
grep -v "pattern" filename
```

### Count Matches

To count the number of matching lines, use:
```bash
grep -c "pattern" filename
```

### Display File Names with Matches

To display the names of files containing the pattern, use:
```bash
grep -l "pattern" filename
```

### Display Line Numbers

To display line numbers along with matching lines, use:
```bash
grep -n "pattern" filename
```

### Recursive Search

To search for a pattern recursively in all files under a directory, use:
```bash
grep -r "pattern" directory
```

### Match Whole Words

To match only whole words, use:
```bash
grep -w "pattern" filename
```

### Show Only Matching Part of Line

To show only the part of a line that matches the pattern, use:
```bash
grep -o "pattern" filename
```

### Highlight Matching Text

To highlight the matching text, use:
```bash
grep --color=auto "pattern" filename
```

## Examples

### Search for a Specific Word in a File

```bash
grep "error" log.txt
```
This command searches for the word "error" in the file `log.txt` and outputs the matching lines.

### Case-Insensitive Search for a Word

```bash
grep -i "warning" log.txt
```
This command searches for the word "warning" in the file `log.txt` without considering case.

### Count the Number of Matches

```bash
grep -c "failure" log.txt
```
This command counts the number of lines containing the word "failure" in the file `log.txt`.

### Search Recursively in a Directory

```bash
grep -r "timeout" /var/log
```
This command searches for the word "timeout" in all files under the `/var/log` directory recursively.

### Display Lines That Do Not Match

```bash
grep -v "success" log.txt
```
This command displays all lines in `log.txt` that do not contain the word "success".

### Display Line Numbers

```bash
grep -n "critical" log.txt
```
This command searches for the word "critical" in the file `log.txt` and displays the matching lines with line numbers.

### Match Whole Words Only

```bash
grep -w "done" script.sh
```
This command searches for the whole word "done" in the file `script.sh`.

### Show Only Matching Part of Line

```bash
grep -o "http.*" urls.txt
```
This command extracts and displays only the parts of the lines in `urls.txt` that match the pattern "http*".

### Highlight Matching Text

```bash
grep --color=auto "session" config.cfg
```
This command highlights the word "session" in the output from the file `config.cfg`.

## Running a Command from History

To run a previous `grep` command from your history, use:
```bash
!grep
```
This will execute the last command that starts with `grep` from your command history.

## Conclusion

The `grep` command is an essential tool for text processing in Linux. It provides a wide range of options to customize your search and make it more efficient. For more detailed information, refer to the `grep` man page by typing:
```bash
man grep
```
