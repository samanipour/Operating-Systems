# Working With The Filesystems

No matter what your goals are when using Linux, becoming familiar with how its filesystem works is a fundamental skill. Linux organizes data using a hierarchy of directories, and mastering this layout along with file management is essential for effective use.

---

## Understanding the Filesystem

Most newcomers to Linux have experience with other systems like Windows. One challenge when transitioning is adapting to how Linux structures and accesses files, which differs significantly from platforms like Windows.

For instance, in Windows, storage devices are typically assigned letters like `C:` or `D:` and appear under "My Computer." Linux doesn’t assign drive letters. Instead, all storage—whether local, networked, or removable—appears under a unified root structure.

The topmost directory in Linux is the **root**, represented by `/`. This symbol is also used to separate folder and file names in paths. Think of a file path like a set of directions. For example:

```
/home/bob/sample.txt
```

This path points to a file named `sample.txt` located inside the folder `bob`, which is itself inside `home`, starting from the root `/`.

### Example Filesystem Layout

Here’s a simplified layout to visualize a portion of a Linux system:

```
/
├── bin
├── boot
├── home
│   ├── bob
│   │   └── sample.txt
│   └── julia
├── media
├── root
└── usr
```

---

## Most Commonly Used Directories

Linux systems include thousands of directories, but beginners should focus on a few key ones:

- **`/home`**  
  This is where users’ personal folders reside. Each user has a subdirectory here where they can create and manage their own files.

- **`/root`**  
  This is the home directory for the **root** (superuser) account. Unlike other users, the root’s home isn’t under `/home` but rather has its own location at `/root`.

- **`/bin`** and **`/usr/bin`**  
  These contain executable programs. Most basic user commands live here.

- **`/sbin`** and **`/usr/sbin`**  
  These folders contain commands primarily used by the system administrator.

- **`/media`**  
  This is where Linux mounts removable storage like USB drives or CD-ROMs. Some systems may use `/run/media` instead.

- **`/tmp`**  
  Temporary files go here. Programs use this location for temporary storage instead of placing files in user home directories.

## Naming Rules and Best Practices

When you create new files or folders in Linux, there are certain naming guidelines to follow:

- **Same Rules for Files and Directories:** Both files and folders follow the same naming conventions.
  
- **Case Sensitivity Matters:** File and folder names are case-sensitive. That means `File.txt` and `file.txt` are treated as two separate items.

- **Special Characters Are Allowed (But Be Cautious):**  
  Technically, you can use characters like spaces, tabs, and symbols (`*`, `?`, `[`, `]`, `!`, `$`, `&`, etc.), but it’s best to avoid them. These characters are interpreted specially by the BASH shell and can cause unexpected behavior.

- **No Forward Slashes in Names:**  
  The `/` character separates directory levels in paths, so you can’t use it in file or folder names.

- **File Extensions Are Optional:**  
  While extensions like `.txt` or `.jpg` are helpful for identifying file types, Linux doesn’t rely on them the same way Windows does. They're more for your benefit or the convenience of GUI tools.

- **Special Shortcut Symbols:**  
  - `~` — Refers to the current user’s home directory.  
  - `.` — Indicates the current directory.  
  - `..` — Means "one level up" from the current directory.

---

## Moving Through the Filesystem

When using the Linux command line, you’ll often navigate through the directory tree to access files or run programs.

The place you’re currently located is called the **working directory**. When you open a terminal session, you start in your home directory by default.

### Types of Paths

Linux supports two path types:

- **Absolute Path:**  
  Starts at the root (`/`) and fully describes the location, such as `/home/julia/notes.txt`.

- **Relative Path:**  
  Starts from your current location. If you’re already in `/home`, then `julia/notes.txt` is a relative path to the same file.

> 📝 Absolute paths begin with `/`. Relative paths never do.

### Example 1: Using an Absolute Path

If you're in `/home/julia` and want to go to `/usr/bin`:

```bash
cd /usr/bin
```

You could confirm the location with:

```bash
pwd
```

Or just look at your prompt if it's configured to show the path.

### Example 2: Using a Relative Path

To go from `/home/julia` to `/usr/bin`, you could do this instead:

```bash
cd ../../usr/bin
```

You're telling the shell to "go up two levels, then down into `usr/bin`."

In some cases, a relative path requires less typing and is easier, especially if you’re deep in the directory structure.

## Working with the Filesystem

Now that you know how to move around directories, you’ll want to list and inspect their contents.

### Listing Directory Contents

The `ls` command shows the files in your current directory:

```bash
ls
```

By default, `ls` does **not** show hidden files—those starting with a dot (`.`). To include them, use the `-a` option:

```bash
ls -a
```

This will display entries like:

```bash
.  ..  .bashrc  notes.txt
```

Where:
- `.` = current directory  
- `..` = parent directory

These two appear in every directory and are always hidden by default.

### Why Use Hidden Files?

If you check your home directory using `ls -a`, you’ll likely see many hidden files and folders, such as:

```bash
.bashrc  .profile  .mozilla  .config
```

These files store personal configurations for your shell, applications, or desktop environment. For example:
- `.bashrc` customizes your shell behavior.
- `.mozilla` holds browser data.

### Viewing File Details

To get more information about each file, use the `-l` option:

```bash
ls -l
```

You’ll see something like:

```bash
-rw-r--r-- 1 julia julia 1452 Jan 5 16:20 .bashrc
```

Each line reveals the file type, permissions, number of links, owner, group, size in bytes, date modified, and the file name.

### Interpreting `ls -l` Output

Let’s break down an example:

```bash
-rw-r--r-- 1 julia julia 1452 Jan 5 16:20 .bashrc
```

- **File Type and Permissions**:  
  - `-` = regular file  
  - `d` = directory  
  - First 10 characters indicate permission settings
- **Link Count**: Number of hard links
- **User Owner**: Who owns the file
- **Group Owner**: Associated group
- **Size**: File size in bytes
- **Timestamp**: Last modified date and time
- **Filename**: Name of the file

> 💡 Use `-h` (human-readable) with `-l` to show file sizes in KB, MB, etc.:

```bash
ls -lh
```

---

## Creating and Removing Directories

To **create a directory**, use:

```bash
mkdir foldername
```

To **create nested directories**, use `-p`:

```bash
mkdir -p parent/child
```

If the parent folder doesn't exist, `-p` tells `mkdir` to create it too.

### Removing Directories

To **remove an empty directory**:

```bash
rmdir foldername
```

If it’s not empty, this won’t work. You’ll see:

```bash
rmdir: failed to remove 'foldername': Directory not empty
```

To **remove a directory and everything in it**:

```bash
rm -r foldername
```

This deletes all files and subdirectories inside it.

## Managing Files

Now that you’ve seen how to work with directories, let’s explore how to create, copy, move, and delete files.

### Copying Files

To duplicate a file, use the `cp` command:

```bash
cp original.txt copy.txt
```

This creates `copy.txt` with the same content as `original.txt`.

To avoid accidentally overwriting files, use the interactive option `-i`, which will prompt you before replacing:

```bash
cp -i original.txt copy.txt
```

### Moving or Renaming Files

Use the `mv` command to either move a file to a different location or rename it:

```bash
mv oldname.txt newname.txt
```

This renames the file. If you specify a different directory:

```bash
mv file.txt /home/user/documents/
```

…it moves the file to that location.

### Deleting Files

To remove a file, use the `rm` command:

```bash
rm filename.txt
```

Like with `cp`, you can use the `-i` flag to ask for confirmation before deleting:

```bash
rm -i filename.txt
```

> ⚠️ Be careful with `rm`. Deleted files are not moved to a trash bin—they’re gone.

---

## Creating Empty Files

You can quickly create an empty file or update the timestamp on an existing one using the `touch` command:

```bash
touch newfile.txt
```

If the file doesn’t exist, it’s created. If it does, the last-modified time is updated.

---

## Using Wildcards for Pattern Matching

Linux lets you use wildcards to match groups of files easily:

- `*` — matches any number of characters
- `?` — matches a single character
- `[]` — matches any one of the characters in the brackets

### Examples

```bash
ls *.txt        # lists all files ending in .txt
ls file?.txt    # matches file1.txt, fileA.txt, etc.
ls [abc]*       # matches files starting with a, b, or c
```

Wildcards are extremely powerful and commonly used when working with groups of files.

---

## Redirecting Output

Linux lets you control where command output goes. Instead of sending it to the screen (the default), you can redirect it to a file.

### Basic Output Redirection

To send output to a file:

```bash
echo "Hello, World!" > hello.txt
```

This writes the text into `hello.txt`. If the file already exists, it will be **overwritten**.

To append instead of overwrite:

```bash
echo "Another line" >> hello.txt
```

This adds the text to the end of the file.

You can also redirect input from a file using `<`. For example:

```bash
tr 'a-z' 'A-Z' < input.txt > output.txt
```

This command reads from `input.txt`, converts lowercase letters to uppercase using `tr`, and writes the result to `output.txt`.

---

## Using Pipes to Chain Commands

The `|` (pipe) symbol lets you take the output of one command and feed it directly into another:

```bash
ls -l /etc | more
```

This runs `ls -l /etc`, then passes the result to `more`, which displays one page at a time.

Pipes are useful for filtering or paging through output from commands that produce a lot of text.

> 📌 Note: Redirection affects only standard output (`stdout`). Error messages (`stderr`) are still printed to the screen unless you explicitly redirect them.

# Exercise Tasks 
## 🧭 Understanding the Filesystem

**Exercise 1: Explore the root structure**

- Open a terminal and run:
  ```bash
  cd /
  ls
  ```
- Identify directories like `/home`, `/usr`, `/bin`, `/root`.

**Exercise 2: Find a specific file path**

- Navigate to your home directory:
  ```bash
  cd ~
  ```
- Create a test file:
  ```bash
  touch testfile.txt
  ```
- Use `pwd` to see the full path.

---

## 📁 Learning the Most Used Directories

**Exercise 3: Explore system directories**

- List contents of the following directories:
  ```bash
  ls /home
  ls /usr/bin
  ls /sbin
  ls /media
  ls /tmp
  ```

**Exercise 4: Identify your shell**

- Use the `which` command to find the path of your shell:
  ```bash
  which bash
  ```

---

## 📝 Naming Considerations

**Exercise 5: Test naming rules**

- Try creating files with:
  - Uppercase and lowercase names
  - Spaces in the filename
  - Asterisks or dollar signs

```bash
touch Data.txt data.txt
touch "file with space.txt"
touch file\*.txt
```

- Observe which ones cause issues and fix them using quotes or escapes.

---

## 🧭 Navigating the Filesystem

**Exercise 6: Use absolute and relative paths**

- Navigate from your home directory to `/usr/bin` using:
  ```bash
  cd /usr/bin     # Absolute
  cd              # Back home
  cd ../../usr/bin  # Relative (if you’re two levels deep)
  ```

**Exercise 7: Practice with `.` and `..`**

- Move up one level from your current directory:
  ```bash
  cd ..
  ```

- Return to the directory you just left:
  ```bash
  cd -
  ```

---

## 🔍 Managing the Filesystem

**Exercise 8: List directory contents with options**

- Use the following commands:
  ```bash
  ls -a
  ls -l
  ls -alh
  ```

- Explore hidden files in your home directory.

**Exercise 9: Interpret `ls -l` output**

- Choose a file and explain its:
  - Permissions
  - Owner/group
  - Size
  - Timestamp

---

## 🗂️ Managing Directories

**Exercise 10: Create nested directories**

```bash
mkdir -p projects/linux/assignment1
```

**Exercise 11: Remove directories**

```bash
rmdir emptydir
rm -r projects
```

> Try creating and removing both empty and non-empty directories.

---

## 📄 Managing Files

**Exercise 12: Copy and rename files**

```bash
touch original.txt
cp original.txt copy.txt
mv copy.txt renamed.txt
```

**Exercise 13: Safely remove files**

```bash
rm -i renamed.txt
```

**Exercise 14: Create and timestamp files**

```bash
touch timestamp.txt
ls -l timestamp.txt
sleep 2
touch timestamp.txt
ls -l timestamp.txt
```

---

## ✨ Using Wildcards

**Exercise 15: Create test files and match with wildcards**

```bash
touch file1.txt file2.txt fileA.txt anotherfile.txt
ls file?.txt
ls *.txt
ls [fa]*.txt
```

---

## ➡️ Redirecting Output

**Exercise 16: Redirect output to a file**

```bash
echo "Hello Linux" > hello.txt
cat hello.txt
```

**Exercise 17: Append to a file**

```bash
echo "Another line" >> hello.txt
cat hello.txt
```

**Exercise 18: Redirect input and transform**

```bash
echo "sample text" > input.txt
tr 'a-z' 'A-Z' < input.txt > output.txt
cat output.txt
```

---

## 🔗 Using Pipes

**Exercise 19: Chain commands with pipes**

```bash
ls -l /etc | less
```

- Try with other commands:
  ```bash
  ps aux | grep bash
  ```