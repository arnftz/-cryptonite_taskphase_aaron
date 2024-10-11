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

### 1. Matching with `*`
I began by using the wildcard `*` to match the challenge file. Running the command:
```bash
cd /ch*
```
allowed me to navigate into the `/challenge` folder. I then executed:
```bash
/challenge/run
```
to retrieve the flag.

### 2. Matching with `?`
Next, I utilized the `?` wildcard, replacing characters in the path:
```bash
cd /ch?l?nge
```
This correctly led me to the directory, where I again ran the command to retrieve the flag.

### 3. Matching with `[]`
I moved to the `/files` folder and executed:
```bash
challenge files_[bash]
```
This command allowed me to specify parameters, leading to successful flag retrieval.

### 4. Matching paths with `[]`
Directly executing the command:
```bash
/run /files/file_[bash]
```
effectively used path globbing to retrieve the flag.

### 5. Mixing Globs
I employed mixed globbing by running:
```bash
cd /challenge/flies
/challenge/run [cep]*
```
This command matched any files that started with the letters `c`, `e`, or `p`.

### 6. Exclusionary Globbing
Finally, I used exclusionary globbing to navigate to the files folder:
```bash
cd /files
challenge [^pwn]*
```
This command successfully filtered out any words starting with the letters `p`, `w`, or `n`.

## Practicing Piping

### 7. Redirecting Output
I learned to redirect outputs to files using the `echo` command:
```bash
echo "flag content" > myflag
```
This led to the successful retrieval of the flag.

### 8. Redirecting More Output
By redirecting the output of the run command:
```bash
/challenge/run > myflag
```
I used `cat myflag` to read and verify the flag.

### 9. Appending Output
I appended the output of the run command to the existing content of my flag file:
```bash
/challenge/run >> myflag
```
This ensured I had the complete flag.

### 10. Redirecting Errors
I redirected the output and errors to separate files:
```bash
/challenge/run > myflag 2> instructions
```
Using `cat myflag`, I was able to retrieve the flag.

### 11. Redirecting Input
I created a file named `PWN` containing the word "college" and redirected its content into the command:
```bash
/challenge/run < PWN
```
This successfully retrieved the flag.

### 12. Grepping Errors
To handle standard error, I combined stderr and stdout:
```bash
/challenge/run 2>&1 | grep pwn.college
```
This allowed me to filter for the flag effectively.

### 13. Grepping Stored Results
After redirecting gibberish output to `tmp data.txt`, I searched for the flag pattern:
```bash
grep pwn.college tmp data.txt
```
This led to a successful flag discovery.

### 14. Duplicate Piped Data with `tee`
I used the `tee` command to duplicate output for debugging:
```bash
/challenge/run | tee err.txt | cat
```
This process helped me find a secret key that was instrumental in completing the challenge.

### 15. Writing to Multiple Programs
I redirected the output of the `hack` command to two files simultaneously using process substitution:
```bash
/challenge/hack | tee >(cat > the) | tee >(cat > planet)
```
This allowed for efficient data management.

### 16. Split Piping stderr and stdout
In a more complex command, I piped errors and standard output to different commands:
```bash
/challenge/hack 2>&1 | tee >(cat > the) | /challenge/hack | tee >(cat > planet)
```
This demonstrated advanced use of piping and redirection.

# Shell Variables

## 1. Printing Variables
**Objective:** Print the value of the `$FLAG` variable.

- **Steps:**
  1. Use `echo` to print the `$FLAG` variable.

- **Command:**
  ```bash
  echo $FLAG
  ```

- **Flag:**  
  `pwn.college{Yg5L21CmG9_fTwMK0Ytm7r8LtMO.ddTN1QDL0czN0czW}`

---

## 2. Setting Variables
**Objective:** Assign the value `COLLEGE` to the variable `PWN`.

- **Steps:**
  1. Assign `COLLEGE` to the variable `PWN`.

- **Command:**
  ```bash
  PWN=COLLEGE
  ```

- **Flag:**  
  `pwn.college{EFN-Tlo0VEvKCXjGM6VfJePd_d7.dlTN1QDL0czN0czW}`

---

## 3. Multi-Word Variables
**Objective:** Assign a multi-word value to a variable using quotes.

- **Steps:**
  1. Assign the value `COLLEGE YEAH` to the variable `PWN` using quotes.

- **Command:**
  ```bash
  PWN="COLLEGE YEAH"
  ```

- **Flag:**  
  `pwn.college{Q4OeINjoy_rB1jR4MFOkLh-3l2o.dBjN1QDL0czN0czW}`

---

## 4. Exporting Variables
**Objective:** Export a variable and verify its availability.

- **Steps:**
  1. Assign `COLLEGE` to the `PWN` variable.
  2. Export the `PWN` variable.
  3. Assign `PWN` to `COLLEGE`.
  4. Run the `/challenge/run` command.

- **Commands:**
  ```bash
  PWN=COLLEGE 
  export PWN
  COLLEGE=PWN
  /challenge/run
  ```

- **Flag:**  
  `pwn.college{MKMxGt3pvvDScEzdLxbrq7WB5OK.dJjN1QDL0czN0czW}`

---

## 5. Printing Exported Variables
**Objective:** Print all environment variables and filter for `FLAG`.

- **Steps:**
  1. Run the `env` command to list all variables.
  2. Optionally, pipe output to `grep` to find `FLAG`.

- **Commands:**
  ```bash
  env
  # or
  env | grep FLAG
  ```

- **Flag:**  
  `pwn.college{I7VSkYwBRJwRMfGXcO20yVa1Tc1.dhTN1QDL0czN0czW}`

---

## 6. Storing Command Output
**Objective:** Store the output of a command in a variable.

- **Steps:**
  1. Use command substitution to assign the output of `/challenge/run` to `PWN`.
  2. Use `echo` to print the value of `FLAG` inside `$PWN`.

- **Commands:**
  ```bash
  PWN=$(/challenge/run)
  echo $PWN
  ```

- **Flag:**  
  `pwn.college{oFuYMYkaVxIfdBVRf07AaDAUsKZ.dVzN0UDL0czN0czW}`

---

## 7. Reading Input
**Objective:** Read user input into a variable.

- **Steps:**
  1. Use the `read` command with `PWN` as the argument.
  2. Input the value `COLLEGE`.

- **Commands:**
  ```bash
  read PWN
  # Input: COLLEGE
  ```

- **Flag:**  
  `pwn.college{UUMNemt7WGHenfCRL-u3dDieRCd.dhzN1QDL0czN0czW}`

---

## 8. Reading Files
**Objective:** Read the contents of a file into a variable.

- **Steps:**
  1. Use the `read` command with the `PWN` variable as the parameter, using `<` to input the contents of `read_me`.

- **Command:**
  ```bash
  read PWN < /challenge/read_me
  ```

- **Flag:**  
  `pwn.college{cpD_2n0uKrWtqczhx5yIWq3tOk2.dBjM4QDL0czN0czW}`

---
