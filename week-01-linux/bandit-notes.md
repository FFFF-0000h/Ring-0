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

    - Prepending ./ explicitly specifies a relative file path (cat ./-), telling cat to treat the dash as a filename inside the current directory rather than a flag.

