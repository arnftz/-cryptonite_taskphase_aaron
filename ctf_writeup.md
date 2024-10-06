# CTF Writeup

## Intro to Commands
- **Command:** `hello`  
  **Action:** Directly executed to retrieve the flag.

## Intro to Arguments
- **Command:** `hello hackers`  
  **Action:** Used the command with an argument to retrieve the flag.

## Pondering Paths

1. **The Root**  
   **Command:** `/pwn`  
   **Action:** Invoked `pwn` using its absolute path from the root to retrieve the flag.

2. **Programs and Absolute Paths**  
   **Command:** `/challenge/run`  
   **Action:** Ran the `run` command located in the challenge folder using its absolute path to obtain the flag.

3. **Position Thy Self**  
   **Command:** `cd <path>` then `run`  
   **Action:** Changed directory to the specified path and executed the `run` command to retrieve the flag.

4. **Position Elsewhere**  
   **Command:** `cd <path>` then `run`  
   **Action:** Changed directory to another specified path and executed the `run` command to retrieve the flag.

5. **Position Yet Elsewhere**  
   **Command:** `cd /` then `run`  
   **Action:** Repositioned to the root directory, guessed the file location, and ran the command to retrieve the flag.

6. **Implicit Relative Paths from /**  
   **Command:** `cd /` then `challenge/run`  
   **Action:** Changed to the root directory and used a relative path to retrieve the flag.

7. **Explicit Relative Paths from /**  
   **Command:** `cd /` then `./challenge/run`  
   **Action:** Explicitly called the relative path to retrieve the flag.

8. **Implicit Relative Path**  
   **Command:** `cd /challenge` then `./run`  
   **Action:** Used the absolute path and repeated the explicit relative path to retrieve the flag.

9. **Home Sweet Home**  
   **Command:** `/challenge/run ~/a`  
   **Action:** Executed the command with a file in the home directory using its absolute path as a parameter.

## Comprehending Commands

- **Cat: Not the Pet, but the Command**  
  **Command:** `cat flag`  
  **Action:** Read the flag file output to the terminal.

- **Catting Absolute Paths**  
  **Command:** `cat /flag`  
  **Action:** Retrieved contents of the flag file using its absolute path.

- **More Catting Practices**  
  **Command:** `cat <absolute_path>`  
  **Action:** Automatically received the path and retrieved the flag.

- **Grep for a Needle in a Haystack**  
  **Command:** `grep pwn.college /challenge/data.txt`  
  **Action:** Searched for the key in the specified file.

- **Listing Files**  
  **Command:** `cd /challenge` then `ls`  
  **Action:** Listed files to find the `run` command, then executed it to retrieve the flag.

- **Touching Files**  
  **Commands:**  
  ```bash
  touch /tmp/pwn
  touch /tmp/college
  /challenge/run
  ```  
  **Action:** Created files and ran the command to retrieve the flag.

- **Removing Files**  
  **Command:** `rm ~/file` then `/challenge/check`  
  **Action:** Removed a file and retrieved the flag.

- **Hidden Files**  
  **Command:** `ls -a`  
  **Action:** Found and accessed a hidden flag file in the root.

- **An Epic Filesystem Quest**  
  **Action:** Navigated through various directories using `cd`, copied paths, and retrieved the flag after extensive searching.

- **Making Directories**  
  **Command:**  
  ```bash
  mkdir -p /absolute/path/to/dir
  touch /absolute/path/to/dir/college
  /challenge/check
  ```  
  **Action:** Created directories and files to retrieve the flag.

- **Finding Files**  
  **Command:** `find / -name "flag"`  
  **Action:** Located all instances of the flag file and retrieved it.

- **Linking Files**  
  **Command:**  
  ```bash
  ln -s /home/hacker/not-the-flag /flag
  cat /flag
  ```  
  **Action:** Created a symlink and accessed the flag.

## Digesting Documentation

- **Learning from Documentation**  
  **Command:** `giveflag`  
  **Action:** Retrieved output using a simple command and its parameter.

- **Learning Complex Usage**  
  **Command:** `giveflag --printfile flag`  
  **Action:** Found the flag file in the root and executed the command.

- **Reading Manuals**  
  **Command:** `man challenge`  
  **Action:** Checked usage and parameters to retrieve the flag.

- **Searching Manuals**  
  **Command:** `/flag` in `man challenge`  
  **Action:** Found the `-d` option and retrieved the flag.

- **Searching FOR Manuals**  
  **Command:** `man -k <keyword>`  
  **Action:** Discovered hidden randomized manpage entries.

- **Helpful Programs**  
  **Command:** `/challenge/challenge --help`  
  **Action:** Found the secret number and flag using provided commands.

- **Help for Bulletins**  
  **Command:** `man challenge`  
  **Action:** Found the secret value, passed it as a parameter, and obtained the key.

## File Globbing

- **Matching with \***  
  **Command:** `cd /ch*`  
  **Action:** Navigated to the `/challenge` folder and executed the `run` command.

- **Matching with ?**  
  **Command:** `cd ?hallenge`  
  **Action:** Accessed the directory and retrieved the flag.

- **Matching with []**  
  **Command:** `cd /files/files_[bash]`  
  **Action:** Retrieved the flag using the challenge command.

- **Matching Paths with []**  
  **Command:** `/files/file_[bash]`  
  **Action:** Executed the `run` command and retrieved the flag.

## Practicing Piping

- **Redirecting Output**  
  **Command:** `echo "output" > myflag`  
  **Action:** Learned to redirect outputs to files.

- **Redirecting More Output**  
  **Command:** `run > myflag`  
  **Action:** Redirected output of the run command to a file and accessed it.

- **Appending Output**  
  **Command:** `run >> myflag`  
  **Action:** Appended output to an existing file to complete the flag.

- **Redirecting Errors**  
  **Command:** `run > myflag 2> instructions`  
  **Action:** Redirected output and errors, then retrieved the flag.

- **Redirecting Input**  
  **Command:** `echo "college" > PWN; /challenge/run < PWN`  
  **Action:** Redirected input to retrieve the flag.

- **Grepping Stored Results**  
  **Command:** `grep pwn.college /tmp/data.txt`  
  **Action:** Searched for flag patterns in a data file.

- **Duplicate Piped Data with Tee**  
  **Command:** `run | tee err`  
  **Action:** Duplicated output for debugging and retrieved the secret key. 

This write-up encapsulates the journey through the challenges, detailing commands and strategies used to retrieve flags along the way.
