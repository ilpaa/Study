# Pattern Matching in Linux

Pattern matching, also known as globbing, is a powerful feature in Linux that allows you to specify patterns to match file names and other string data. This guide covers the basics of pattern matching in Linux.

## Wildcards

Wildcards are special characters that can be used to represent one or more characters in a file name or string.

## Wildcard Table

| Wildcard         | Description                                                   | Example Usage                             | Matches                   | Does Not Match           |
|------------------|---------------------------------------------------------------|-------------------------------------------|---------------------------|--------------------------|
| `*`              | Matches zero or more characters                               | `ls *.txt`                                | `file.txt`, `report.txt`  | `filetxt`, `txtfile`     |
| `?`              | Matches exactly one character                                 | `ls file?.txt`                            | `file1.txt`, `file2.txt`  | `file10.txt`, `file.txt` |
| `[]`             | Matches any one of the enclosed characters                    | `ls file[123].txt`                        | `file1.txt`, `file2.txt`  | `file4.txt`, `file12.txt`|
| `[a-d]`          | Matches any one character in the specified range (a to d)     | `ls file[a-d].txt`                        | `filea.txt`, `fileb.txt`  | `filee.txt`, `fileg.txt` |
| `[^]`            | Matches any one character not enclosed                        | `ls file[^0-9].txt`                       | `filea.txt`, `fileb.txt`  | `file1.txt`, `file2.txt` |
| `{}`             | Matches any one of the comma-separated patterns               | `ls file{1,2,3}.txt`                      | `file1.txt`, `file2.txt`  | `file4.txt`, `file12.txt`|
| `[[::anumeric]]` | Matches any alphanumeric character                            | `ls file[[::anumeric]].txt`               | `file1.txt`, `filea.txt`  | `file-!.txt`, `file#.txt`|
| `[[::uppercase]]`| Matches any uppercase letter                                  | `ls file[[::uppercase]].txt`              | `fileA.txt`, `fileB.txt`  | `filea.txt`, `file1.txt` |

## Wildcard Usage and Examples

### `*` (Asterisk)

- **Function**: Matches zero or more characters.
- **Usage**: 
  ```bash
  ls *.txt
  ```
  This command lists all files with a `.txt` extension.

#### Example
```bash
ls *file*
```
This command matches any file name containing the word "file".

### `?` (Question Mark)

- **Function**: Matches exactly one character.
- **Usage**:
  ```bash
  ls file?.txt
  ```
  This command matches files like `file1.txt`, `file2.txt`, etc., but not `file10.txt`.

#### Example
```bash
ls document?.pdf
```
This command matches `document1.pdf`, `document2.pdf`, etc.

### `[]` (Square Brackets)

- **Function**: Matches any one of the enclosed characters.
- **Usage**:
  ```bash
  ls file[123].txt
  ```
  This command matches `file1.txt`, `file2.txt`, and `file3.txt`.

#### Example
```bash
ls report[1-3].pdf
```
This command matches `report1.pdf`, `report2.pdf`, and `report3.pdf`.

### `[a-d]` (Character Range)

- **Function**: Matches any one character in the specified range (a to d).
- **Usage**:
  ```bash
  ls file[a-d].txt
  ```
  This command matches `filea.txt`, `fileb.txt`, `filec.txt`, and `filed.txt`.

#### Example
```bash
ls report[a-c].pdf
```
This command matches `reporta.pdf`, `reportb.pdf`, and `reportc.pdf`.

### `[^]` (Negation)

- **Function**: Matches any one character not enclosed.
- **Usage**:
  ```bash
  ls file[^0-9].txt
  ```
  This command matches files like `filea.txt`, `fileb.txt`, but not `file1.txt`.

#### Example
```bash
ls doc[^1-3].pdf
```
This command matches `doc4.pdf`, `doc5.pdf`, etc., but not `doc1.pdf`, `doc2.pdf`, or `doc3.pdf`.

### `{}` (Braces)

- **Function**: Matches any one of the comma-separated patterns.
- **Usage**:
  ```bash
  ls file{1,2,3}.txt
  ```
  This command matches `file1.txt`, `file2.txt`, and `file3.txt`.

#### Example
```bash
ls report_{final,draft,review}.pdf
```
This command matches `report_final.pdf`, `report_draft.pdf`, and `report_review.pdf`.

### `[[::anumeric]]` (Alphanumeric)

- **Function**: Matches any alphanumeric character.
- **Usage**:
  ```bash
  ls file[[::anumeric]].txt
  ```
  This command matches `file1.txt`, `filea.txt`, etc.

#### Example
```bash
ls doc[[::anumeric]].pdf
```
This command matches `doc1.pdf`, `doca.pdf`, etc.

### `[[::uppercase]]` (Uppercase)

- **Function**: Matches any uppercase letter.
- **Usage**:
  ```bash
  ls file[[::uppercase]].txt
  ```
  This command matches `fileA.txt`, `fileB.txt`, etc.

#### Example
```bash
ls doc[[::uppercase]].pdf
```
This command matches `docA.pdf`, `docB.pdf`, etc.

## Examples of Combining Wildcards

### Combining `*` and `?`
```bash
ls *file?.txt
```
This command matches files like `myfile1.txt`, `yourfile2.txt`, etc.

### Combining `[]` and `*`
```bash
ls report[0-9]*.txt
```
This command matches `report1.txt`, `report2_final.txt`, etc.

### Combining `{}` and `*`
```bash
ls *.{jpg,png,gif}
```
This command matches all files with `.jpg`, `.png`, and `.gif` extensions.

## Conclusion

Pattern matching is a versatile feature in Linux that allows you to efficiently manage and manipulate files. By mastering wildcards and their combinations, you can perform complex file operations with ease.

For more detailed information, refer to the shell documentation by typing:
```bash
man bash
```
