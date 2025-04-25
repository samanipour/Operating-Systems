# Essential Commands
A big part of working with Linux involves using the command-line interface, also known as the CLI. While you don’t need to learn every single command, becoming familiar with the core tools will greatly simplify software development on a Linux system.

## ✅ What you will learn

You will learn essential command-line tools every Linux developer should know. You practiced:

- Viewing files with tools like `cat`, `less`, `head`, and `wc`
- Finding files with `find`, `locate`, and `which`
- Comparing files using `diff`, `cmp`, and `sdiff`
- Using history and command substitution
- Understanding and changing file permissions
- Compressing files using `gzip`, `bzip2`, and `xz`
- Searching text with `grep`

Mastering these commands gives you the power to navigate, inspect, and manage your system efficiently—skills you’ll use constantly in your Linux development journey.

---

## Why Use Command-Line Tools?

If you’re coming from a GUI-focused environment like Windows, you might think command-line tools are outdated. But there's a good reason why they remain relevant today:

- **Reliability:** Many Linux commands originated from Unix and have been in use for decades. Their long-standing stability means developers can rely on them while focusing on building new tools.
- **Faster to Create:** Command-line tools are quicker to develop than graphical tools. This means tool developers can be more productive.
- **Automation with Scripting:** Unlike GUI tools that require manual steps, CLI tools can be scripted to automate repetitive tasks.
- **Efficiency in Use:** You can execute tasks faster using the CLI—especially if you’re a good typist. Repeating, modifying, and running past commands is simple and saves time.
- **Power:** Combining commands allows for elegant and efficient workflows, often beyond what the original command creators envisioned.

---

## How Many Commands Are Out There?

Students often ask, “Can you list all Linux commands?” That’s like asking astronomers to name all the stars. While there aren’t billions of commands, there are definitely thousands—especially on systems with extensive software installed.

> 🔑 Tip: Don’t worry about learning every command. Instead, focus on the ones that help you with your tasks—especially those related to programming.

---

## Viewing Files

Most Linux files are plain text. So many commands exist to inspect them. Here are a few important ones:

### `file` Command

Before opening a file, it’s smart to check its type:

```bash
file /usr/share/dict/linux.words
```

This will return something like:

```
/usr/share/dict/linux.words: ASCII text
```

You can also inspect binary files, compressed files, or others:

```bash
file /bin/ls
file /usr/share/doc/sed-4.2.2/sedfaq.txt.gz
```

If a file is **not** plain text, avoid viewing it directly—it might garble your terminal. If that happens, use the `reset` command to fix it.

---

### `cat` Command

For smaller text files, `cat` is handy:

```bash
cat /etc/cgrules.conf
```

To number the lines (especially helpful when debugging scripts), use:

```bash
cat -n display.sh
```

This displays the file with line numbers.

---

### `more` and `less` Commands

`cat` scrolls everything quickly. For long files, use `more` or `less`:

```bash
more /usr/share/dict/linux.words
less /usr/share/dict/linux.words
```

You can also pipe output into them:

```bash
ls -l /etc | more
```

Useful controls while reading with `more`/`less`:
- `Spacebar` – move one screen
- `Enter` – scroll one line
- `/pattern` – search for pattern
- `n` – repeat search
- `q` – quit

---

### `head` and `tail`

To show the top or bottom of a file:

```bash
head /usr/share/dict/linux.words
tail -n 5 /etc/passwd
```

These commands default to showing 10 lines unless otherwise specified.

---

### `wc` Command

Displays stats about a file:

```bash
wc display.sh
```

Output example:

```
13  59  291  display.sh
```

That’s 13 lines, 59 words, 291 bytes (characters).

Flags:
- `-l` = lines
- `-w` = words
- `-c` = bytes
- `-m` = characters (better for non-text files)

## 🔍 Finding Files

Linux includes several ways to search for files. Here are some of the most useful tools:

### `which` Command

Finds the path to a command (if it’s in your current `PATH`):

```bash
which bash
```

Returns something like:

```
/bin/bash
```

This is useful to check if a command is available and where it’s located.

---

### `locate` Command

A super-fast way to find files:

```bash
locate gcc
```

The speed comes from using a pre-built database. To update it, run (with root access):

```bash
updatedb
```

If `locate` isn’t installed, you may need to add the `mlocate` package using your distro’s package manager.

---

### `find` Command

A very powerful tool, although it can be slower:

```bash
find / -name bash 2>/dev/null
```

- Searches the entire system from `/`
- `-name` specifies what you’re searching for
- `2>/dev/null` hides permission errors

You can also narrow it down:

```bash
find . -name '*.sh'
```

This searches the current directory and subdirectories for `.sh` files.

---

## 📄 Comparing Files

When working with scripts or config files, comparing changes is common.

### `cmp` Command

Byte-by-byte comparison:

```bash
cmp file1.txt file2.txt
```

Output is either silence (if they’re identical) or something like:

```
file1.txt file2.txt differ: byte 10, line 1
```

---

### `diff` Command

Shows line-by-line differences:

```bash
diff file1.txt file2.txt
```

It outputs how to change one file to make it match the other.

---

### `sdiff` Command

Displays the two files side-by-side:

```bash
sdiff file1.txt file2.txt
```

Useful for visually comparing matching and differing lines.

---

## 🐚 Shell Features

### Command History

Linux keeps a history of your commands. Use the up/down arrow keys to scroll through previous commands.

List all past commands:

```bash
history
```

Re-run a previous command:

```bash
!25      # Runs command #25 from history
```

Re-run the last command with a pattern:

```bash
!ls      # Runs the most recent ls command
```

---

### Command Substitution

You can run one command and insert its output into another using:

- Backticks: `` `command` ``
- `$()` syntax (preferred):

```bash
echo "Current directory: $(pwd)"
```

## 🔐 File Permissions

Every file and directory in Linux has associated permissions that control who can access and modify them.

### Understanding Permission Output

Use `ls -l` to view file permissions:

```bash
ls -l filename.txt
```

You’ll see something like:

```
-rw-r--r-- 1 user group 1234 Apr 1 10:00 filename.txt
```

The first 10 characters break down as:

- First character: file type (`-` for file, `d` for directory)
- Next 3: user (owner) permissions
- Next 3: group permissions
- Final 3: other (everyone else) permissions

### Modifying Permissions

Use the `chmod` command to change them:

```bash
chmod u+x script.sh     # Adds execute permission to the user
chmod g-w file.txt      # Removes write permission for group
chmod o+r file.txt      # Adds read permission for others
```

You can also use numeric (octal) mode:

```bash
chmod 755 script.sh
```

Breakdown:
- `7` (user): read + write + execute
- `5` (group): read + execute
- `5` (other): read + execute

---

## 📦 File Compression Tools

Linux supports several compression methods. Below are key tools to compress and extract files.

### `gzip` and `gunzip`

Compress a file:

```bash
gzip bigfile.txt
```

This produces `bigfile.txt.gz` and removes the original.

Uncompress it:

```bash
gunzip bigfile.txt.gz
```

### `bzip2` and `bunzip2`

Works similarly to `gzip` but often gives better compression:

```bash
bzip2 data.csv
bunzip2 data.csv.bz2
```

### `xz` and `unxz`

Offers high compression ratios:

```bash
xz log.txt
unxz log.txt.xz
```

> 📌 All of these tools work on a single file. Use `tar` to combine multiple files first (covered in later chapters).

---

## 🔎 The `grep` Command

`grep` searches through files for lines that match a pattern.

### Basic Usage

```bash
grep main hello.c
```

Finds all lines containing `main` in `hello.c`.

### Flags and Options

- `-i` — case-insensitive match  
- `-v` — show lines that **don’t** match  
- `-r` — recursive search through directories  
- `-n` — show line numbers

Example:

```bash
grep -i 'error' logfile.txt
```
Awesome! Here are **practice exercises for each section** of Chapter 4: *Essential Commands* based on the paraphrased content.

---

# Exercise Tasks 

Each section below contains tasks that allow you to apply what you’ve learned.

---

## 📂 Viewing Files

**Exercise 1: Identify file types**

- Run:
  ```bash
  file /bin/ls
  file /etc/passwd
  ```

- Create your own test file and check its type:
  ```bash
  echo "Hello Linux" > test.txt
  file test.txt
  ```

**Exercise 2: View files with different commands**

- Try viewing `/etc/passwd` using:
  ```bash
  cat /etc/passwd
  more /etc/passwd
  less /etc/passwd
  head /etc/passwd
  tail -n 5 /etc/passwd
  ```

**Exercise 3: Count lines and words**

- Use `wc`:
  ```bash
  wc /etc/passwd
  wc -l /etc/passwd
  wc -w /etc/passwd
  ```

---

## 🔍 Finding Files

**Exercise 4: Locate commands and files**

- Find where a command is located:
  ```bash
  which bash
  which ls
  ```

- Use `locate` to find `.conf` files:
  ```bash
  sudo updatedb   # may require root
  locate .conf | head
  ```

**Exercise 5: Search with `find`**

- Look for all `.sh` scripts in your home directory:
  ```bash
  find ~ -name "*.sh"
  ```

- Find all files called `README.txt`:
  ```bash
  find / -name "README.txt" 2>/dev/null
  ```

---

## 🔁 Comparing Files

**Exercise 6: Compare files**

- Create two similar text files:
  ```bash
  echo "Hello" > file1.txt
  echo "hello" > file2.txt
  ```

- Run comparisons:
  ```bash
  cmp file1.txt file2.txt
  diff file1.txt file2.txt
  sdiff file1.txt file2.txt
  ```

---

## 🐚 Shell Features

**Exercise 7: Use history and re-run commands**

- Type some commands, then:
  ```bash
  history
  !ls
  !echo
  ```

**Exercise 8: Use command substitution**

- Try:
  ```bash
  echo "Today is $(date)"
  echo "You're in $(pwd)"
  ```

---

## 🔐 File Permissions

**Exercise 9: Explore file permissions**

- Create a file and list its details:
  ```bash
  touch perms.txt
  ls -l perms.txt
  ```

- Modify the permissions:
  ```bash
  chmod u+x perms.txt
  chmod 755 perms.txt
  ls -l perms.txt
  ```

---

## 📦 File Compression

**Exercise 10: Compress and decompress**

- Create a text file:
  ```bash
  echo "Compression test" > sample.txt
  ```

- Try:
  ```bash
  gzip sample.txt
  gunzip sample.txt.gz
  bzip2 sample.txt
  bunzip2 sample.txt.bz2
  xz sample.txt
  unxz sample.txt.xz
  ```

---

## 🔎 Searching with `grep`

**Exercise 11: Search for patterns**

- Try searching in system files:
  ```bash
  grep root /etc/passwd
  grep -i bash /etc/shells
  grep -v /bin /etc/passwd
  ```

- Create a log and search:
  ```bash
  echo -e "ERROR: missing\nINFO: all good\nWARNING: check this" > log.txt
  grep -i error log.txt
  grep -n warning log.txt
  ```