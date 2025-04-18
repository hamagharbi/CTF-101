# Not Normal Strings Writeup
**Description:**
It's not the easy one you know Use Decode

**Attachment:**
[Not Normal Strings](../Files/Not%20Normal%20Strings)

## Solution

We start by running the command `file` to know the type of our file:

    Not Normal Strings: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=320e4ee2f7a4cb29c7e7a5922a74ca7d9d620735, for GNU/Linux 3.2.0, not stripped

We notice that our file is a linux executable.

We run our program:

    ==   Not   N O R M A L   S t r i n g s  ==
    --  Author: O T C  --
    I have a secret for you...
    My favourite number is 13
    I know it is a curious number
    That's it!
    Also don't forget to upgrade from 8 to 16
    Bye bye!

The program contains some hints to find the flag.

**My favourite number is 13** ==> The flag is encrypted in ROT13

**Also don't forget to upgrade from 8 to 16** ==> The flag is encoded in UTF-16 Little Endian.

All we have to do is to use `strings` with additional options. We combine it with `-e l main | tr 'A-Za-z` to reveal our flag.

    strings -e l  Not\ Normal\ Strings | tr 'A-Za-z' 'N-ZA-Mn-za-m'

After that we got our flag:

    FL1TZ{WaywA_WaywA_WaywA}

***Author: OTC***