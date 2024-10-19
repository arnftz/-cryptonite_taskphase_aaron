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

# Processes and Jobs

## Challenge 1: Listing Processes

**Steps:**
1. List all processes using the `ps aux` command.
2. Pipe the output and use `grep` to find the word "challenge".
3. Copy the renamed command and execute it.

**Commands:**
```bash
ps aux | grep /challenge
/challenge/30460-run-15236
```

**Flag:** 
```
pwn.college{0IVwhCBWuhtyxTlxkEYkRVcUhkC.dhzM4QDL0czN0czW}
```

---

## Challenge 2: Killing Processes

**Steps:**
1. List all processes using the `ps aux` command.
2. Pipe the output and use `grep` to find the word "dont_run".
3. Identify the PID and kill it.
4. Execute the run command.

**Commands:**
```bash
ps aux | grep /challenge
kill 73
/challenge/run
```

**Flag:** 
```
pwn.college{8AH_eLDBLBPFjhLzOHOg9x6Q7LW.dJDN4QDL0czN0czW}
```

---

## Challenge 3: Interrupting Processes

**Steps:**
1. Execute the run command.
2. Exit the process using Ctrl-C.

**Commands:**
```bash
/challenge/run
# Ctrl-C
```

**Flag:** 
```
pwn.college{AVfob4WVEHpkAqkRm1XmRwQaGZC.dNDN4QDL0czN0czW}
```

---

## Challenge 4: Suspending Processes

**Steps:**
1. Execute the run command.
2. Suspend the process using Ctrl-Z.
3. Execute the run command again.

**Commands:**
```bash
/challenge/run
# Ctrl-Z
/challenge/run
```

**Flag:** 
```
pwn.college{Yb8K5J_AvVMLS-1cR8zhmpjiP_w.dVDN4QDL0czN0czW}
```

---

## Challenge 5: Resuming Processes

**Steps:**
1. Execute the run command.
2. Suspend the process using Ctrl-Z.
3. Restart the process using `fg`.

**Commands:**
```bash
/challenge/run
# Ctrl-Z
fg
```

**Flag:** 
```
pwn.college{4LvMxl3-vO6YhkUBd3OEzynZgES.dZDN4QDL0czN0czW}
```

---

## Challenge 6: Backgrounding Processes

**Steps:**
1. Execute the run command.
2. Suspend the process using Ctrl-Z.
3. Restart the process in the background using `bg`.
4. Execute the run command again.

**Commands:**
```bash
/challenge/run
# Ctrl-Z
bg
/challenge/run
```

**Flag:** 
```
pwn.college{0XW-2eMsYmPb_My5AQ5PcoJhUIF.ddDN4QDL0czN0czW}
```

---

## Challenge 7: Foregrounding Processes

**Steps:**
1. Execute the run command.
2. Suspend the process using Ctrl-Z.
3. Restart the process in the background using `bg`.
4. Bring the process back to the foreground using `fg`.
5. Hit enter to get the flag.

**Commands:**
```bash
/challenge/run
# Ctrl-Z
bg
fg
# ENTER
```

**Flag:** 
```
pwn.college{ojCtFRtdpWutWTzbfgPDNHJ0wTt.dhDN4QDL0czN0czW}
```

---

## Challenge 8: Starting Background Processes

**Steps:**
1. Execute the run command with `&` to run it in the background.

**Commands:**
```bash
/challenge/run &
```

**Flag:** 
```
pwn.college{YJi_gcP-E-nPAo1soFuU1VYCU2M.dlDN4QDL0czN0czW}
```

---

## Challenge 9: Process Exit Codes

**Steps:**
1. Execute the `get-code` command.
2. Find the exit code using `echo $?`.
3. Input the found code to the `submit-code` command.

**Commands:**
```bash
/challenge/get-code
echo $?
/challenge/submit-code 198
```

**Flag:** 
```
pwn.college{oQfeapHxddKrKh8zcwFSD2iWyOG.dljN4UDL0czN0czW}
``` 

---

# Perceiving Permissions

## Challenge 1: Changing File Ownership

**Steps:**
1. Use the `chown` command to give the user `hacker` ownership of the `/flag` file.
2. Use the `cat` command with `/flag` as the parameter to read the flag.

**Commands:**
```bash
chown hacker /flag
cat /flag
```

**Flag:** 
```
pwn.college{s5LePg1zD0IjpjBQNMFeXXEdGXZ.dFTM2QDL0czN0czW}
```

---

## Challenge 2: Groups and Files

**Steps:**
1. Use the `chgrp` command to change the group ownership of the `/flag` file to `hacker`.
2. Use the `cat` command with `/flag` as the parameter to read the flag.

**Commands:**
```bash
chgrp hacker /flag
cat /flag
```

**Flag:** 
```
pwn.college{kWZEqPq6zY5qIO8TwVCpLMikzv0.dFzNyUDL0czN0czW}
```

---

## Challenge 3: Fun With Group Names

**Steps:**
1. Use the `id` command to get the group ID.
2. Use the `chgrp` command to give the group ownership of the `/flag` file with the newly found group name.
3. Use the `cat` command with `/flag` as the parameter to read the flag.

**Commands:**
```bash
id
chgrp grp6337 /flag
cat /flag
```

**Flag:** 
```
pwn.college{03Ui-9zSVRmholTi7eVKPdpIzwc.dJzNyUDL0czN0czW}
```

---

## Challenge 4: Changing Permissions

**Steps:**
1. Use the `chmod` command to give all users, groups, and others read, write, and execute permissions for `/flag`.
2. Use the `cat` command with `/flag` as the parameter to read the flag.

**Commands:**
```bash
chmod a+rwx /flag
cat /flag
```

**Flag:** 
```
pwn.college{8gNlF6hkZxwAMqKnKZg7_UiFpwC.dNzNyUDL0czN0czW}
```

---

## Challenge 5: Executable Files

**Steps:**
1. Use the `chmod` command to give users execute permission for the `run` command.
2. Execute the `run` command.

**Commands:**
```bash
chmod u+x /challenge/run
/challenge/run
```

**Flag:** 
```
pwn.college{ImrY7YMCB1cbAZTFVsF7OkUF9sR.dJTM2QDL0czN0czW}
```

---

## Challenge 6: Permission Tweaking Practice

**Steps:**
1. Execute the `run` command to start the challenge.
2. Remove read permissions for the world.
3. Remove all permissions for everyone.
4. Give write permissions to the world.
5. Remove write permission for the world.
6. Give write permissions to groups and the world.
7. Give execute permissions to groups.
8. Give read and execute permissions to the world.
9. Remove read and execute permissions from the world.
10. Use the `chmod` command to give read access to the user.
11. Use the `cat` command with `/flag` as the parameter to read the flag.

**Commands:**
```bash
/challenge/run
chmod o-r /challenge/pwn
chmod a-rwx /challenge/pwn 
chmod o+w /challenge/pwn
chmod o-w /challenge/pwn
chmod go+w /challenge/pwn 
chmod g+x /challenge/pwn
chmod o+rx /challenge/pwn
chmod o-rx /challenge/pwn
chmod u+r /flag 
cat /flag
```

**Flag:** 
```
pwn.college{ImrY7YMCB1cbAZTFVsF7OkUF9sR.dJTM2QDL0czN0czW}
```

---

## Challenge 7: Permission Setting Practice

**Steps:**
1. Execute the `run` command to start the challenge.
2. Set permissions for users, groups, and others in a sequence.
3. Use the `chmod` command to give read access to the user.
4. Use the `cat` command with `/flag` as the parameter to read the flag.

**Commands:**
```bash
/challenge/run
chmod u=x,g=wx,o=w /challenge/pwn
chmod u=wx,g=w,o=rx /challenge/pwn
chmod u=-,g=rwx,o=rx /challenge/pwn
chmod u=w,g=rx,o=rw /challenge/pwn
chmod u=rw,g=-,o=wx /challenge/pwn
chmod u=wx,g=-,o=rx /challenge/pwn
chmod u=rx,g=x,o=- /challenge/pwn
chmod u=rx,g=x,o=- /challenge/pwn
chmod u+r /flag 
cat /flag
```

**Flag:** 
```
pwn.college{MkHhSbZaAIz623fKjCOT_kp5_38.dNTM5QDL0czN0czW}
```

---

## Challenge 8: The SUID Bit

**Steps:**
1. Use the `chmod` command to give users the SUID permission for the `/challenge/get_root` command.
2. Execute the `get_root` command.
3. Use the `cat` command with `/flag` as the parameter to read the flag.

**Commands:**
```bash
chmod u+s /challenge/get_root
/challenge/get_root
cat /flag
```

**Flag:** 
```
pwn.college{ot_PBsU6mRSYn_X6Fk5VU-3yzks.dNTM2QDL0czN0czW}
```

---

# Untangling Users

## Becoming Root With su

**Steps:**
1. Use the `su` command to switch to the root user.
2. Enter the password `hack-the-planet`.
3. Use the `cat` command with `/flag` as the parameter to read the flag.

**Commands:**
```bash
su
hack-the-planet
cat /flag
```

**Flag:** 
```
pwn.college{0SStI5mkoFwSeZuLOvIEiOKRzHu.dVTN0UDL0czN0czW}
```

---

## Other Users With su

**Steps:**
1. Use the `su` command with `zardus` as the parameter to specify the user.
2. Enter the password `dont-hack-me`.
3. Execute the `run` command upon login.

**Commands:**
```bash
su zardus
dont-hack-me
/challenge/run
```

**Flag:** 
```
pwn.college{McEP6revcXQ2H-jurnx9hYfDQHD.dZTN0UDL0czN0czW}
```

---

## Cracking Passwords

**Steps:**
1. Use the `john` command with the leak file as the parameter to find the password for `zardus`.
2. Use the `su` command with `zardus` as the parameter and enter the found password.
3. Execute the `run` command.

**Commands:**
```bash
john /challenge/shadow-leak
su zardus
aadvark
/challenge/run
```

**Flag:** 
```
pwn.college{EAYcNbCCtDhzRlPokD5vpF91qyG.ddTN0UDL0czN0czW}
```

---

## Using Sudo

**Steps:**
1. Use the `sudo` command to read the `/flag` file using `cat`.

**Commands:**
```bash
sudo cat /flag
```

**Flag:** 
```
pwn.college{oPBk71P08goBNOJFjBCrL62AlTi.dhTN0UDL0czN0czW}
```

---

# Chaining Commands

## Chaining with Semicolons

**Steps:**
1. Use a semicolon to chain both `pwn` and `college` commands.

**Commands:**
```bash
/challenge/pwn ; /challenge/college
```

**Flag:** 
```
pwn.college{srp7_wWUYt7MNfzlyqJOkrbvOcM.dVTN4QDL0czN0czW}
```

---

## Your First Shell Script

**Steps:**
1. Use the `touch` command to create a bash file `x.sh`.
2. Use the `echo` command with the required chained commands as a parameter in quotes and redirect the output to `x.sh`.
3. Execute `x.sh` using the `bash` command.

**Commands:**
```bash
touch x.sh
echo "/challenge/pwn ; /challenge/college" > x.sh
bash x.sh
```

**Flag:** 
```
pwn.college{gQYgDg3mQlp_8uSuXHx8qpapPZh.dFzN4QDL0czN0czW}
```

---

## Redirecting Script Output

**Steps:**
1. Use the `touch` command to create a bash file `x.sh`.
2. Use the `echo` command with the required chained commands as a parameter in quotes and redirect the output to `x.sh`.
3. Execute `x.sh` using the `bash` command and pipe the output to the `solve` command.

**Commands:**
```bash
touch x.sh
echo "/challenge/pwn ; /challenge/college" > x.sh
bash x.sh | /challenge/solve
```

**Flag:** 
```
pwn.college{oQk7wlXRPX2Ms-oEsZHwcTwPkQp.dhTM5QDL0czN0czW}
```

---

## Executable Shell Scripts

**Steps:**
1. Use the `touch` command to create a bash file `x.sh`.
2. Use the `echo` command with the required command as a parameter in quotes and redirect the output to `x.sh`.
3. Use `chmod` to modify user permissions for the file to be executable.
4. Execute the `./x.sh` file.

**Commands:**
```bash
touch x.sh
echo "/challenge/solve" > x.sh
chmod u=rwx x.sh
./x.sh
```

**Flag:** 
```
pwn.college{QqXU_155Sfvn84JPxux-hm6zHpC.dRzNyUDL0czN0czW}
```

---

# Pondering PATH

## The PATH Variable

**Steps:**
1. Rewrite `PATH` to only include `/run/challenge/bin`.
2. Execute the `run` command.

**Commands:**
```bash
PATH="/run/challenge/bin"
/challenge/run
```

**Flag:** 
```
pwn.college{g3YOKR_rxQmGgGlX-2j5mqzuDBl.dZzNwUDL0czN0czW}
```

---

## Setting PATH

**Steps:**
1. Rewrite `PATH` to include the `more_commands` folder.
2. Execute the `run` command.

**Commands:**
```bash
PATH="/challenge/more_commands"
/challenge/run
```

**Flag:** 
```
pwn.college{YTPsd9l_1RzLg36F8rVe0Yf-olI.dVzNyUDL0czN0czW}
```

---

## Adding Commands

**Steps:**
1. Use the `touch` command to create a file in home called `win`.
2. Use the `echo` command to write `read /flag` into the `win` file.
3. Use `chmod` to make it executable.
4. Add the home/hacker path to the `PATH` variable.
5. Execute the `run` command.

**Commands:**
```bash
touch win
echo "read /flag" > win
chmod a=rwx win
PATH='~'
/challenge/run
```

**Flag:** 
```
pwn.college{ou0iXM8lGuXvL71yiVFCcOHDLVD.dZzNyUDL0czN0czW}
```

---

## Hijacking Commands

**Steps:**
1. Use the `touch` command to create a file in home called `rm`.
2. Use the `echo` command to write `cat /flag` into the `rm` file.
3. Use `chmod` to make it executable.
4. Add the home/hacker path to the beginning of the `PATH` variable.
5. Execute the `run` command.

**Commands:**
```bash
touch rm
echo "cat /flag" > rm
chmod a=rwx rm
PATH="~:/run/challenge/bin:/run/workspace/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
/challenge/run
```

**Flag:** 
```
pwn.college{wNe8MC6w-rGcTlBVbBASMAFX_WR.ddzNyUDL0czN0czW}
```
