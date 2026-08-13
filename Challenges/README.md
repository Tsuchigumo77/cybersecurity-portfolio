# SpookyPass - Hack The Box

## Overview

This write-up documents my approach to completing the **SpookyPass** challenge on Hack The Box.

The challenge involved analyzing a Linux executable and identifying information embedded within the binary. I used basic Linux commands and static analysis techniques to inspect the executable and locate the required information.

## Tools Used

* Kali Linux
* Linux Terminal
* `unzip`
* `file`
* `strings`
* `cat`

---

## 1. Extracting the Challenge Files

The challenge was provided as a ZIP archive. I initially attempted to extract it without elevated privileges:

```bash
unzip spookypassst.zip
```

After encountering an issue, I extracted the archive using:

```bash
sudo unzip Spookypassst.zip
```

This created the following directory:

```text
rev_spookypass/
```

Inside the directory was a file named:

```text
pass
```

![Challenge Extraction](challenge_extration.png)

---

## 2. Identifying the File

I first checked the file type using the `file` command:

```bash
file pass
```

The output identified the file as:

```text
ELF 64-bit LSB pie executable, x86-64
```

This indicated that `pass` was a compiled Linux executable rather than a normal text file.

![File Identification](Spookypass_1.png)

---

## 3. Executing the Program

I executed the program with:

```bash
./pass
```

The program displayed the following message:

```text
Welcome to the SPOOKIEST party of the year.
Before we let you in, you'll need to give us the password:
```

This indicated that the executable required a password before providing access.

![Program Execution](Spookypass_1.png)

---

## 4. Inspecting the Binary with `strings`

Since the file was an executable, using `cat` would not be an effective way to examine it.

Instead, I used:

```bash
strings pass
```

The `strings` command extracts sequences of readable characters from binary files. This allowed me to identify readable text that was embedded within the executable.

Among the output, I discovered the password:

```text
s3cr3t_p455_f0r_gh0st5_4nd_gh0ul5
```

I also noticed other readable strings from the program, including its welcome message.

![Strings Output](screenshots/Spookypass_2.png)

---

## 5. Retrieving the Flag

Further examination of the output from `strings` revealed the flag embedded within the executable:

```text
HTB{un0bfusc4t3d_5tr1ng5}
```

This demonstrated that sensitive information can sometimes be recovered directly from compiled programs when it has been stored as readable strings.

![Flag Discovery](Spookypass_3.png)

---

## 6. Why `cat` Produced Gibberish

I also tested:

```bash
cat pass
```

The output contained a large amount of unreadable characters and binary data.

This happened because `pass` is an ELF executable rather than a text file. The `cat` command simply attempts to print the file's raw contents to the terminal, so compiled machine code and binary data appear as garbled characters.

![Binary Output](Spookypass_4.png)

Using:

```bash
strings pass
```

was more appropriate because it searches the binary for sequences of printable characters.

---

## Lessons Learned

This challenge reinforced several important concepts:

* How to identify Linux executables using the `file` command.
* The difference between text files and compiled binaries.
* How `strings` can be used during basic binary reconnaissance.
* Why sensitive information stored directly in an executable can potentially be recovered.
* The importance of choosing the appropriate tool based on the type of file being analyzed.

### Key Takeaway

Even without performing advanced reverse engineering, basic static analysis can reveal useful information from an executable. Commands such as `file` and `strings` are simple but valuable tools when beginning an investigation of an unknown binary.

## Disclaimer

This write-up documents work performed in a legal and controlled Hack The Box lab environment for educational and cybersecurity training purposes.

