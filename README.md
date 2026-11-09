# OverTheWire-Bandit-Writeups
Sloving bandits from OverTheWire

Welcome to the **OverTheWire Bandit** challenge write-ups repository. This project documents hands-on solutions, Linux command-line techniques, and foundational cybersecurity concepts learned while solving levels 0 through 13.

---
## 📊 Progress Tracker

| Level | Goal / Key Concept | Key Commands | Solution Writeup | Status |
| :---: | :--- | :--- | :---: | :---: |
| **00** | Initial SSH Server Connection | `ssh` | [Writeup](#level-00) | ✅ |
| **00 → 01** | Reading standard files in home directory | `ls`, `cat` | [Writeup](#level-00--01) | ✅ |
| **01 → 02** | Reading files named with a dash (`-`) | `cat ./-` | [Writeup](#level-01--02) | ✅ |
| **02 → 03** | Handling filenames with spaces | `cat "file name"` | [Writeup](#level-02--03) | ✅ |
| **03 → 04** | Inspecting hidden files and directories | `ls -la`, `cat` | [Writeup](#level-03--04) | ✅ |
| **04 → 05** | Identifying human-readable file types | `file`, `cat` | [Writeup](#level-04--05) | ✅ |
| **05 → 06** | Advanced file search by size and properties | `find`, `cat` | [Writeup](#level-05--06) | ✅ |
| **06 → 07** | System-wide search & error redirection | `find /`, `2>/dev/null` | [Writeup](#level-06--07) | ✅ |
| **07 → 08** | Text filtering and keyword matching | `grep` | [Writeup](#level-07--08) | ✅ |
| **08 → 09** | Extracting unique, non-repeated lines | `sort`, `uniq -u` | [Writeup](#level-08--09) | ✅ |
| **09 → 10** | Extracting printable strings from binary data | `strings`, `grep` | [Writeup](#level-09--10) | ✅ |
| **10 → 11** | Decoding Base64 encoded data | `base64 -d` | [Writeup](#level-10--11) | ✅ |
| **11 → 12** | Decrypting ROT13 letter rotation cipher | `tr` | [Writeup](#level-11--12) | ✅ |
| **12 → 13** | Multi-layered binary & archive decompression | `xxd`, `gzip`, `bzip2`, `tar` | [Writeup](#level-12--13) | ✅ |
---

## 📝 Level Writeups & Walkthroughs

Level 00 → 01
Goal: Read the password stored in the readme file located in the home directory.

Commands:
ls
cat readme
Concept: Directory inspection with ls and reading standard text file contents using cat.

Level 01 → 02
Goal: Read a file named - located in the home directory.

Commands:
cat ./-
Concept: Using relative paths (./) to prevent cat from interpreting - as an option flag or standard input stream.

Level 02 → 03
Goal: Read a file containing spaces in its name (spaces in this filename).

Commands:
cat "spaces in this filename"
OR
cat spaces\ in\ this\ filename
Concept: Escaping whitespace characters using double quotes ("") or backslashes (\) in Bash syntax.

Level 03 → 04
Goal: Read a hidden file located inside the inhere directory.

Command
cd inhere
ls -la
cat .hidden
Concept: Uncovering hidden dotfiles starting with . using ls -la.

Level 04 → 05
Goal: Find the human-readable text file stored in the inhere directory among multiple binary files.

Commands:
cd inhere
file ./*
cat ./-file07
Concept: Inspecting raw file types and encoding formats using the file utility.

Level 05 → 06
Goal: Find a file inside inhere matching specific criteria (1033 bytes, non-executable, human-readable).

Commands:
find inhere -type f -size 1033c ! -executable -exec cat {} \;
Concept: Advanced querying using find with size filters (1033c), attribute negation (!), and inline command execution (-exec).

Level 06 → 07
Goal: Find a file anywhere on the server owned by user bandit7, group bandit6, and exactly 33 bytes in size.

Commands:
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
Concept: Searching root directories with ownership attributes while suppressing I/O permission errors via stderr redirection 2>/dev/null.

Level 07 → 08
Goal: Find the password in data.txt next to the word millionth.

Commands:
grep "millionth" data.txt
Concept: Pattern matching and text filtering using grep.

Level 08 → 09
Goal: Extract the only non-repeated line of text from data.txt.

Commands:
sort data.txt | uniq -u
Concept: Alphabetical stream sorting with sort combined with uniq -u via Unix piping (|) to isolate unique strings.

Level 09 → 10
Goal: Extract the printable string preceded by several = characters from binary file data.txt.

Commands:
strings data.txt | grep "="
Concept: Extracting ASCII strings from raw binary files using strings coupled with grep.
Level 10 → 11
Goal: Decode Base64 encoded text in data.txt.

Commands:
base64 -d data.txt
Concept: Decoding Base64 data streams using base64 -d.

Level 11 → 12
Goal: Decrypt text encrypted using the ROT13 substitution cipher.

Commands:
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
Concept: Character translation and 13-position alphabet rotation using tr.

Level 12 → 13
Goal: Recursively decompress a hexdump file compressed using multiple formats (gzip, bzip2, tar).

Workflow:
mkdir /tmp/myworkspace12 && cd /tmp/myworkspace12
cp /home/bandit12/data.txt .
xxd -r data.txt > data1
file data1
# Rename extension according to file signature and decompress iteratively
mv data1 data1.gz && gzip -d data1.gz
bzip2 -d data2.bz2
tar -xf data3.tar
cat data_final.txt
Concept: Reversing hexdumps using xxd -r, analyzing file signatures using file, and handling multi-layered archive extraction.
