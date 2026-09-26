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
