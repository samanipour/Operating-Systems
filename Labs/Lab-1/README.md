# Lab-1: Basic Commands and Directory Hierarchy

## 1. Introduction to the Shell

### 1.1 Opening the Shell
- **Objective**: Open a terminal window to access the shell.
- **Steps**:
  1. On Ubuntu, search for "Terminal" in the application menu and open it.
  2. On Fedora, do the same or press `Ctrl+Alt+T`.
  3. The prompt should appear as `name@host:path$`, where:
     - `name`: Your username
     - `host`: Machine name
     - `path`: Current directory

---

## 2. Basic Commands

### 2.1 Displaying Directory Contents with `ls`
- **Objective**: List files and directories.
- **Commands**:
  - List all files in the current directory:
    ```bash
    ls
    ```
  - Detailed listing (including file permissions, owner, size, and modification date):
    ```bash
    ls -l
    ```
  - List all files, including hidden ones:
    ```bash
    ls -a
    ```

### 2.2 Copying Files with `cp`
- **Objective**: Copy files from one location to another.
- **Commands**:
  - Copy a file:
    ```bash
    cp source_file destination_file
    ```
  - Copy multiple files to a directory:
    ```bash
    cp file1 file2 file3 target_directory/
    ```

### 2.3 Moving and Renaming Files with `mv`
- **Objective**: Move or rename files.
- **Commands**:
  - Rename a file:
    ```bash
    mv old_name new_name
    ```
  - Move files to another directory:
    ```bash
    mv file1 file2 target_directory/
    ```

### 2.4 Creating Files with `touch`
- **Objective**: Create an empty file or update the timestamp of an existing file.
- **Commands**:
  - Create an empty file:
    ```bash
    touch newfile.txt
    ```
  - Update the timestamp of an existing file:
    ```bash
    touch existingfile.txt
    ```

### 2.5 Deleting Files with `rm`
- **Objective**: Permanently remove files.
- **Commands**:
  - Delete a single file:
    ```bash
    rm filename.txt
    ```
  - Delete multiple files:
    ```bash
    rm file1.txt file2.txt
    ```

### 2.6 Displaying Text with `echo`
- **Objective**: Output text to the screen.
- **Commands**:
  - Print a message:
    ```bash
    echo "Hello, OS-Lab!"
    ```

---

## 3. Navigating Directories

### 3.1 Changing Directories with `cd`
- **Objective**: Move between directories.
- **Commands**:
  - Go to a specific directory:
    ```bash
    cd directory_name
    ```
  - Move to the parent directory:
    ```bash
    cd ..
    ```
  - Return to the home directory:
    ```bash
    cd ~
    ```

### 3.2 Creating Directories with `mkdir`
- **Objective**: Make new directories.
- **Commands**:
  - Create a new directory:
    ```bash
    mkdir new_directory
    ```

### 3.3 Removing Directories with `rmdir`
- **Objective**: Delete empty directories.
- **Commands**:
  - Remove an empty directory:
    ```bash
    rmdir empty_directory
    ```
  - Remove a non-empty directory and all of its contents:
    ```bash
    rm -r non_empty_directory
    ```

---

## 4. Viewing File Contents

### 4.1 Displaying File Content with `cat`
- **Objective**: View the contents of a file.
- **Commands**:
  - Display a file's contents:
    ```bash
    cat filename.txt
    ```

### 4.2 Viewing File Contents Page by Page with `less`
- **Objective**: View large files page by page.
- **Commands**:
  - Open a file with `less`:
    ```bash
    less largefile.txt
    ```
  - Navigate using:
    - Spacebar: Move forward one page.
    - `b`: Move back one page.
    - `q`: Quit the viewer.

---

## 5. Intermediate Commands

### 5.1 Searching Text with `grep`
- **Objective**: Search for text patterns within files.
- **Commands**:
  - Search for a word in a file:
    ```bash
    grep "search_term" filename.txt
    ```
  - Case insensitive search:
    ```bash
    grep -i "search_term" filename.txt
    ```
  - Search in all files of a directory:
    ```bash
    grep "search_term" /path/to/directory/*
    ```

### 5.2 Viewing Directory Path with `pwd`
- **Objective**: Show the current working directory.
- **Commands**:
    ```bash
    pwd
    ```

---

## 6. Special Characters and Wildcards

### 6.1 Using Wildcards
- **Objective**: Match patterns to file and directory names.
- **Examples**:
  - List all files starting with 'file':
    ```bash
    ls file*
    ```
  - List all files ending in '.txt':
    ```bash
    ls *.txt
    ```
  - List all files containing 'log':
    ```bash
    ls *log*
    ```

---

## 7. Getting Help

### 7.1 Using `man` Pages
- **Objective**: Access manual pages for commands.
- **Commands**:
  - View the manual for a command:
    ```bash
    man ls
    ```
  - Search for a command by keyword:
    ```bash
    man -k keyword
    ```

### 7.2 Using `--help` Option
- **Objective**: Get a quick summary of a command's options.
- **Commands**:
  - Display help for a command:
    ```bash
    ls --help
    ```

---

## 8. Practical Exercises
1. Create a directory named `os_lab`.
2. Inside `os_lab`, create an empty file called `test.txt`.
3. Copy `test.txt` to `backup.txt`.
4. Rename `backup.txt` to `test_backup.txt`.
5. Create another directory called `archive`.
6. Move `test_backup.txt` to `archive`.
7. List all files and directories with detailed information.
8. Display the contents of `test.txt` using `cat`.
9. Search for the word "test" inside `test.txt` using `grep`.
10. Delete `test.txt` and `archive`.
