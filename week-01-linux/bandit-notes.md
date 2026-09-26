# OverTheWire: Bandit Walkthrough & Lab Notes

**Target Server:** `bandit.labs.overthewire.org:2220`  
**Track:** Linux CLI Basics, File System Navigation, and Permissions

---

## Level 0 -> Level 1

* **Goal:** Connect to the remote server via SSH and retrieve the password stored in the home directory to unlock Level 1.
* **Commands Executed:**
  ```bash
  ssh bandit0@bandit.labs.overthewire.org -p 2220
  ls
  cat readme
  ```
- Concepts & Takeaways:

    - Standard SSH listens on TCP port 22. OverTheWire uses port 2220, specified using the -p flag.

    - Standard visible files do not require special flags to read or list.

## Level 1 -> Level 2

* **Goal:** Retrieve the password stored in a file named `-` in the home directory.
* **Commands Executed:**
  ```bash
  ssh bandit1@bandit.labs.overthewire.org -p 2220
  cat ./-
  ```
- Concepts & Takeaways:

    - When a filename begins with a dash (-), command-line tools like cat interpret the argument as an option flag or expect input from stdin.

    - Prepending `./` explicitly specifies a relative file path (`cat ./-`), telling cat to treat the dash as a filename inside the current directory rather than a flag.

## Level 2 -> Level 3

* **Goal:** Retrieve the password stored in a file named `--spaces in this filename--` in the home directory.
* **Commands Executed:**
  ```bash
  ssh bandit2@bandit.labs.overthewire.org -p 2220
  cat ./"--spaces in this filename--"
  # Alternative: cat "./--spaces in this filename--"
  # Alternative: cat -- "--spaces in this filename--"
  # Alternative: cat ./--spaces\ in\ this\ filename--

- Concepts & Takeaways:

    - Bash uses whitespace (spaces) to separate arguments passed to a command. Passing cat spaces in this filename causes cat to search for four separate files (spaces, in, this, and filename).

    - Enclosing the filename in double quotes ("...") or escaping every space with a backslash (\ ) tells the shell to process the entire string as a single filename argument.

## Level 3 -> Level 4

* **Goal:** Retrieve the password stored in a hidden file inside the `inhere` directory.
* **Commands Executed:**
  ```bash
  ssh bandit3@bandit.labs.overthewire.org -p 2220
  cd inhere
  ls -a
  cat ...Hiding-From-You
  # Alternative: cat ./...Hiding-From-You
  # Alternative: cat "...Hiding-From-You"
  # Alternative: cat "./...Hiding-From-You"
  # Alternative: cat ./"./...Hiding-From-You"
  ```
- Concepts & Takeaways:

    - In Linux/Unix systems, any file or directory whose name begins with a dot (.) is considered hidden and will not show up with a standard ls command.

    - The `-a` (all) flag with `ls` (ls -a) is required to reveal hidden files (dotfiles).

## Level 4 -> Level 5

* **Goal:** Find and read the only human-readable (ASCII text) file inside the `inhere` directory.
* **Commands Executed:**
  ```bash
  ssh bandit4@bandit.labs.overthewire.org -p 2220
  cd inhere
  file ./*
  cat ./-file07
  ```
- Concepts & Takeaways:

    - The `file` command inspects file signatures (magic bytes) to determine file types regardless of extension or contents.

    - Combining wildcard expansion (*) with relative path prefixing (./*) allows file to inspect every item in a directory without leading dashes breaking parameter parsing.

## Level 5 -> Level 6

* **Goal:** Find a file under the `inhere` directory tree that is 1033 bytes in size, non-executable, and human-readable.
* **Commands Executed:**
  ```bash
  ssh bandit5@bandit.labs.overthewire.org -p 2220
  cd inhere
  find ./ -type f -size 1033c # Alternative: find ./ -type f -size 1033c ! -executable
  cat inhere/maybehere07/.file2
  ```
- Concepts & Takeaways:

    - The `find` utility recursively searches directory structures based on file attributes rather than text content.

    - `-size 1033c` matches files of exactly 1033 bytes (c specifier).

    - Exclamation mark `!` acts as a logical NOT operator in find queries (e.g., ! -executable).

## Level 6 -> Level 7

* **Goal:** Locate a 33-byte file somewhere on the system owned by user `bandit7` and group `bandit6`.
* **Commands Executed:**
  ```bash
  ssh bandit6@bandit.labs.overthewire.org -p 2220
  find / -user bandit7 -group bandit6 -size 33c #Alternative: find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
  cat /var/lib/dpkg/info/bandit7.password
  ```
- Concepts & Takeaways:

    - `find /` searches recursively starting from the root directory.

    - `-user` and `-group` filter search results by ownership.

    - `2>/dev/null` redirects stderr (file descriptor 2) to the null device, suppressing access/permission errors from cluttering terminal output.

## Level 7 -> Level 8

* **Goal:** Find the password in `data.txt` located next to the word `millionth`.
* **Commands Executed:**
  ```bash
  ssh bandit7@bandit.labs.overthewire.org -p 2220
  grep "millionth" data.txt
  ```
- Concepts & Takeaways:

    - grep (Global Regular Expression Print) searches text files for lines matching a specified string or pattern.

    - Using grep on large datasets eliminates manual terminal scrolling and isolates target data instantly.

## Level 8 -> Level 9

* **Goal:** Find the only line of text in `data.txt` that occurs exactly once.
* **Commands Executed:**
  ```bash
  ssh bandit8@bandit.labs.overthewire.org -p 2220
  sort data.txt | uniq -c
  # Use the -u flag to print only the unique line
  # Alternative: sort data.txt | uniq -u 
  ```
- Concepts & Takeaways:

    - `sort` rearranges lines in text files into alphabetical/numerical order.

    - `uniq` filters duplicate lines, but requires sorted input because it only evaluates adjacent/consecutive matches.

    - The `-u` flag on `uniq` causes it to print only unique lines (suppressing all repeated lines).

    - The pipe operator (`|`) redirects standard output from `sort` directly into standard input for `uniq`.

## Level 9 -> Level 10

* **Goal:** Extract a human-readable string preceded by multiple `=` signs from a binary file (`data.txt`).
* **Commands Executed:**
  ```bash
  ssh bandit9@bandit.labs.overthewire.org -p 2220
  strings data.txt | grep "=="
  ```
- Concepts & Takeaways:

    - strings extracts human-readable ASCII text sequences from binary or non-text files.

    - Piping strings into `grep` allows quick filtering of printable output without corrupting the terminal display with raw binary data.

## Level 10 -> Level 11

* **Goal:** Decode the Base64-encoded data inside `data.txt` to retrieve the password.
* **Commands Executed:**
  ```bash
  ssh bandit10@bandit.labs.overthewire.org -p 2220
  base64 -d data.txt
  ```
- Concepts & Takeaways:

    - Base64 is an encoding scheme that translates data into a 64-character ASCII representation for reliable transmission.

    - The base64 utility decodes files using the `-d` (or `--decode`) flag.

## Level 11 -> Level 12

* **Goal:** Decode text in `data.txt` that has been obfuscated using a ROT13 cipher.
* **Commands Executed:**
  ```bash
  ssh bandit11@bandit.labs.overthewire.org -p 2220
  cat data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-M'
  ```
- Concepts & Takeaways:

    - ROT13 is a classic symmetric substitution cipher shifting letters by 13 positions.

    - The `tr` (translate) command transforms standard input by mapping a source set of characters to a destination set.

## Level 12 -> Level 13

* **Goal:** Reconstruct a binary file from a hex dump and iteratively decompress nested archives (`gzip`, `bzip2`, `tar`) to extract the password.
* **Commands Executed:**
  ```bash
  ssh bandit12@bandit.labs.overthewire.org -p 2220
  mkdir /tmp/workspace_bandit12 && cp data.txt /tmp/workspace_bandit12/ && cd /tmp/workspace_bandit12
  xxd -r data.txt > file1
  file file1
  # Iteratively rename (.gz, .bz2, .tar) and unpack until plain text is reached
  cat <final_file>
  ```
- Concepts & Takeaways:

    - System accounts with restricted write access require temporary scratchpads like `/tmp` to alter or unpack data.

    - `xxd -r` turns a hex dump back into raw binary bytes.

    - The `file` utility acts as a navigator across nested archives by identifying the compression signature regardless of current extension.

## Level 13 -> Level 14

* **Goal:** Use an RSA private key stored in `sshkey.private` to authenticate as `bandit14` via SSH on `localhost` and retrieve the Level 14 password.
* **Commands Executed:**
  ```bash
  ssh bandit13@bandit.labs.overthewire.org -p 2220
  ls -l
  #copy the content of the sshkey.private file and then logout of the session
  logout
  ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
  cat /etc/bandit_pass/bandit14
  ```
- Concepts & Takeaways:

    - Public-key cryptography allows SSH authentication using private keys instead of passwords.

    - The `ssh -i <keyfile>` flag tells SSH to use a specific private key file for authentication.

    - System passwords for all Bandit levels reside in `/etc/bandit_pass/`, protected by standard Linux file permissions.

