## Table of contents

- [Navigation](#navigation)
  * [Paths](#paths)
  * [Globs](#globs)
  * [pwd](#pwd)
  * [cd](#cd)
  * [tree](#tree)
- [Creation and deletion](#creation-and-deletion)
  * [touch](#touch)
  * [mkdir](#mkdir)
  * [rmdir](#rmdir)
  * [rm](#rm)
- [Copying and moving](#copying-and-moving)
  * [cp](#cp)
  * [mv](#mv)
- [File manipulation](#file-manipulation)
  * [cat](#cat)
  * [more](#more)
  * [less](#less)
  * [head](#head)
  * [tail](#tail)
  * [chmod](#chmod)
  * [file](#file)
- [Process manipulation](#process-manipulation)
  * [I/O redirections](#io-redirections)
  * [ps](#ps)
  * [kill](#kill)
- [File searching](#file-searching)
  * [find](#find)
  * [wc](#wc)
  * [ls](#ls)
- [Finding commands and manuals](#finding-commands-and-manuals)
  * [which](#which)
  * [man](#man)
- [Information in file](#information-in-file)
  * [grep](#grep)
  * [cut](#cut)
  * [sort](#sort)
- [File editing](#file-editing)
  * [sed](#sed)
- [Archiving](#archiving)
  * [tar](#tar)
  * [zip](#zip)

## Navigation

### Paths

A quick primer on paths first.

#### Absolute

An absolute path is the complete path to a directory or file, starting from the root of the machine. In Unix-like systems, `/` is a file separator, which represents the root directory when typed alone.

#### Relative

A relative path is a path to a file that is relative to some directory. For example, if we are currently working in the directory `/home/user/work/phd/`, then:

- We can either refer to the `work` folder as `/home/user/work` or `../` (i.e., up one level)
- We can either refer to the `phd folder`as `/home/user/work/phd/` or simply `. (i.e., the same level)`

Note how `..` is the parent directory and `.` it the current directory.

This very useful for quick navigation and file manipulation (e.g. copy a file to the parent directory). Another special symbol is `~`, which refers to `/home/user`. To refer to another use, use `~username`.

### Globs

Globs is short for global patterns and refers to a symbol(s) that will be expanded out to match one or more strings.

Note that `ls` will list the files and folders in the current directory.

```sh
user:~$ ls
main.c a.txt b.txt c.txt d.txt 0.txt to.txt go.txt go1.txt gofile1.txt gofile2.txt

More information later.


Let us assume we have a directory that has the following files:

```sh
user:~$ ls
main.c a.txt b.txt c.txt d.txt 0.txt to.txt go.txt go1.txt gofile1.txt gofile2.txt
```

Hint, you can create this files with `touch main.c a.txt b.txt c.txt d.txt 0.txt to.txt go.txt go1.txt gofile1.txt gofile2.txt`.

#### ?

The question mark matches to any character once. For example,

```sh
user:~$ ls ?.txt
a.txt b.txt 0.txt
```

We can use it multiple times in funky ways,

```sh
user:~$ ls ??.t?t
to.txt go.txt
```

```sh
user:~$ ls main.?
main.c
```

#### *

The asterisk matches to any string. For example,

```sh
user:~$ ls *.txt
a.txt b.txt c.txt d.txt 0.txt to.txt go.txt go1.txt gofile1.txt gofile2.txt
```

We can even combine wildcards.


```sh
user:~$ ls go*e?.txt
gofile1.txt gofile2.txt
```

#### []

Square brackets match to either a list, range or class. This is easier shown than explained.

#### List

```sh
user:~$ ls [ab0].txt
a.txt b.txt 0.txt
```

Note that this is case sensitive, e.g. `[ab0].txt` would not match with `A.txt`.
You can also separate the list with commas, i.e. `[a,b,0]`.

#### Ranges

Similar to list, but with more efficient notation.

```sh
user:~$ ls [a-d].txt
a.txt b.txt c.txt d.txt
```

#### Classes

Classes are shorthand for pre-defined ranges and lists.
- [:upper:] is the same as A-Z
- [:lower:] is the same as a-z
- [:alpha:] is the same as a-zA-Z
- [:digit:] is the same as 0-9
- [:alnum:] is the same as a-zA-Z0-9
- [:puncht:] includes all legal symbols.

Note that we do not have the ranges (e.g. A-Z) wrapped in square brackets. Hence, to match on a class, we would use:

```sh
user:~$ ls [[:alpha:]].txt
a.txt b.txt c.txt d.txt
```

#### ^

The caret is used in conjunction with the square brackets to reverse the range. For example,

```sh
user:~$ ls [^[:alpha:]].txt
0.txt
```

#### {}

We can use curly brackets to combine multiple globs, such as:

```sh
user:~$ ls {[[:alpha:]].txt,*.c}
a.txt b.txt c.txt d.txt main.c
```

Remember, no spaces!

### pwd

Also known as present working directory, this is a useful command that will print out the absolute path you are currently in.

```sh
user:~$ pwd
/home/user
```

If you already know something about *symlinks* then `pwd -P` will show the path the symlink is pointing to.

### cd

Also known as change directory, this allows to (duh) change directory.

#### No arguments

Running just `cd` will change your directory to the current user's home directory.

```sh
user:~$ pwd
/home/user/some/path/down/here
user:~$ cd
user:~$ pwd
/home/user
```

#### One argument

Will change the directory to the path in the argument. If the path starts with `/`, then we are dealing with an absolute path. If we start with anything else (let's ignore variables for now), it will treat the argument as a relative path.

```sh
user:~$ pwd
/home/user/some/path/down/here
user:~$ cd ..
user:~$ pwd
/home/user/some/path/down
user:~$ cd /home/user/university/work/
user:~$ pwd
/home/user/university/work/
```

Note how we can use `~`, `..`, and `.` as arguments!

```sh
user:~$ pwd
/home/user/some/path/down/here
user:~$ cd ~/university/work
user:~$ pwd
/home/user/university/work/
```

### tree

Shows the contents of a directory and its sub directories as a tree.

#### No arguments

Will show the contents and sub directories of the current directory.

```sh
user:~$ tree
.
├── afolder
├── bfolder
│   ├── cfolder
│   │   ├── hello.txt
│   │   ├── world.txt
│   │   └── yay.sh
│   └── something
├── file1.txt
└── file2.sh

4 directories, 6 files
```

We can specify how deep into the sub directories we want to go using the `-L` flag, followed by a number.

```sh
user:~$ tree -L 1
.
├── afolder
├── bfolder
├── file1.txt
└── file2.sh

3 directories, 2 files
```

Other useful flags are `-d` for directories only, `-a` to include hidden folders and files (i.e. whose name begins with `.`)

#### One argument

The argument is a path, and the command will work exactly the same as above, but will act upon the specified directory.

```sh
user:~$ tree -L 1 /home/user
/home/user
├── Desktop
├── Documents
├── Downloads
├── Games
├── Misc
├── Music
├── Pictures
├── Public
├── Sync
├── Templates
├── Videos
└── Work

12 directories, 0 files
```

## Creation and deletion

### touch

This command will create an empty file.

```sh
user:~$ ls

user:~$ touch emptyfile.bat
user:~$ ls
emptyfile.bat
```

There are some funky things we can do with touch, such as altering access or modification times, but it's not important now.

### mkdir

Creates empty directories.

```sh
user:~$ ls

user:~$ mkdir lessons
user:~$ ls
lessons
```

A useful flag I use a lot if `-p` which will create all necessary parent folders if they do not exist. For example

```sh
user:~$ ls

user:~$ mkdir lessons/so
mkdir: cannot create directory ‘lessons/so’: No such file or directory
```

However, with the `-p` flag.

```sh
user:~$ ls

user:~$ mkdir -p lessons/so
user:~$ tree
.
└── lessons
    └── so
```

### rmdir

Removes a directory but only if it is empty! If it is not empty you will get an error saying `rmdir: failed to remove 'X': Directory not empty`

```sh
user:~$ ls
somefolder
user:~$ rmdir somefolder
user:~$ ls

```

We can also use `-p` to also delete the parent folders. For example,

```sh
user:~$ tree
.
└── lessons
    └── so
user:~$ rmdir -p lessons/so
user:~$ ls

```

### rm

This will remove a file and with some flags, directories that have files within them.

```sh
user:~$ ls
file1.txt
user:~$ rm file1.txt
user:~$ ls

```

The two flags that are frequently used as `-r`, which means recursive, and `-f`, which means force (i.e. do not ask for confirmation). This is useful when we have a folder with a complex structure and lots of files that we want to delete.

```sh
user:~$ ls
example
user:~$ tree example
example
├── afolder
├── bfolder
│   ├── cfolder
│   │   ├── hello.txt
│   │   ├── world.txt
│   │   └── yay.sh
│   └── something
├── file1.txt
└── file2.sh

4 directories, 6 files

user:~$ rm -rf example
user:~$ ls

```

You can use wildcards (more on this later) to perform deletions.

```sh
user:~$ tree .
.
├── afolder
├── bfolder
│   ├── cfolder
│   │   ├── hello.txt
│   │   ├── world.txt
│   │   └── yay.sh
│   └── something
├── file1.txt
└── file2.sh

4 directories, 6 files

user:~$ rm -rf *
user:~$ ls

```

This is not advised because:
1. `rm` is irreversible, there is no concept of recycling bins or trash.
2. It will really delete everything.
3. Accidental spaces can lead the necessity to reinstall your whole operating system and potentially lose all your files, especially if you are running root. For example:

```sh
user:~$ rm -rf /home/user/example
# This is ok!

user:~$ rm -rf /home/user/ example
# You have just deleted your entile home folder.
```

Some nicer removal tools include `gio trash`, but we will not cover that here.


## Copying and moving

### cp

This command copies folders and files. The below example shows us copying a file and maintaining its filename.

```sh
user:~$ ls
example file1.txt
user:~$ ls example

user:~$ cp file1.txt example
user:~$ tree example
example
└── file1.txt

1 directory, 1 file
```

To change the filename during copying:

```sh
user:~$ ls
example file1.txt
user:~$ ls example

user:~$ cp file1.txt example/copied_file.txt
user:~$ tree example
example
└── copied_file.txt

1 directory, 1 file
```

To copy a directory, we need to tell `cp` to copy the folder recursively (otherwise it will just ignore the folder copying and raise the error `cp: -r not specified; omitting directory 'X'`).

```sh
user:~$ ls
example should_be_subfolder
user:~$ ls example

user:~$ ls should_be_subfolder
file1.txt

user:~$ cp -r should_be_subfolder example
user:~$ tree example
example
└── should_be_subfolder
    └── file1.txt

2 directory, 1 file
```

We can also change the folder's name in the same way we did for files.

```sh
user:~$ ls
example should_be_subfolder
user:~$ ls example

user:~$ ls should_be_subfolder
file1.txt

user:~$ cp -r should_be_subfolder example/part1
user:~$ tree example
example
└── part1
    └── file1.txt

2 directory, 1 file
```

In essence, to copy a file (respectively, folders) to another location, we use:

```sh
cp path/to/source_file.ext path/to/target_file.ext
```

And to copy a file into another directory, keeping the filename:

```sh
cp path/to/source_file.ext path/to/target_parent_directory
```

### mv

Move files! Works very similarly to `cp`. Interestingly, there is no "rename" command in Unix systems, meaning that to rename, we do the following:


```sh
user:~$ ls
file1.txt
user:~$ mv file1.txt file1.txt.bak

user:~$ ls
file1.txt.bak
```

In essence, we are "moving" the file into the current directory with a new name.


## File manipulation

### cat

This command will print and concatenate files. For example, we can print to the standard output by doing the following:

```sh
user:~$ cat file1.txt
This is a file that I have written! How exciting!

```

You can change the stream using I/O redirections, which will be discussed later.

Tip, you can write directly to a file by using `cat > filename` (you will see this in the exercise sheet).

### more

Displays a file interactively, with the following controls:

- `Space` or `z` will go to the next page
- `d` will scroll down some lines (on my terminal, it's 11 lines)
- `q` will exit

To learn more about it, press `h`.

You can use `+100` or `-100` to start displaying the file from line 100.

The main problem is that you can only really scroll forward

### less

Similar to more, but with backwards scrolling!

- `Space` or `d` will scroll down
- `b` or `u` will scroll up
- `G` will go to the end of a file
- `g` will go to the start of a file
- `q` will exit

### head

Outputs the first part of a file.

```sh
 head [-n|--lines] <number> path/to/file
```

You can also use `-c` to output the first `<number>` of bytes.

### tail

Same as head, but will show the last part of a file.

### chmod

This will change the permissions of a file or folder.

#### Overview of Unix permissions

For a file, it can either be:
- read (`r`)
- written to (`w`), also can indicate deletion
- executed as a script/binary (`x`).

For folders, these have different meaning:
- read (`r`), i.e. we can list the contents of the folder
- written to (`w`), i.e. we can create and delete content in the folder
- executable (`x`), i.e. we can actually go *into* the folder (without this, we would not be able to `cd` into the directory).

Each file can have different permission attributes for:
- The owner of the file (e.g. who created the file)
- The group of the file (e.g. a group the user belongs to when they created the file)
- Other users/groups

By running `ls -l`, we can see file permissions:

```sh
user:~$ ls -l .
-rwxr-xr--  1 user   users 1024  Nov 2 01:12  file1.txt
drwxr-xr--- 1 user   users 1024  Nov 2 02:10  subfolder
```

The `-rwxr-xr--` may seem confusing at first, but it is quite simple:

- Char 1: Either `-` or `d`, which determines whether we are dealing with a file or folder.
- Chars 2-4 display owner permissions, so `rwx` means the owner can read, write and execute the file.
- Chars 5-7 display the group permissions, so `r-x` means the group `users` can read and execute the file, but not write to it.
- Chars 8-10 display the other users'/groups' permissions, so `r--` means that any other user or group can only read the file.

For folders, `r` means being able to see what is inside the folder (e.g. run `ls`), `w` means we can create and delete files/sub folders, and `x` means we can access the folder (e.g. `cd` into it).

#### chmod

##### Symbolic mode

We can *change mode* on files or directories via this command.

For example, for a `<W> \in {u, g, o}` and  `<P> \in {r, w, x}` running

```sh
chmod <W>+<P> file
```

will add the permission <P> (either read, write or execute) for either the user owner, group or other people. To allow the user owner to read and execute a file, we can run:

```sh
chmod u+rx file
```

To remove permissions, we use `-` instead, and to set permissions using `=`.
For example, to give the other users the same rights as groups (i.e. copy the permissions), we run:

```sh
chmod o=g file
```

Or to set the others' permissions to exactly `r--`, we run:

```sh
chmod o=r file
```

We can chain arguments, like so:

```sh
chmod u+x,o=g file
```

When dealing with folders, `-R` will apply the mode changes to all sub folders and files in the directory.

##### Dreaded absolute permissions

If you want to be l33t efficient, don't bother with all those pesky `r`, w` and `x`, just remember this table:

| Octal | Symbolic |
|-------|----------|
| 0 | --- |
| 1 | --x |
| 2 | -w- |
| 3 | -wx |
| 4 | r-- |
| 5 | r-x |
| 6 | rw- |
| 7 | rwx |

I have never memorised this apart from `0` being no permissions and `7` being everything. Maybe I am just a lazy Gen Z.

In essence, running:

```sh
chmod 746 file
```

Will let owners `rwx`, group `r` and others `rw`.


### file

This command will help us determine the file type.

```sh
user:~$ file mydir
mydir: directory

user:~$ file main.tex
main.tex: LaTeX 2e document, Unicode text, UTF-8 text, with very long lines (460)
```

## Process manipulation


### I/O redirections

Firstly, you have three streams: standard in (stdin or 0), standard out (stdout or 1) and standard error (stderror or 2).

When we say redirection, it means that instead of printing any of the streams to the terminal, they are redirected somewhere else, such as a file.

#### > and >>

This will send the result of executing the leftmost command's standard output to some file, even if it has to overwrite it. We can also write `1>` and `1>>` but this is not common.


```sh
user:~$ cat a.txt

user:~$ echo a\\nb\\nd > a.txt
a
b
d
user:~$ echo a\\nb\\nc > a.txt
user:~$ cat a.txt
a
b
c
```

The above example sends the standard output of the echo command to a file called a.txt.

Using `>>` prevents overwrites and will instead *append* the output.

```sh
user:~$ cat a.txt
a
user:~$ echo b\\nc >> a.txt
a
b
c
user:~$ echo d >> a.txt
user:~$ cat a.txt
a
b
c
d
```

#### < and <<

When we feed an input to a program, the program reads from the standard input, which is normally the keyboard. However, we can also feed a file into standard input.

``` sh
user:~$ echo a\\nc\\nb > unsorted.txt
user:~$ cat unsorted.txt
a
c
b
user:~$ sort <unsorted.txt
a
b
c
user:~$ sort <unsorted.txt > sorted.txt
user:~$ cat sorted.txt
a
b
c
```

#### 2> and 2>>

This redirects and error messages to a file. For example, from a non-root user, we cannot `ls` the directory `/root`.

```sh
user:~$ ls /root
ls: cannot open directory '/root': Permission denied
```

We can also get this error message into a file,

```sh
user:~$ ls /root 2> error.log
user:~$ cat error.log
ls: cannot open directory '/root': Permission denied
```

#### Some operations using redirects

We can redirect both the standard out and standard error to the same file.

```sh
user:~$ ls /root > error.log 2> error.log
user:~$ cat error.log
ls: cannot open directory '/root': Permission denied
```

or

```sh
user:~$ ls /root > error.log 2>&1
user:~$ cat error.log
ls: cannot open directory '/root': Permission denied
```

or even

```sh
user:~$ ls /root &> error.log
user:~$ cat error.log
ls: cannot open directory '/root': Permission denied
```

The symbols `&>>` work as expected.

#### Pipes |

Pipes are essential in any Unix-like system as it allows us to chain together programs by redirecting their standard output.

For example, instead of echoing `a\\nc\\nb` to a file, which we then feed into sort, we could just do this:

```sh
user:~$ echo a\\nb\\nd | sort > sorted.txt
user:~$ cat sorted.txt
a
b
c
```

#### &

A bit of a side note, but `&` will run whatever program is on its left in the background (still tied to the terminal, so if you shut off the terminal, the process dies). Hence, it is still connected to the terminal session you are currently running.

```sh
user:~$ cp -R big_really_big_folder/ backups/ &
[1] 21451
[1]  + 21451 done                 cp -R big_really_big_folder/ backups/
```

And you can check the status with `jobs`.

```sh
user:~$ jobs
[1]-  Running                 cp -R big_really_big_folder/ backups/ &
```

But more on this later.

### ps

This will print information about processes.

Just running `ps` will tell you about the current shell and about `ps` itself.

```sh
user:~$ ps
    PID TTY          TIME CMD
  44085 pts/0    00:00:03 zsh
  54635 pts/0    00:00:00 ps
```

We will get into the header meaning soon.

#### -A

This will list all processes currently running in the system.

```sh
user:~$ ps -A
PID TTY          TIME CMD
      1 ?        00:00:02 systemd
      2 ?        00:00:00 kthreadd
      3 ?        00:00:00 pool_workqueue_release
      4 ?        00:00:00 kworker/R-rcu_gp
      ...
```

#### aux

To list all running processes:

```sh
user:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           2  0.0  0.0      0     0 ?        S    Sep25   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    Sep25   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   Sep25   0:00 [kworker/R-rcu_gp]
root           5  0.0  0.0      0     0 ?        I<   Sep25   0:00 [kworker/R-sync_wq]
root           6  0.0  0.0      0     0 ?        I<   Sep25   0:00 [kworker/R-kvfree_rcu_reclaim]
root           7  0.0  0.0      0     0 ?        I<   Sep25   0:00 [kworker/R-slub_flushwq]
root           8  0.0  0.0      0     0 ?        I<   Sep25   0:00 [kworker/R-netns]
root          10  0.0  0.0      0     0 ?        I<   Sep25   0:00 [kworker/0:0H-events_highpri]
...
```

Note that `aux` means `a` for all users' processes, `u`  to display this "user friendly" format, and `x` to show tasks that are running the background.

The columns mean the following (note, there are even more columns, but it's a bit much):
- `USER` lists the user who is running the process (e.g. `root` or `user`)
- `PID` is the process ID
- `%CPU`is the CPU usage (well, % of time running during the process' lifetime)
- `%MEM` is the RAM usage
- `VSZ` is the virtual memory size that the process is using
- `RSS` is the non-swappable physical memory size that the process is using
- `STAT` is the process status code, such as `Z`, zombie, `S` for sleeping or `R` for running. Modifiers include `<` for high priority and `I` is multi-threaded.
- `TIME` is the cumulative time CPU time (includes time used by threads)
- `START` is when the command started
- `TTY` is the name of the terminal that is controlling the process
- `CMD` the command invoked to start the process.


We can shorten this by running, for example, `ps aux | head -n 5`.

We can also search for processes by piping the output of `ps aux` to grep.

```sh
user:~$ ps aux | grep emacs
joao        1049  0.0  0.3 255388 120704 ?       Ss   Sep25   0:01 /usr/bin/emacs --fg-daemon
joao       36145  1.8  0.9 887824 316904 tty2    Sl+  10:46   2:17 emacs



### jobs

As we have seen, this will list the jobs that are currently running.

### top

This will display real-time information about running processes.

```sh
user:~$ top
top - 13:14:57 up 20:35,  1 user,  load average: 0.62, 0.68, 0.67
Tasks: 454 total, 2 running, 451 sleep, 1 d-sleep, 0 stopped, 0 zombie
%Cpu(s):  0.5 us,  0.2 sy,  0.0 ni, 81.1 id, 18.0 wa,  0.2 hi,  0.0 si,  0.0 st
MiB Mem :  31648.2 total,   8398.2 free,   8316.3 used,  18756.9 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.  23331.9 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
  36248 user      20   0 2019856 441604 200912 S   5.0   1.4   0:23.29 ghostty
   1500 user      20   0 1081588 199872 155724 R   3.7   0.6  21:10.06 Hyprland
   1640 user      20   0   72.6g 641536  52000 S   3.7   2.0  22:17.73 filen-cli
```

Press `h` to see all different commands. If you have it installed, `htop` is much nicer.

### kill

Self explanatory, it will kill a desired process. In course, we will only use the strongest version, which is `kill -9`, which is equivalent to force kill the process.

```sh
user:~$ ps aux | grep bottles
56971 pts/0    S+     0:00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn --exclude-dir=.idea --exclude-dir=.tox --exclude-dir=.venv --exclude-dir=venv bottles

user:~$ kill -9 56971

```

## File searching

### find

We can find files by name, such as:

```sh
user:~$ ls
file1.txt file2.txt

user:~$ find . -name "file1.txt"
./file1.txt
```

We can use the aforementioned patterns to find files:

```sh
user:~$ find dir1/dir3 -name "[fg][35][4-7a-z].txt" -print
dir1/dir3/f3a.txt

```

You can also specify the size of the file you want to search for (in blocks, which are usually 512 bytes) using `-size`.

```sh
user:~$ ls -lh
-rw-r--r-- 1 joao joao  33 Sep 26 13:20 file1.txt
-rw-r--r-- 1 joao joao  33 Sep 26 13:20 file2.txt

user:~$ find . -name "file1.txt" -size 2

user:~$ find . -name "file1.txt" -size 1
./file1.txt

```

We can use `--size 2c` for 2 bytes.

### wc

Word count!

Using the `-l` option will count the number of lines.

```sh
user:~$ cat file1.txt
hello
my
name
is
something maybe

user:~$ wc -l file1.txt
5 file1.txt

```

And using the `-m` option will count the number of characters.

```sh
user:~$ cat file1.txt
hello
my
name
is
something maybe

user:~$ wc -m file1.txt
33 file1.txt

```

And `-w` will count the number of words.

```sh
user:~$ cat file1.txt
hello
my
name
is
something maybe

user:~$ wc -w file1.txt
6 file1.txt

```

Very exciting, I know.

### ls

We have covered this before, list the content of a directory.
We can use `-l` to use "long listing", which just means to print out more information such as file ownership.

Tip, use `-h` along `-l` to make the output human readable.

```sh
user:~$ ls
file1.txt  main.tex  my.zip  unsorted.txt

user:~$ ls -l
total 16
-rw-r--r-- 1 joao joao  33 Sep 26 13:20 file1.txt
-rw-r--r-- 1 joao joao   6 Sep 26 12:47 main.tex
-rw-r--r-- 1 joao joao 330 Sep 26 12:48 my.zip
-rw-r--r-- 1 joao joao   6 Sep 26 12:47 unsorted.txt

user:~$ ls -lh
total 16K
-rw-r--r-- 1 user user  33 Sep 26 13:20 file1.txt
-rw-r--r-- 1 user user   6 Sep 26 12:47 main.tex
-rw-r--r-- 1 user user 330 Sep 26 12:48 my.zip
-rw-r--r-- 1 user user   6 Sep 26 12:47 unsorted.txt

```

If we had sub directories, we could use `-R` to run `ls` recursively.

## Finding commands and manuals

### which

This command will locate a program. For example, `vi` is a command that will run the program called `vi`. But where does the program `vi` live?

user:~$ which vi
/usr/bin/vi
```

There is a special variable called `$PATH` that is a list of all directories in which programs live in. Part of this list is, obviously, `/usr/bin`.

For example, my own personal `$PATH` is (I am using a different shell):

```sh
❯ echo $PATH
/home/joao/.opam/ec-jasmin-stable/bin:/home/joao/.pyenv/shims:/usr/local/bin:/home/joao/.local/bin/:/home/joao/.opam/default/bin:/home/joao/Projects/me/ellsp/bin/:/home/joao/Projects/phd/formal-verification/easycrypt:/opt/piper-tts:/home/joao/.dotnet/tools:/usr/local/sbin:/usr/local/bin:/usr/bin:/var/lib/flatpak/exports/bin:/usr/lib/jvm/default/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl:/home/joao/.antigen/bundles/robbyrussell/oh-my-zsh/lib:/home/joao/.antigen/bundles/robbyrussell/oh-my-zsh/plugins/git:/home/joao/.antigen/bundles/zsh-users/zsh-syntax-highlighting:/home/joao/.antigen/bundles/zsh-users/zsh-autosuggestions:/home/joao/.antigen/bundles/zsh-users/zsh-history-substring-search:/home/joao/.antigen/bundles/robbyrussell/oh-my-zsh/plugins/bgnotify:/home/joao/.antigen/bundles/robbyrussell/oh-my-zsh/plugins/colorize:/home/joao/.antigen/bundles/robbyrussell/oh-my-zsh/plugins/extract:/home/joao/.antigen/bundles/robbyrussell/oh-my-zsh/plugins/starship:/home/joao/.antigen/bundles/Aloxaf/fzf-tab
```
Note how paths are separated by `:`. Messing up your `$PATH` permanently is a good way to brick your user (or machine if you mess with the root's `$PATH`).



### man

Shows the manual for a command.
I do not like this command because it is too verbose and is awful to read through. Tip! Install `tldr`, which is a much nicer way to navigate manuals.

Compare:

```sh
user:~$ man which

WHICH(1)                          General Commands Manual                         WHICH(1)

NAME
       which - shows the full path of (shell) commands.

SYNOPSIS
       which [options] [--] programname [...]

DESCRIPTION
       Which takes one or more arguments. For each of its arguments it prints to stdout
       the full path of the executables that would have been executed when this argument
       had been entered at the shell prompt. It does this by searching for an executable
       or script in the directories listed in the environment variable PATH using the same
       algorithm as bash(1).

       This man page is generated from the file which.texinfo.

OPTIONS
       --all, -a
           Print all matching executables in PATH, not just the first.

       --read-alias, -i
           Read  aliases  from stdin, reporting matching ones on stdout. This is useful in
           combination with using an alias for which itself. For example
           alias which=’alias | which -i’.
...
...
...
```

This is a lot. Compare with `tldr`:

```sh
user:~$ tldr which

  Locate a program in the user's path.
  More information: <https://manned.org/which>.

  Search the PATH environment variable and display the location of any matching executables:

      which executable

  If there are multiple executables which match, display all:

      which [-a|--all] executable
```

That is much nicer and helpful. See (https://github.com/tldr-pages/tldr-python-client)[https://github.com/tldr-pages/tldr-python-client].


## Information in file

### grep

This searches for a pattern within a file.

For example, searching for Mordor in the text of exercise 10:

```
user:~$ grep Mordor q2.txt
In the Land of Mordor where the Shadows lie.
In the Land of Mordor where the Shadows lie.

```

We can "invert" this search by printing lines where Mordor does not appear.

```
user:~$ grep -v Mordor q2.txt
Three Rings for the Elven-kings under the sky,
Seven for the dwarf-lords in their halls of stone,
Nine for Mortal Men doomed to die,
One for the Dark Lord on his dark throne,
One Ring to rule them all, One Ring to find them,
One Ring to bring them all and in the darkness bind them

```

This command is especially useful when piping output from another command, such as `ps` or `cat`. It is a very complicated and complex command, so it will take time to master.

The reason for this is that you can use regular expressions, which is another fun fun journey.

```
user:~$ grep "Mor*" q2.txt
Nine for Mortal Men doomed to die,
In the Land of Mordor where the Shadows lie.
In the Land of Mordor where the Shadows lie.

```

Another use is that I can grep the grep help page to find what certain options do:

```
user:~$ grep --help | grep '^ *-v'
  -v, --invert-match        select non-matching lines

```

### cut

This will print selected parts of lines of a file to the standard output.

We can choose to split our strings every time we see a `:` (we call this character a delimiter).

For example,

```
pine:253:221:1.2
oak:144:123:0.9
birch:92:83:1.6
yew:65:63:4.3
alder:12:5:9.6
```

This is delimited by `:`, so when running `cut`, we get four sub strings per line.

We can then choose to print out a selection of those sub strings. For example, if we want to print out the 1st sub string per line:

```sh
user:~$ cat trees.txt | cut -d ":" -f 1
pine
oak
birch
yew
alder

```

### sort

We have seen this before! Sorts inputs.

## File editing

### sed

We will focus on a single `sed` command because it is quite complex, namely `sed s/X/Y/`.

This is a stream editor that will filter and transform text. For example, to replace the world "hello" with "goodbye"

```sh
user:~$ cat moviescript.txt
hello my friend, I will see you later!

user:~$ cat moviescript.txt | sed 's/hello/goodbye/g'
goodbye my friend, I will see you later!

```

To save this edit, we have two options:

```sh
user:~$ cat moviescript.txt | sed 's/hello/goodbye/g' | tee something.txt
goodbye my friend, I will see you later!

user:~$ cat moviescript.txt
goodbye my friend, I will see you later!

```

`tee` will simply redirect input to standard in to standard out and files.

Or, ignoring `cat` entirely:

```sh
user:~$ cat moviescript.txt
hello my friend, I will see you later!

user:~$ sed -i 's/hello/goodbyte/g' moviescript.txt

user:~$ cat moviescript.txt
goodbye my friend, I will see you later!
```

Instead of `g`, we can use a number to specify that we want to replace the, for example, second instance of "hello".


## Archiving

### tar

I can never remember this one. We can use this to create a `tar` or `tar.gz` archive.

```sh
user:~$ ls
dir1       main.tex         my.zip  something      trees.txt
file1.txt  moviescript.txt  q2.txt  something.txt  unsorted.txt

user:~$ tar czf ../stuff.tar.gz dir1/dir2 q2.txt unsorted.txt

user:~$ ls ../
stuff.tar.gz
```

The `c` stands for create, `z` for gzip (the `.gz` at the end), and `f` means that we want to write the output to a file.

To extract it:
```sh
user:~$ mkdir extracted

user:~$ tar xvf ../stuff.tar.gz -C extracted/
dir1/dir2/
dir1/dir2/dir4/
dir1/dir2/dir4/g22.doc
dir1/dir2/f1.txt
dir1/dir2/h22.txt
dir1/dir2/g368.pdf
q2.txt
unsorted.txt

```

The `x` stands for extract, the `v` for verbose, and the `f` means we are working with an archive file.


### zip

Similar to `tar`, but only archives files.


```sh
user:~$ ls
dir1       main.tex         my.zip  something      trees.txt
file1.txt  moviescript.txt  q2.txt  something.txt  unsorted.txt

user:~$ zip ../stuff.zip dir1/dir2 q2.txt unsorted.txt
  adding: dir1/dir2/ (stored 0%)
  adding: q2.txt (deflated 48%)
  adding: unsorted.txt (stored 0%)

user:~$ ls ../
stuff.zip
```

To unzip, there exists the command `unzip`.
