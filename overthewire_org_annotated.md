# OVERTHEWIRE
> [overthewire.org/wargames/bandit](https://overthewire.org/wargames/bandit)

> ⚠️ No level passwords are published in this file, per [OverTheWire's rules for educators/content-creators](https://overthewire.org/rules/): "Do not publish credentials to any of the games." Every step below shows the exact command that reveals the password on your own connection — the terminal output just marks where it would appear.
>
> Credit: all wargame content, levels, and infrastructure belong to the [OverTheWire community](https://overthewire.org/). If you find this useful, consider [donating](https://overthewire.org/information/donate.html) to support them.
---
## Bandit Level 0
> The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.
- ssh bandit0@bandit.labs.overthewire.org -p 2220
```
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walkthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: [REDACTED]
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

ssh (Secure Shell) opens an encrypted remote login session to another machine. user@host sets who you're logging in as and where; -p <port> overrides the default SSH port (22) with a custom one — Bandit uses 2220 instead of the standard port.
ls (list) prints the contents of the current directory, so you can see what files are available to work with.
cat (concatenate) prints the full contents of a file to the terminal. It's the simplest way to read a small text file.

## Bandit Level 1 → Level 2
> Level Goal: The password for the next level is stored in a file called - located in the home directory
- ssh bandit1@bandit.labs.overthewire.org -p 2220
```
bandit1@bandit:~$ ls -altr
total 24
-rw-r--r--   1 root    root     807 Feb 13  2026 .profile
-rw-r--r--   1 root    root     220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root    root    3851 Jun 24 14:50 .bashrc
-rw-r-----   1 bandit2 bandit1   33 Jun 24 14:58 -
drwxr-xr-x   2 root    root    4096 Jun 24 14:58 .
drwxr-xr-x 150 root    root    4096 Jun 24 15:02 ..
bandit1@bandit:~$ cat ./-
[REDACTED]
bandit1@bandit:~$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

ls -altr combines four flags: -a shows hidden files (those starting with a dot), -l uses the "long" listing format (permissions, owner, group, size, date), -t sorts by modification time, and -r reverses the sort order (oldest first). Together they give a detailed, chronologically-ordered view, including dotfiles.
A file literally named - is tricky because most command-line tools interpret a leading - as the start of a flag rather than a filename. Prefixing it with ./ (meaning "in the current directory") tells the shell to treat it as a path, not an option, so cat ./- reads the file safely.

## Bandit Level 2 → Level 3
> Level Goal: The password for the next level is stored in a file called --spaces in this filename-- located in the home directory
- ssh bandit2@bandit.labs.overthewire.org -p 2220
```
bandit2@bandit:~$ ls -altr
total 24
-rw-r--r--   1 root    root     807 Feb 13  2026 .profile
-rw-r--r--   1 root    root     220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root    root    3851 Jun 24 14:50 .bashrc
-rw-r-----   1 bandit3 bandit2   33 Jun 24 14:59 --spaces in this filename--
drwxr-xr-x   2 root    root    4096 Jun 24 14:59 .
drwxr-xr-x 150 root    root    4096 Jun 24 15:02 ..
bandit2@bandit:~$ cat ./"--spaces in this filename--"
[REDACTED]
bandit2@bandit:~$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

The shell splits commands into arguments on whitespace by default, so a filename containing spaces would otherwise be read as several separate arguments. Wrapping the name in quotes ("...") tells the shell to treat everything inside as a single argument.
Combining ./ with quotes (./"--spaces in this filename--") solves two problems at once: ./ stops the leading - from being read as a flag, and the quotes preserve the embedded spaces.

## Bandit Level 3 → Level 4
> Level Goal: The password for the next level is stored in a hidden file in the inhere directory.
- ssh bandit3@bandit.labs.overthewire.org -p 2220
```
bandit3@bandit:~$ ls -altr
total 24
-rw-r--r--   1 root root  807 Feb 13  2026 .profile
-rw-r--r--   1 root root  220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root root 3851 Jun 24 14:50 .bashrc
drwxr-xr-x   3 root root 4096 Jun 24 14:59 .
drwxr-xr-x   2 root root 4096 Jun 24 14:59 inhere
drwxr-xr-x 150 root root 4096 Jun 24 15:02 ..
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls -altr
total 12
drwxr-xr-x 3 root    root    4096 Jun 24 14:59 ..
-rw-r----- 1 bandit4 bandit3   33 Jun 24 14:59 ...Hiding-From-You
drwxr-xr-x 2 root    root    4096 Jun 24 14:59 .
bandit3@bandit:~/inhere$ cat ./"...Hiding-From-You"
[REDACTED]
bandit3@bandit:~/inhere$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

cd <directory> (change directory) moves your current working location in the filesystem into that folder, so subsequent commands act on its contents.
"Hidden" files on Linux are simply files whose name starts with a dot (.); they're skipped by a plain ls but revealed by ls -a. This is a naming convention, not a security feature — anyone can still read them if permissions allow.

## Bandit Level 4 → Level 5
> Level Goal: The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the "reset" command.
- ssh bandit4@bandit.labs.overthewire.org -p 2220
```
bandit4@bandit:~$ ls -altr
total 24
-rw-r--r--   1 root root  807 Feb 13  2026 .profile
-rw-r--r--   1 root root  220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root root 3851 Jun 24 14:50 .bashrc
drwxr-xr-x   3 root root 4096 Jun 24 14:59 .
drwxr-xr-x   2 root root 4096 Jun 24 14:59 inhere
drwxr-xr-x 150 root root 4096 Jun 24 15:02 ..
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ ls -altr
total 48
drwxr-xr-x 3 root    root    4096 Jun 24 14:59 ..
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file00
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file01
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file02
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file03
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file04
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file05
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file06
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file07
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file08
-rw-r----- 1 bandit5 bandit4   33 Jun 24 14:59 -file09
drwxr-xr-x 2 root    root    4096 Jun 24 14:59 .
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: OpenPGP Secret Key
./-file03: data
./-file04: data
./-file05: data
./-file06: Non-ISO extended-ASCII text, with NEL line terminators
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$ cat ./-file07
[REDACTED]
bandit4@bandit:~/inhere$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

file <target> inspects a file's actual content (not just its name/extension) and reports what type of data it appears to hold — e.g. "ASCII text", "data" (binary/unknown), or a specific format like "OpenPGP Secret Key". This is done by checking the file's structure/"magic bytes", so it works even on misleadingly-named files.
The ./* wildcard (glob) expands to every file in the current directory, letting file check them all in a single command instead of one at a time.
"Human-readable" means plain text you can view directly in a terminal, as opposed to binary data that would show up as garbled characters.

## Bandit Level 5 → Level 6
> Level Goal: The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
    - human-readable
    - 1033 bytes in size
    - not executable
- ssh bandit5@bandit.labs.overthewire.org -p 2220
```
bandit5@bandit:~$ ls -altr
total 24
-rw-r--r--   1 root root     807 Feb 13  2026 .profile
-rw-r--r--   1 root root     220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root root    3851 Jun 24 14:50 .bashrc
drwxr-xr-x   3 root root    4096 Jun 24 14:59 .
drwxr-x---  22 root bandit5 4096 Jun 24 14:59 inhere
drwxr-xr-x 150 root root    4096 Jun 24 15:02 ..
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ ls -altr
total 88
drwxr-xr-x  3 root root    4096 Jun 24 14:59 ..
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere00
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere01
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere02
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere03
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere04
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere05
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere06
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere07
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere08
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere09
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere10
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere11
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere12
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere13
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere14
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere15
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere16
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere17
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere18
drwxr-x--- 22 root bandit5 4096 Jun 24 14:59 .
drwxr-x---  2 root bandit5 4096 Jun 24 14:59 maybehere19
bandit5@bandit:~/inhere$ find . -type f -size 1033c ! -executable -exec file '{}' \; | grep ASCII
./maybehere07/.file2: ASCII text, with very long lines
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
[REDACTED]
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

find <path> <options> recursively searches a directory tree for files or directories matching given criteria. Here, . means "start searching from the current directory", -type f restricts results to regular files, -size 1033c matches files that are exactly 1033 bytes (c = bytes), ! -executable negates the executable test so it only matches non-executable files, and -exec file '{}' \; runs the file command on each match individually ({} is replaced with the found file's path, and \; terminates the -exec clause).
The pipe | sends the output of one command as input to the next; grep ASCII then filters that output down to only lines mentioning "ASCII", isolating the human-readable candidate from the rest.
Together, this chains a precise multi-criteria search with a content-type filter, instead of manually checking every file in 19+ subdirectories.

## Bandit Level 6 → Level 7
> Level Goal: The password for the next level is stored somewhere on the server and has all of the following properties:
    - owned by user bandit7
    - owned by group bandit6
    - 33 bytes in size
- ssh bandit6@bandit.labs.overthewire.org -p 2220
```
bandit6@bandit:~$ ls -altr
total 20
-rw-r--r--   1 root root  807 Feb 13  2026 .profile
-rw-r--r--   1 root root  220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root root 3851 Jun 24 14:50 .bashrc
drwxr-xr-x   2 root root 4096 Jun 24 14:58 .
drwxr-xr-x 150 root root 4096 Jun 24 15:02 ..
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
[REDACTED]
bandit6@bandit:~$
```
> We add 2>/dev/null so it filters out all the permission errors

*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

Starting the search at / (the filesystem root) tells find to search the entire filesystem, not just the home directory.
-user bandit7 and -group bandit6 filter results by file ownership metadata — every file on Linux records which user owns it and which group owns it, independent of the file's name or location.
Every command produces two separate output streams: stdout (standard output, normal results) and stderr (standard error, error messages). 2>/dev/null redirects stream 2 (stderr) to /dev/null, a special "black hole" device that discards anything written to it. This suppresses the flood of "Permission denied" errors you'd get scanning / as a low-privilege user, leaving only the actual matches visible.

## Bandit Level 7 → Level 8
> Level Goal: The password for the next level is stored in the file data.txt next to the word millionth
- ssh bandit7@bandit.labs.overthewire.org -p 2220
```
bandit7@bandit:~$ ls -altr
total 4108
-rw-r--r--   1 root    root        807 Feb 13  2026 .profile
-rw-r--r--   1 root    root        220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root    root       3851 Jun 24 14:50 .bashrc
drwxr-xr-x   2 root    root       4096 Jun 24 14:59 .
-rw-r-----   1 bandit8 bandit7 4184396 Jun 24 14:59 data.txt
drwxr-xr-x 150 root    root       4096 Jun 24 15:02 ..
bandit7@bandit:~$ cat data.txt | grep millionth
millionth       [REDACTED]
bandit7@bandit:~$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Grep can be used to search lines that contain a specific pattern like follow grep < pattern > . With the pipe (|), we can pipe the output of cat to grep as input to look through a text file.

> Theory

grep <pattern> (global regular expression print) scans input text line by line and prints only the lines that contain a match for the given pattern. It's one of the most fundamental text-searching tools in Unix.
cat data.txt | grep millionth is a classic pipeline: cat streams the whole file's contents out, and the pipe (|) feeds that stream directly into grep as its input, which then filters it down to just the line(s) containing "millionth". (For a single file this could also be written more efficiently as grep millionth data.txt, skipping cat entirely.)

## Bandit Level 8 → Level 9
> Level Goal: The password for the next level is stored in the file data.txt and is the only line of text that occurs only once
- ssh bandit8@bandit.labs.overthewire.org -p 2220
```
bandit8@bandit:~$ ls -altr
total 56
-rw-r--r--   1 root    root      807 Feb 13  2026 .profile
-rw-r--r--   1 root    root      220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root    root     3851 Jun 24 14:50 .bashrc
-rw-r-----   1 bandit9 bandit8 33033 Jun 24 14:59 data.txt
drwxr-xr-x   2 root    root     4096 Jun 24 14:59 .
drwxr-xr-x 150 root    root     4096 Jun 24 15:02 ..
bandit8@bandit:~$ sort data.txt | uniq -u
[REDACTED]
bandit8@bandit:~$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

sort reorders the lines of its input alphabetically (or numerically with the right flag). This matters because uniq only detects duplicate lines when they're adjacent to each other — it doesn't compare the whole file at once, so the input must be sorted first.
uniq filters out repeated adjacent lines. The -u flag inverts this to show only the lines that appear exactly once (unique lines), discarding everything that has any duplicate.
Chaining sort data.txt | uniq -u is a very common idiom for finding "the odd one out" in a large list of otherwise-repeated values.

## Bandit Level 9 → Level 10
> Level Goal: The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several '=' characters.
- ssh bandit9@bandit.labs.overthewire.org -p 2220
```
bandit9@bandit:~$ ls -altr
total 40
-rw-r--r--   1 root     root      807 Feb 13  2026 .profile
-rw-r--r--   1 root     root      220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-rw-r-----   1 bandit10 bandit9 19382 Jun 24 14:58 data.txt
drwxr-xr-x   2 root     root     4096 Jun 24 14:58 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
bandit9@bandit:~$ strings data.txt | grep ===
cL0========== the
========== password
>========== is
R========== [REDACTED]
bandit9@bandit:~$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

data.txt here is mostly binary (non-text) data, so cat would print unreadable garbage. strings scans a binary file and extracts only the sequences of printable characters embedded within it — a standard technique for pulling readable text out of otherwise non-text files.
grep === then filters that extracted text down to only the lines containing three or more consecutive = characters, matching the pattern described in the level goal and narrowing thousands of lines down to a handful of candidates.

## Bandit Level 10 → Level 11
> Level Goal: The password for the next level is stored in the file data.txt, which contains base64 encoded data
- ssh bandit10@bandit.labs.overthewire.org -p 2220
```
bandit10@bandit:~$ ls -altr
total 24
-rw-r--r--   1 root     root      807 Feb 13  2026 .profile
-rw-r--r--   1 root     root      220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-rw-r-----   1 bandit11 bandit10   69 Jun 24 14:58 data.txt
drwxr-xr-x   2 root     root     4096 Jun 24 14:58 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
bandit10@bandit:~$ cat data.txt
[REDACTED - base64 blob]
bandit10@bandit:~$ base64 -d data.txt
The password is [REDACTED]
bandit10@bandit:~$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

Base64 is an encoding scheme that represents arbitrary binary data using only 64 printable ASCII characters (A–Z, a–z, 0–9, +, /), often padded with = at the end. It's not encryption — it carries no secrecy, just a safe way to transmit or store binary-ish data as plain text (e.g. in emails, URLs, or config files).
base64 is the command-line tool for encoding/decoding it. The -d flag means "decode": it takes base64 text as input and converts it back to its original readable form.

## Bandit Level 11 → Level 12
> Level Goal: The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
- ssh bandit11@bandit.labs.overthewire.org -p 2220
```
bandit11@bandit:~$ ls -altr
total 24
-rw-r--r--   1 root     root      807 Feb 13  2026 .profile
-rw-r--r--   1 root     root      220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root     root     3851 Jun 24 14:50 .bashrc
-rw-r-----   1 bandit12 bandit11   49 Jun 24 14:58 data.txt
drwxr-xr-x   2 root     root     4096 Jun 24 14:58 .
drwxr-xr-x 150 root     root     4096 Jun 24 15:02 ..
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is [REDACTED]
bandit11@bandit:~$
```
*(Password omitted — see [OverTheWire's rules](https://overthewire.org/rules/): credentials from the games are not published here. Run the command above yourself after connecting to see it.)*

> Theory

This describes ROT13, a simple substitution cipher that shifts every letter 13 places through the alphabet — because the alphabet has 26 letters, applying it twice returns the original text, so it's its own inverse. It provides no real security; it's traditionally used to obscure text like spoilers, not to protect secrets.
tr <set1> <set2> (translate) replaces each character found in set1 with the corresponding character at the same position in set2. tr 'A-Za-z' 'N-ZA-Mn-za-m' maps A-Z onto N-Z,A-M (a 13-place rotation) and does the same for lowercase a-z, which is exactly what's needed to both encode and decode ROT13 text.
