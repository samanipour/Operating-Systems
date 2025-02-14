# Introduction to Shell Scripts

---

## 1. Introduction

### 1.1 Overview
- A shell script is a series of commands written in a file that the shell executes as if they were typed into a terminal.
- Shell scripts automate repetitive tasks, simplify complex commands, and facilitate system administration.
- The Bourne shell (`/bin/sh`) is the standard shell, but most Linux distributions use an enhanced version called **bash**.

### 1.2 Objectives
- Learn the basics of writing and running shell scripts.
- Understand quoting, variables, conditionals, loops, and subshells.
- Master common shell utilities like `basename`, `awk`, and `sed`.

---

## 2. Shell Script Basics

### 2.1 Creating a Shell Script
- **Objective**: Create a basic shell script.
- **Steps**:
  1. Open a text editor (e.g., `nano` or `vim`).
  2. Start the script with the shebang line:
      ```sh
      #!/bin/sh
      ```
      - This indicates that `/bin/sh` will interpret the script.
  3. Write the script:
      ```sh
      #!/bin/sh
      echo "Hello, World!"
      ls
      ```
  4. Save the file as `hello.sh`.

### 2.2 Making the Script Executable
- **Objective**: Set the executable permission.
- **Command**:
  ```bash
  chmod +x hello.sh
  ```
- **Explanation**:
  - `chmod +x` adds the executable permission.
  - If you don’t want others to read or execute the script, use:
    ```bash
    chmod 700 hello.sh
    ```

### 2.3 Running the Script
- **Objective**: Execute the shell script.
- **Commands**:
  - If the script is in the current directory:
    ```bash
    ./hello.sh
    ```
  - If the script is in a directory in the PATH:
    ```bash
    hello.sh
    ```
  - Using the full path:
    ```bash
    /home/user/scripts/hello.sh
    ```

---

## 3. Quoting and Literals

### 3.1 Understanding Quotes
- **Objective**: Learn when to use quotes.
- **Types of Quotes**:
  - **Single Quotes** (`'`): Preserve literal value (no variable expansion).
    ```sh
    echo '$HOME'  # Output: $HOME
    ```
  - **Double Quotes** (`"`): Allow variable expansion.
    ```sh
    echo "$HOME"  # Output: /home/username
    ```
  - **Backslash** (`\`): Escape a single character.
    ```sh
    echo "He said, \"Hello!\""
    ```
  - **No Quotes**: Words are split at spaces and tabs.
    ```sh
    echo $HOME
    ```

### 3.2 Special Cases
- **Escape a Single Quote**:
  - Using double quotes:
    ```sh
    echo "It's a beautiful day!"
    ```
  - Using backslash:
    ```sh
    echo 'It'\''s a beautiful day!'
    ```

---

## 4. Special Variables

### 4.1 Using Script Arguments
- **Objective**: Access arguments passed to the script.
- **Special Variables**:
  - `$1`, `$2`, ... : Individual arguments.
  - `$#`: Number of arguments.
  - `$@`: All arguments.
  - `$0`: Script name.
- **Example**:
  ```sh
  #!/bin/sh
  echo "Script name: $0"
  echo "First argument: $1"
  echo "All arguments: $@"
  echo "Number of arguments: $#"
  ```
- **Running the Script**:
  ```bash
  ./script.sh one two three
  ```
- **Output**:
  ```
  Script name: ./script.sh
  First argument: one
  All arguments: one two three
  Number of arguments: 3
  ```

### 4.2 Exit Codes
- **Objective**: Understand exit codes.
- **Explanation**:
  - `$?`: Holds the exit code of the last executed command.
  - `0`: Success.
  - Non-zero: Error.
- **Example**:
  ```sh
  ls /not_exist_dir
  echo "Exit code: $?"
  ```

---

## 5. Conditionals

### 5.1 Using `if` Statements
- **Objective**: Execute commands conditionally.
- **Syntax**:
  ```sh
  if [ condition ]; then
    # commands if true
  else
    # commands if false
  fi
  ```
- **Example**:
  ```sh
  #!/bin/sh
  if [ -f "$1" ]; then
    echo "File exists."
  else
    echo "File does not exist."
  fi
  ```
- **Explanation**:
  - `-f`: Checks if the file exists.

### 5.2 Using `case` Statements
- **Objective**: Match a variable against patterns.
- **Syntax**:
  ```sh
  case $1 in
    start) echo "Starting...";;
    stop) echo "Stopping...";;
    *) echo "Unknown command.";;
  esac
  ```

---

## 6. Loops

### 6.1 `for` Loops
- **Objective**: Iterate over a list of items.
- **Syntax**:
  ```sh
  for var in item1 item2 item3; do
    echo $var
  done
  ```
- **Example**:
  ```sh
  for file in *.txt; do
    echo "Processing $file..."
  done
  ```

### 6.2 `while` Loops
- **Objective**: Repeat commands while a condition is true.
- **Syntax**:
  ```sh
  while [ condition ]; do
    # commands
  done
  ```
- **Example**:
  ```sh
  i=1
  while [ $i -le 5 ]; do
    echo "Count: $i"
    i=$((i+1))
  done
  ```

---

## 7. Command Substitution

### 7.1 Using Command Output in Scripts
- **Objective**: Capture the output of a command.
- **Syntax**:
  ```sh
  var=$(command)
  ```
- **Example**:
  ```sh
  DATE=$(date)
  echo "Today is $DATE"
  ```

---

## 8. Practical Exercises

### Exercise 1: Writing and Running a Script
1. Create a script that prints the current date and lists all files in the home directory.
2. Make the script executable and run it.

### Exercise 2: Using Conditionals and Loops
1. Write a script that checks if a file exists.
2. If the file exists, print its contents line by line using a `while` loop.

### Exercise 3: Command Substitution and Variables
1. Write a script that captures the output of the `df -h` command.
2. Display the disk usage for each mounted filesystem.