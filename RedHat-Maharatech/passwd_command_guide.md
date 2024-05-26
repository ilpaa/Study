# `passwd` Command in Linux

## Overview
The `passwd` command is used to change the password of a user account in Linux. It can be used by both regular users and administrators to update their own passwords or to reset passwords for other users.

## Basic Usage

### Changing Your Own Password
To change your own password, simply type:
```bash
passwd
```
You will be prompted to enter your current password, followed by your new password (twice for confirmation).

### Changing Another User's Password (as root)
To change the password for another user (requires root privileges), use the following syntax:
```bash
sudo passwd <username>
```
Replace `<username>` with the actual username of the account you wish to modify. You will be prompted to enter and confirm the new password for the specified user.

## Command Options

- `--help`: Displays the help message.
- `-d, --delete`: Deletes the password for the account, making it passwordless.
- `-e, --expire`: Forces the user to change their password at the next login.
- `-i, --inactive <days>`: Sets the number of days after a password expires until the account is permanently disabled.
- `-l, --lock`: Locks the user's account.
- `-n, --mindays <days>`: Sets the minimum number of days between password changes.
- `-q, --quiet`: Operates quietly.
- `-r, --repository <repo>`: Uses the specified repository.
- `-S, --status`: Displays account status information.
- `-u, --unlock`: Unlocks the user's account.
- `-w, --warndays <days>`: Sets the number of days of warning before a password change is required.
- `-x, --maxdays <days>`: Sets the maximum number of days a password remains valid.

## Examples

### Change Own Password
```bash
passwd
```
You will be prompted to:
1. Enter your current password.
2. Enter your new password.
3. Confirm your new password.

### Change Another User's Password
```bash
sudo passwd john
```
Replace `john` with the username of the account whose password you want to change. You will be prompted to enter and confirm the new password for `john`.

### Lock a User Account
```bash
sudo passwd -l jane
```
Locks the account of user `jane`, preventing her from logging in.

### Unlock a User Account
```bash
sudo passwd -u jane
```
Unlocks the account of user `jane`.

### Force Password Expiration
```bash
sudo passwd -e john
```
Forces user `john` to change his password at the next login.

### Set Minimum and Maximum Password Age
```bash
sudo passwd -n 7 -x 90 john
```
Sets the minimum password age to 7 days and the maximum password age to 90 days for user `john`.

## Conclusion
The `passwd` command is a versatile and essential tool for managing user passwords in Linux. Whether you are a regular user or a system administrator, understanding how to use `passwd` can help maintain the security and integrity of user accounts.

For more detailed information, refer to the `passwd` man page by typing:
```bash
man passwd
```
