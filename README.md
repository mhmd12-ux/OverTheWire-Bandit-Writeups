# 🛡️ OverTheWire Bandit: Cybersecurity & Linux CLI Write-ups

Welcome to the **OverTheWire Bandit** write-ups repository. This project documents hands-on solutions, Linux command-line mechanics, data manipulation techniques, and foundational security concepts learned while completing Levels 0 through 13.

---

## 📌 Table of Contents
- [📊 Progress Tracker](#-progress-tracker)
- [🛠️ Environment & Tooling](#️-environment--tooling)
- [📝 Detailed Level Writeups](#-detailed-level-writeups)
  - [Level 00](#level-00)
  - [Level 00 → 01](#level-00--01)
  - [Level 01 → 02](#level-01--02)
  - [Level 02 → 03](#level-02--03)
  - [Level 03 → 04](#level-03--04)
  - [Level 04 → 05](#level-04--05)
  - [Level 05 → 06](#level-05--06)
  - [Level 06 → 07](#level-06--07)
  - [Level 07 → 08](#level-07--08)
  - [Level 08 → 09](#level-08--09)
  - [Level 09 → 10](#level-09--10)
  - [Level 10 → 11](#level-10--11)
  - [Level 11 → 12](#level-11--12)
  - [Level 12 → 13](#level-12--13)
- [⚠️ Disclaimer](#️-disclaimer)

---

## 📊 Progress Tracker

| Level | Goal / Key Concept | Key Commands | Writeup Link | Status |
| :---: | :--- | :--- | :---: | :---: |
| **00** | Initial SSH Server Connection | `ssh` | [Writeup](#level-00) | ✅ Completed |
| **00 → 01** | Reading standard files in home directory | `ls`, `cat` | [Writeup](#level-00--01) | ✅ Completed |
| **01 → 02** | Reading files named with a dash (`-`) | `cat ./-` | [Writeup](#level-01--02) | ✅ Completed |
| **02 → 03** | Handling filenames with spaces | `cat "file name"` | [Writeup](#level-02--03) | ✅ Completed |
| **03 → 04** | Inspecting hidden files and directories | `ls -la`, `cat` | [Writeup](#level-03--04) | ✅ Completed |
| **04 → 05** | Identifying human-readable file types | `file`, `cat` | [Writeup](#level-04--05) | ✅ Completed |
| **05 → 06** | Advanced file search by size and properties | `find`, `cat` | [Writeup](#level-05--06) | ✅ Completed |
| **06 → 07** | System-wide search & error redirection | `find /`, `2>/dev/null` | [Writeup](#level-06--07) | ✅ Completed |
| **07 → 08** | Text filtering and keyword matching | `grep` | [Writeup](#level-07--08) | ✅ Completed |
| **08 → 09** | Extracting unique, non-repeated lines | `sort`, `uniq -u` | [Writeup](#level-08--09) | ✅ Completed |
| **09 → 10** | Extracting printable strings from binary data | `strings`, `grep` | [Writeup](#level-09--10) | ✅ Completed |
| **10 → 11** | Decoding Base64 encoded data | `base64 -d` | [Writeup](#level-10--11) | ✅ Completed |
| **11 → 12** | Decrypting ROT13 letter rotation cipher | `tr` | [Writeup](#level-11--12) | ✅ Completed |
| **12 → 13** | Multi-layered binary & archive decompression | `xxd`, `gzip`, `bzip2`, `tar` | [Writeup](#level-12--13) | ✅ Completed |

---

## 🛠️ Environment & Tooling

* **Operating System:** Linux (Linux Mint / Debian-based OS)
* **Shell Environment:** GNU Bash
* **Primary Utilities Used:** `ssh`, `grep`, `find`, `sort`, `uniq`, `strings`, `base64`, `tr`, `xxd`, `file`, `gzip`, `bzip2`, `tar`

---

## 📝 Detailed Level Writeups

### Level 00
* **Goal:** Connect to the remote OverTheWire Bandit server via SSH on non-standard port `2220`.
* **Commands Executed:**
  ```bash
  ssh bandit0@bandit.labs.overthewire.org -p 2220
  ```
* **Key Takeaway:** Establishing a secure remote shell session using OpenSSH with explicit port targeting (`-p`).

---

### Level 00 → 01
* **Goal:** Retrieve the password stored in the `readme` file located in the user's home directory.
* **Commands Executed:**
  ```bash
  ls -la
  cat readme
  ```
* **Key Takeaway:** Basic directory listing with `ls` and inspecting text contents using `cat`.

---

### Level 01 → 02
* **Goal:** Read the content of a file named `-` located in the home directory.
* **Commands Executed:**
  ```bash
  cat ./-
  ```
* **Key Takeaway:** Utilizing relative paths (`./`) to prevent the CLI parser from interpreting `-` as a standard input stream or option flag.

---

### Level 02 → 03
* **Goal:** Read a file containing spaces in its filename (`spaces in this filename`).
* **Commands Executed:**
  ```bash
  cat "spaces in this filename"
  # Alternative approach using backslash escapes:
  cat spaces\ in\ this\ filename
  ```
* **Key Takeaway:** Escaping special characters and whitespace characters in Bash using double quotes or backslashes.

---

### Level 03 → 04
* **Goal:** Locate and read a hidden file inside the `inhere` directory.
* **Commands Executed:**
  ```bash
  cd inhere
  ls -la
  cat .hidden
  ```
* **Key Takeaway:** Hidden dotfiles (files starting with `.`) require the `-a` flag in `ls` to be uncovered.

---

### Level 04 → 05
* **Goal:** Identify the only human-readable ASCII text file stored in the `inhere` directory among multiple binary files.
* **Commands Executed:**
  ```bash
  cd inhere
  file ./*
  cat ./-file07
  ```
* **Key Takeaway:** Using the `file` command to analyze Magic Bytes and determine exact file types regardless of extension.

---

### Level 05 → 06
* **Goal:** Search `inhere` for a file that is 1033 bytes in size, non-executable, and human-readable.
* **Commands Executed:**
  ```bash
  find inhere -type f -size 1033c ! -executable -exec cat {} \;
  ```
* **Key Takeaway:** Combining multiple conditions in `find` (size specs `1033c`, logic negation `! -executable`) and passing matches directly to `cat` via `-exec`.

---

### Level 06 → 07
* **Goal:** Search the root filesystem (`/`) for a file owned by user `bandit7`, group `bandit6`, and exactly 33 bytes in size.
* **Commands Executed:**
  ```bash
  find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
  cat /var/lib/dpkg/info/bandit7.password
  ```
* **Key Takeaway:** System-wide searching using ownership attributes, coupled with Standard Error redirection (`2>/dev/null`) to suppress access permission errors.

---

### Level 07 → 08
* **Goal:** Extract the password in `data.txt` located next to the keyword `millionth`.
* **Commands Executed:**
  ```bash
  grep "millionth" data.txt
  ```
* **Key Takeaway:** Fast pattern matching and line filtering within large datasets using `grep`.

---

### Level 08 → 09
* **Goal:** Extract the single unique line of text in `data.txt` that is not repeated.
* **Commands Executed:**
  ```bash
  sort data.txt | uniq -u
  ```
* **Key Takeaway:** Sorting data streams with `sort` prior to invoking `uniq -u` (which requires adjacent identical lines to filter effectively).

---

### Level 09 → 10
* **Goal:** Extract printable strings preceded by several `=` characters from binary file `data.txt`.
* **Commands Executed:**
  ```bash
  strings data.txt | grep "="
  ```
* **Key Takeaway:** Stripping non-printable binary junk using `strings` and piping stdout into `grep` for pattern filtering.

---

### Level 10 → 11
* **Goal:** Decode Base64 encoded text stored in `data.txt`.
* **Commands Executed:**
  ```bash
  base64 -d data.txt
  ```
* **Key Takeaway:** Decoding Base64 data streams using standard GNU coreutils (`base64 -d`).

---

### Level 11 → 12
* **Goal:** Decrypt text encrypted using a ROT13 letter rotation substitution cipher.
* **Commands Executed:**
  ```bash
  cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
  ```
* **Key Takeaway:** Stream character translation and alphabet mapping via `tr`.

---

### Level 12 → 13
* **Goal:** Reconstruct a file from a Hexdump (`xxd -r`) and recursively decompress it through multiple nested compression layers (`gzip`, `bzip2`, `tar`).
* **Workflow & Commands Executed:**
  ```bash
  # Step 1: Create an isolated workspace in /tmp
  mkdir /tmp/myworkspace12 && cd /tmp/myworkspace12
  cp /home/bandit12/data.txt .
  
  # Step 2: Reverse hexdump to binary
  xxd -r data.txt > data1
  
  # Step 3: Inspect file signature and decompress iteratively
  file data1
  mv data1 data1.gz && gzip -d data1.gz
  
  file data1
  mv data1 data1.bz2 && bzip2 -d data1.bz2
  
  file data1
  tar -xf data1
  # Repeat file inspection and matching extraction until ASCII text is retrieved
  ```
* **Key Takeaway:** Reversing hexdumps, analyzing signatures with `file`, and handling multi-format archive decompression workflows.

---

##  Disclaimer

In accordance with ethical write-up standards for cybersecurity wargames, all actual passwords and flags have been strictly redacted. This repository focuses solely on methodology, command mechanics, and problem-solving logic.
