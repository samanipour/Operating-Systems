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

The problem with the cat command comes about when trying to display large files. You will discover that it doesn’t pause at any point during display of the file, but rather it scrolls through the file as if you had some superhero speed-reading skill.


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

    Why Both more and less?

    Why two commands that do essentially the same thing? The more command is the original and the less command is an “improved version” of the more command (hence giving rise to the joke “less does more than more”4).

    In reality, the extra features provided by the less command are less-often used features, at least for most Linux users. The more command is also useful because it is on every Linux (and Unix, MacOS, and Windows) system in the world. The less command is part of an optional software package and not available by default on many systems.
---

### `head` and `tail`

Sometimes you might want to display only the top or bottom part of a file. For example, you might want to look at the comments at the top of a source code file. Or, you want to display recent entries of a log file, which are normally placed at the bottom of the file. For these situations, use the head and tail commands.

By default, these commands display ten lines. For example, Listing 4.2 demonstrates displaying the top ten lines of the /usr/share/dict/linux.words file.

```bash
head /usr/share/dict/linux.words
```
Use the -n option to specify how many lines to display. For example, the command tail -n 5 /etc/passwd displays the last five lines of the /etc/passwd file.

```bash
tail -n 5 /etc/passwd
```

These commands default to showing 10 lines unless otherwise specified.

---

### `wc` Command

To display statistical information about a file, including the number of lines, words, and
characters in the file, use the wc command:

```bash
wc display.sh
```
The output displayed is the number of lines (13), the number of words (59), and number of bytes (291) that are in the display.sh file. Because display.sh is a text file, the number of bytes is actually the number of characters (1 character = 1 byte).

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

There is bound to be a time when you have misplaced a file or just cannot remember where a file is stored. In these cases, you can turn to the locate or find commands to search the system for the missing file.

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

Early each morning a database is created that contains a list of all files and directories on the
system. The locate command is used to search this database. For example, to find the linux.
words file, execute the following command:

```bash
locate linux.words
```
The locate command searches for any file that contains the pattern “linux.words,” which
might result in more output than expected:

```bash
locate words | head
```

---

### `find` Command

The find command searches the live filesystem, which takes more time than using the locate
command (searching databases is much faster), but it does find files that are currently on the
filesystem. The syntax for the find command is as follows:
```bash
find [starting location] [option/arguments]
```
For example, to search for the linux.words file, execute the following command:

```bash
find /usr -name linux.words
```
Note the error messages that appear are because directories existed that the current user was not
allowed to search. This is one of the reasons why you want to start your search in a subdirectory,
not in the / directory. Another reason is because a search of the entire filesystem, starting
from the root directory, might take a lot of time.

You can prevent these error messages by using the redirection method:


- Searches the entire system from `/`
- `-name` specifies what you’re searching for
- `2>/dev/null` hides permission errors

Another advantage of the find command over the locate command is that the find command can search using a variety of different file attributes. For example, you can search for files that are owned by specific users:

```bash
find /home -user student
```

Commonly used find options for specifying what to search for include the following:
- -mmin n—Display files that were modified n minutes ago. Use -mmin +n to specify
- ore than n minutes ago” or -mmin -n to specify “less than n minutes ago.”
- -mtime n—Display files that were modified n days ago (technically n*24 hours ago).
- n use +n and -n like the -mmin option.
- -group groupname—Display files owned by groupname.
- -size n—Display files of a given size represented by n. Follow the n value with a
character to represent a unit of space. For example, -size +10M would display files that
were 10 megabytes or larger.

You can change what the find command does when it finds a file. For example, the -ls option can provide detailed information about each file found:
```bash
find /usr -name linux.words -ls 2> /dev/null
```
Commonly used find options for specifying what to do with the files that are found:
- -delete Deletes the file.
- -ls Provides a long display listing of the files that were found (like the ls -l command).
- -exec { } \; Executes a command on that file that was found. For example:
find /home/student -name sample.txt -exec more {} \;

---

## 📄 Comparing Files

As a developer, you are going to have different versions of files as you improve and bug-fix existing programs. This can cause confusion because determining whether two files are the same or somehow different is sometimes hard. In these cases, you should use the cmp and diff commands.

### `cmp` Command

If you only want to determine whether two files are different, not how they are different, then use the cmp command. Based on the output of the following commands, the display.sh and show.sh files contain identical content (this results in no output when cmp is executed) whereas present.sh contains different content:

```bash
cmp display.sh present.sh
```
The cmp command is also useful for comparing two non-text files. For example, you could
compare two files that contain compiled code.

---

### `diff` Command

If you want two see how two text files differ, use the diff command:

```bash
diff display.sh present.sh
```

The output of the diff command is essentially saying, “If you make these changes, then the files will look the same.” Each section starts with a code that includes the line of the first file, what action to take and the line of the second file. For example, 5c5 means “Change line 5 of the first file to look like line 5 of the second file.”

Additional lines after the “code” line indicate what the changes would look like:
```
< if [-d /etc]
---
> if [ -d /etc ]
```
The line that begins with < shows the current fifth line of the first file. The ```---``` is just used to separate the lines, and the line that begins with the > shows the current fifth line of the second file.


---

## 🐚 Shell Features

### History

Commands that you execute in a shell are saved in memory so you can execute them again. To see these commands, execute the history command (the output could be hundreds of commands, so limit the output with the tail command):

```bash
history | tail -n 5
```

```bash
history
```

Re-run a previous command:

Each command is assigned a number. You can re-execute a command by using this number
with an ! character preceding it

```bash
!25      # Runs command #25 from history
```

Re-run the last command with a pattern:

```bash
!ls      # Runs the most recent ls command
```

---
### Shell Variables
Just like programming languages use variables to store values, the bash shell also stores critical shell information in variables. To create a variable, use the following syntax: VAR=value. To display a variable, use the echo command and place a $ character in front of the variable name:
```bash
[student@localhost ~]$ EDITOR=vi
[student@localhost ~]$ echo $EDITOR
```

    vi
To display all variables, use the set command. There are many predefined variables, so you
might want to pipe the output to the more or head command to limit the output. 

```bash
set | head
```
    output
    ABRT_DEBUG_LOG=/dev/null
    BASH=/bin/bash
    BASHOPTS=checkwinsize:cmdhist:expand_aliases:extglob:extquote:force_
    fignore:histappend:interactive_comments:login_shell:progcomp:promptvars:sourcepath
    BASH_ALIASES=()
    BASH_ARGC=()
    BASH_ARGV=()
    BASH_CMDS=()
    BASH_COMPLETION_COMPAT_DIR=/etc/bash_completion.d
    BASH_LINENO=()
    BASH_REMATCH=()

### Aliases
If you find yourself using the find command daily to search the system for new shell scripts:
```bash
find / -name "*.sh" -ls
```
At some point you ask yourself, “Why do I need to type this long command every day?” The fact is, you don’t need to. You can make an alias for this command, which can be much shorter and easier to type. For example:

```bash
alias myfind='find / -name "*.sh" -ls'
```
Now when you execute the myfind alias, it will run that long find command. However, you must create this alias every time you log in and every time you open a new shell. To have this happen automatically, place this alias command in a file named .bashrc in your home directory. You can also use this file to create variables that you want enabled every time you log in to the system.

### Command Substitution

You can run one command and insert its output into another using:

- Backticks: `` `command` ``
- `$()` syntax (preferred):

```bash
echo "Current directory: $(pwd)"
```

## 🔐 File Permissions

Understanding file and directory permissions is critical for Linux developers because Linux is a multi-user environment and permissions are designed to protect your work from others. To understand permissions, you first need to know the types of permissions that are available in Linux and how these permissions differ when they are applied to files versus when they are applied to directories.

You also need to know how to set permissions. Linux provides two methods: 
- the symbolic method 
- the octal (or numeric) method.

### Understanding Permission Output

To view the permissions of a file or directory, use the ls -l command:


```bash
ls -l /etc/chrony.keys
```

You’ll see something like:

```
-rw-r-----. 1 root chrony 62 May 9 2015 /etc/chrony.keys
```

The first 10 characters break down as:

- First character: file type (`-` for file, `d` for directory)
- Next 3: user (owner) permissions
- Next 3: group permissions
- Final 3: other (everyone else) permissions

Each set has three possible permissions: read (symbolized by r), write (w), and execute (x). If the permission is set, the character that symbolizes the permission displays. Otherwise, a - character displays to indicate that permission isn’t set. So, r-x means “read and execute are set, but write is not set.”

#### Files versus Directories
What read, write, and execute permissions really mean depends on whether the object is a file or directory. 
For files, it means the following:

- Read—Can view or copy file contents.
- Write—Can modify file contents.
- Execute—Can run the file like a program; after you create a program, you must make it
executable before you can run it.

For directories, it means the following:

- Read—Can list files in directory.
- Write—Can add and delete files in directory (requires execute).
- Execute—Can cd into directory or use in pathname.

The write permission on directories is potentially the most dangerous. If a user has write and execute permission on one of your directories, then that user can delete every file in that directory.

### Modifying Permissions

The `chmod` command is used to change permissions on files. It can be used in two ways: symbolic method and octal method. With the octal method, the permissions are assigned numeric values:

- Read = 4
- Write = 2
- Execute = 1

With these numeric values, one number can be used to describe an entire permission set:
- 7 = rwx
- 6 = rw-
- 5 = r-x
- 4 = r--
- 3 = -wx
- 2 = -w-
- 1 = --x
- 0 = ---

So, to change the permissions of a file to rwxr-xr--, you execute the following command:
```
chmod 754 filename
```
With octal permissions, you should always provide three numbers, which will change all the permissions. But what if you only want to change a single permission of the set? For that, use the symbolic method by passing three values to the chmod command as shown in Table

|Who| What| Permission|
|---|---|---|
u = user owner| +| r|
g = group owner| −| w|
o = other| =| x|
a = all sets|

The following demonstrates adding execute permission to all three sets (user owner, group
owner, and others):
```bash
chmod u+x script.sh     # Adds execute permission to the user
chmod g-w file.txt      # Removes write permission for group
chmod o+r file.txt      # Adds read permission for others
```

Examples:

```bash
ls -l display.sh
#-rw-rw-r--. 1 student student 291 Apr 30 20:09 display.sh
```
```bash
chmod a+x display.sh
```
```bash
ls -l display.sh
# -rwxrwxr-x. 1 student student 291 Apr 30 20:09 display.sh
```
---

## 📦 File Compression Tools

As a developer you will be in a position to transfer files from one system to another. You might be downloading software from the Internet, uploading your programs to a server, or sending your programs to someone via email. In all of these cases, knowing how to merge files into a single file and compressing this merged file will be useful. This process makes transporting large amounts of data easy and quick as well as provides something that will take up less disk space. Many commands in Linux enable you to create compressed files, including the gzip, bzip2, and tar commands.

### `gzip` and `gunzip`

The purpose of the gzip command is to create a compressed version of a file. By default, it replaces the original file with the compressed version:


```bash
cp /usr/share/dict/linux.words .
```
```bash
ls -l linux.words
```
```bash
gzip linux.words
```
```bash
ls -l linux.words.gz
```

If you want both the compressed and original file, you have to use the -c option to send the
output to standard output and keep the original file. Of course, you don’t really want the
output to be sent to the screen, so redirect the compressed output to a file:

```bash
ls -l linux.words
```
```bash
gzip -c linux.words > linux.words.gz
```
```bash
ls -l linux.words linux.words.gz
```

    Note
    Typically file extensions, such as .txt and .cvs, are unnecessary in Linux. However, they are
    important for files that you create with the gzip command. This utility expects the extension of
    .gz when it uncompresses a file. If you name the file linux.words.zipped, for example, the gzip
    command will attempt to use the file named linux.words.zipped.gz when uncompressing the file
    (and this will fail).


To uncompress a gzipped file, use the -d option:

```bash
ls -l linux.words.gz
```
```bash
gzip -d linux.words.gz
```
```bash
ls -l linux.words
```

### `bzip2` Command

The difference between gzip and bzip2 is how they perform the compression operation. In
some cases, gzip results in better compression, whereas in others the bzip2 command does.
The gzip utility is the older of the two and considered more established, but the bzip2 utility
is used fairly often on modern Linux distributions.

```bash
bzip2 linux.words
```

to uncompress:

```bash
bzip2 -d linux.words.bz2
```
    Which One Should You Use? 

    Keep in mind that gzip and bzip2 are just two of the compression commands available on Linux. Others include the zip utility and the xz command. With so many to choose from, which should you use?

    If you are only concerned about the compressed file being used on Linux, then it really comes down to compression ratio. Try them all, and find out which compresses the best (or the fastest because higher compression is often slower). If you are considering compressing a file that will be used on other platforms, such as Microsoft Windows, I suggest using zip or gzip because they use a more standard compression algorithm.

### The `tar` Command

The gzip and bzip commands are great for compressing a single file, but what if you want to
merge a bunch of files together? Typically, this means using the tar9 command.
To create a tar file (also called a tar ball), use the following syntax:
```bash
tar -cvf zip.tar /usr/share/doc/zip-3.0
```

You use the -c option to create the tar file. The -v option stands for verbose and results in a list
of the files that are being merged into the tar file. You use the -f option to specify the name of
the resulting tar file.

To list the contents of an existing tar file, use the -t option (t for table of contents):
```bash
tar -tf zip.tar
```

To extract the files from the tar ball, use the -x option:

```bash
tar -xf zip.tar
```

In the current directory there should now be a usr directory with a directory structure of usr/ share/doc/zip-3.0. All the extracted files are in the zip-3.0 directory. By default, the tar command does not compress. However, you can have the tar command use either gzip or bzip2 to compress by using the -z (gzip) or -j (bzip2) options.

---

## 🔎 The `grep` Command (Again!)

Many tools available on Linux are designed to perform operations on text data, but the most powerful and useful for a software developer is the grep command. This command is designed to act as a filter, only displaying the lines of data that match a pattern

For example, to view all the comment lines in a shell script, execute the following command:

### Basic Usage

```bash
grep "#" /etc/rc.local
```

By default, the grep command matches the pattern regardless of whether it is part of another word.You can see the result of this by looking at line 2 of the following command output:
```bash
grep the /etc/bashrc | cat -n
```
If you only want to match a pattern as a separate word, use the -w option:

```bash
grep -w the /etc/bashrc | cat -n
```

### Regular Expressions

discussed wildcards, special characters that the bash shell uses to match filenames in a directory. Wildcards are fairly simple to use, because filenames are typically small and not very complex. However, text within a file can be much more rich and complex. To perform flexible matching with the grep command, use regular expressions (think “wildcards on steroids”).

Regular expressions are a huge topic (seriously, enough to fill up a book larger than the size of this one). As a developer, you don’t need to know everything about regular expressions, so to get you started, I just cover the basics.

As you can see from the following example, the grep command returns results regardless of where on the line the pattern is found:

```bash
grep "growths" /usr/share/dict/linux.words
```
If you only want to see the lines that begin with the pattern, use the regular expression
character ^ at the beginning of the pattern:
```bash
grep "^growths" /usr/share/dict/linux.words
```

If you only want to see the lines that end with the pattern, use the regular expression character $ at the end of the pattern.

Another useful regular expression character is the . character, which represents exactly one character. In the following example, "r..t" matches “root” on lines 1 and 2. For line 3 "r..t" matches “r/ft”:

```bash
grep "r..t" /etc/passwd | cat -n
```

Additional useful grep regular expressions (some of which require using the -E option for extended regular expressions) include:

- * Matches zero or more of the previous character.
- + Matches one or more of the previous character (requires using the -E option).
- . Matches any single character.
- [ ] Matches a single character from a subset of characters; [abc] matches either
-  a, b, or c.
- ? Matches an optional character; a? means “match either the character ‘a’ or nothing”
- equires using the -E option).
- | Match one or another; abc|xyz matches either abc or xyz (requires using the -e
- tion).
- \ Escapes the special meaning of a regular expression character; \* matches simply
a * character.

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

### Searching for Files with grep
The find and locate commands are useful for finding files by name, but they can’t find files based on the contents of a file. The grep command can search all files within a directory structure recursively if you use the -r option.

When you use the grep command in this manner, you probably want to use the -l option, which will list matching filenames (rather than listing every line in every file that matches). You probably also want to redirect STDERR to suppress error messages for files and directories that you don’t have permission to.

for an example that searches for all bash shell scripts in the /etc directory structure:

```bash
grep -rl '^#!/bin/bash' /etc/* 2> /dev/null
```

Here’s a **comprehensive challenge question** that tests students’ ability to use **all major commands** from the lab—including file viewing, searching, comparing, permissions, compression, redirection, and piping—without requiring shell scripting knowledge:

---

### 🧠 Exercise: Investigate, Analyze, and Report**

> **Scenario:**
> You have been asked to conduct a quick forensic investigation of system text files for suspicious content and storage usage. You must combine multiple command-line tools to explore the system and generate a summarized report based on your findings. No shell scripting is required—just command combinations using pipes, redirection, and substitution.

> **Instructions:**

1. **Locate** all `.conf` files in the `/etc` directory modified in the last 5 days. Suppress any error messages caused by permission issues.
2. For each file:

   * **Check the file type** and ensure it's a plain text file.
   * **Count the number of lines and words** using a single command.
3. **Search** for the word “deny” (case-insensitive) in these `.conf` files and list the **filenames and line numbers** where it occurs.
4. **Summarize the permissions** of these files using `ls -l` and **redirect the output** to a file named `conf_permissions.txt`.
5. **Compress** all these `.conf` files into a single `tar.gz` archive named `conf_backup.tar.gz`.
6. **Display the total number of files** in the archive using a command pipeline.
7. **BONUS:** Use `command substitution` to include the current date and time in your output, like:

   ```bash
   echo "Scan completed on: $(date)"
   ```

> **Requirements:**

* You must use and combine:
  `find`, `file`, `wc`, `grep`, `ls`, `chmod`, `tar`, `gzip`, `head`, `tail`, `less`, `cat`, `diff`, `cmp`, redirection (`>`, `2>`), pipes (`|`), and substitution (`$(...)`).

---