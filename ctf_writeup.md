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
