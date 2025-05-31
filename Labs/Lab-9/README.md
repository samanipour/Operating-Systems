## Git Essentials

Git is a free, open source, distributed version control system created by Linus Torvalds. Git requires low operational overhead, yet is flexible and powerful enough to support the demands of complex, large-scale, distributed software development projects.

![img](./assets/git%20components.png)

## Git server

A Git server enables you to collaborate more easily because it ensures the availability
of a central and reliable source of truth for the repositories you will be
working on. A Git server is also where your remote Git repositories are stored; as
common practice goes, the repository has the most up-to-date and stable source
of your projects. You have the option to install and configure your own Git
server, or you can forgo the overhead and opt to host your Git repositories on
reliable third-party hosting sites such as GitHub, GitLab, and Bitbucket.

## Git clients

Git clients interact with your local repositories, and you are able to interact with
Git clients via the Git command line or the Git GUI tools. When you install and
configure a Git client, you will be able to access the remote repositories, work on
a local copy of the repository, and push changes back to the Git server. If you are
new to Git, we recommend starting out using the Git command line; familiarize
yourself with the common subset of git commands required for your day-to-day
operations and then progress to a Git GUI tool of your choice.

## Git Characteristics
- Git stores revision changes as snapshots
- Git is enhanced for local development
- Git is designed to bolster nonlinear development


## Start Working With Git

Whether you are creating a new repository or working with an existing repository, there are basic prerequisite configurations that you need to complete after installing Git on your local development machine. This is akin to setting up the correct date, time zone, and language on a new camera before taking your first snapshot.

At a bare minimum, Git requires your name and email address before you make your first commit in your repository. The identity you supply then shows as the commit author, baked in with other snapshot metadata. You can save your identity in a configuration file using the git config command:

```bash
$ git config user.name "samanipour"
$ git config user.email "alisamanipour.official@gmail.com"
```

## Working with a Local Repository

Now that you have configured your identity, you are ready to start working with
a repository. Start by creating a new empty repository on your local development
machine. We will start simple and work our way toward techniques for working with
a shared repository on a Git server.

### Creating an initial repository
We will model a typical situation by creating a repository for your personal website. Let’s assume you’re starting from scratch and you are going to add content for your project in the local directory ~/my_website, which you place in a Git repository. Type in the following commands to create the directory, and place some basic content in a file called index.html:
```bash
$ mkdir ~/my_website
$ cd ~/my_website
$ echo 'My awesome website!' > index.html
```
To convert ~/my_website into a Git repository, run git init. Here we provide the
option -b followed by a default branch named main:

```bash
$ git init -b main
```
The git init command creates a hidden directory called .git at the root level of your
project. All revision information along with supporting metadata and Git extensions
are stored in this top-level, hidden .git folder.

Git considers ~/my_website to be the working directory. This directory contains the
current version of files for your website. When you make changes to existing files or
add new files to your project, Git records those changes in the hidden .git folder.

![img](./assets/Initial%20repository.png)

## Adding a file to your repository
Up to this point, you have only created a new Git repository. In other words, this Git
repository is empty. Although the file index.html exists in the directory ~/my_website,
to Git, this is the working directory, a representation of a scratch pad or directory
where you frequently alter your files.
When you have finalized changes to the files and want to deposit those changes into
the Git repository, you need to explicitly do so by using the git add file command:

```bash
$ git add index.html
```
With the git add command, Git understands that you intend to include the final
iteration of the modification on index.html as a revision in the repository. However,
so far Git has merely staged the file, an interim step before taking a snapshot via a
commit.

Git separates the add and commit steps to avoid volatility while providing flexibility
and granularity in how you record changes. Imagine how disruptive, confusing, and
time-consuming it would be to update the repository each time you add, remove, or
change a file. Instead, multiple provisional and related steps, such as an add, can be
batched, thereby keeping the repository in a stable, consistent state.

Running the git status command reveals this in-between state of index.html:

```bash
$ git status
```
The command reports that the new file index.html will be added to the repository
during the next commit.

After staging the file, the next logical step is to commit the file to the repository. Once
you commit the file, it becomes part of the repository commit history; for brevity, we
will refer to this as the repo history. Every time you make a commit, Git records
several other pieces of metadata along with it, most notably the commit log message
and the author of the change.

A fully qualified git commit command should supply a terse and meaningful log
message using active language to denote the change that is being introduced by the
commit. This is very helpful when you need to traverse the repo history to track
down a specific change or quickly identify changes of a commit without having to dig
deeper into the change details. We dive in deeper on this topic:

Let’s commit the staged index.html file for your website:
```bash
$ git commit -m "Initial contents of my_website"
```

After you commit the index.html file into the repository, run git status to get an
update on the current state of your repository. In our example, running git status
should indicate that there are no outstanding changes to be committed:

```bash
$ git status
```
Git also tells you that your working directory is clean, which means the working directory has no new or modified files that differ from what is in the repository.

The difference between git add and git commit is much like you organizing a group
of schoolchildren in a preferred order to get the perfect classroom photograph: git add does the organizing, whereas git commit takes the snapshot.

![img](./assets/Staging%20and%20adding%20a%20file%20to%20a%20repository.png)

## Making another commit
Next, let’s make a few modifications to index.html and create a repo history within the
repository.

Convert index.html into a proper HTML file, and commit the alteration to it:

```bash
Convert index.html into a proper HTML file, and commit the alteration to it:
$ cd ~/my_website
# edit the index.html file.
$ cat index.html
<html>
<body>
My website is awesome!
</body>
</html>
```

```bash
$ git commit index.html -m 'Convert to HTML'
```

In our example, we decided to commit the index.html file with an additional argument,
the -m switch, which supplied a message explaining the changes in the commit:

![img](./assets/Staging%20and%20adding%20changes%20to%20a%20tracked%20file%20in%20a%20repository.png)

Note that our usage of git commit index.html -m 'Convert to HTML' does not
skip the staging of the file; Git handles it automatically as part of the commit action.

### Viewing your commits

Now that you have more commits in the repo history, you can inspect them in a
variety of ways. Some git commands show the sequence of individual commits,
others show the summary of an individual commit, and still others show the full
details of any commit you specify in the repository.
The git log command yields a sequential history of the individual commits within
the repository:

```bash
git log
```

### Viewing commit differences

With the repo history in place from the addition of commits, you can now see
the differences between the two revisions of index.html. You will need to recall the
commit ID numbers and run the git diff command:

```bash
$git diff c149e12e89a9c035b9240e057b592ebfc9c88ea4 \
521edbe1dd2ec9c6f959c504d12615a751b5218f
```

### Removing and renaming files in your repository
Now that you have learned how to add files to a Git repository, let’s look at how to
remove a file from one. Removing a file from a Git repository is analogous to adding
a file but uses the git rm command. Suppose you have the file adverts.html in your
website content and plan to remove it. You can do so as follows:

```bash
$ cd ~/my_website
$ ls
index.html adverts.html
$ git rm adverts.html
rm 'adverts.html'
$ git commit -m "Remove adverts html"
[main 97ff70a] Remove adverts html
1 file changed, 0 insertions(+), 0 deletions(-)
delete mode 100644 adverts.html
```
Similar to an addition, a deletion also requires two steps: express your intent to
remove the file using git rm, which also stages the file you intend to remove. Realize
the change in the repository with a git commit.

You can rename a file indirectly by using a combination of the git rm and git add
commands, or you can rename the file more quickly and directly with the command
git mv. Here’s an example of the former:
```bash
$ mv foo.html bar.html
$ git rm foo.html
rm 'foo.html'
$ git add bar.html
```

In this sequence, you must execute mv foo.html bar.html at the onset to prevent
git rm from permanently deleting the foo.html file from the filesystem.
Here’s the same operation performed with git mv:
```bash
$ git mv foo.html bar.html
```
```bash
In either case, the staged changes must subsequently be committed:
$ git commit -m "Moved foo to bar"
[main d1e37c8] Moved foo to bar
1 file changed, 0 insertions(+), 0 deletions(-)
rename foo.html => bar.html (100%)
```
Git handles file move operations differently than most similar systems, employing
a mechanism based on the similarity of the content between two file versions

# Remotes

The repository you’re currently working in is called the local or current repository, and the repository with which you exchange files is called the remote repository. But the latter term is a bit of a misnomer because the repository may or may not be on a physically remote or even different machine; it could conceivably be just another repository on a local filesystem.

Git uses both the remote and the remote-tracking branch to reference and facilitate the connection to another repository. The remote provides a friendly name for the repository and can be used in place of the actual repository URL. A remote also forms part of the name basis for the remote-tracking branches for that repository

Use the git remote command to create, remove, manipulate, and view a remote. All
the remotes you introduce are recorded in the .git/config file and can be manipulated
using git config.

![img](./assets/)

In addition to git clone, other common Git commands that refer to remote repositories
include the following:
```bash
git fetch
```
Retrieves objects and their related metadata from a remote repository.
```bash
git pull
```
Like git fetch but also merges changes into a corresponding local branch.
```bash
git push
```
Transfers objects and their related metadata to a remote repository.
```bash
git ls-remote
```
Shows a list of references along with their associated commit SHA1s, held by a given remote (on an upstream server). This command indirectly answers the question “Is an update available ?”

# Dev-Test-Prod and Git

You can think of this environment as a sort of promotion model where content moves up through the levels as it matures. Each movement can be initiated by someone or some process when it is deemed ready. At any point, different levels may contain the same or different versions of some particular piece of content, depending on which levels it has been promoted to and whether any additional changes have been made at a lower level.

![img](./assets/Dev-Test-Prod%20and%20Git.png)