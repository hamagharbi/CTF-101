# Basic Strings Writeup
**Description:**

my name is enough !!!!

**Attachment:**
[Basic Strings](../Files/Basic%20Strings)

## Solution

We start by running the command `file` to know the type of our file:

    Basic Strings: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=1c5d4cda69c759c64eb791bc78735659339c6ced, for GNU/Linux 3.2.0, not stripped

We notice that our file is a linux executable.

We run our program:

    Just type strings bro

You just need to use the `strings` command to reveal it. You can also combine it with the `grep` command to find the flag faster: `strings welcome | grep FL1TZ`.

    FL1TZ{Strings_1s_Y0ur_Best_Fr1end}

***Author: OTC***