# Vending Machine Writeup
**Description:**


**Attachment:**
[Vending Machine](../Files/Vending%20Machine)

## Solution

We start by running the command `file` to know the type of our file:

    Vending Machine: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=2d99b78b90a5fc242b7da6319d5dbf4110e978f9, for GNU/Linux 3.2.0, stripped

It is a linux executable.

We use `IDA` to decompile our binary:

## **Step 1: Static Analysis**

Let's look at the code logic:

- The user starts with `$1000`.
- Menu options 1–6 are affordable, but option 7 (the flag) costs `$100,000,000`.
- If the user tries to buy option 7 without enough money, the program prints "Not enough money!" and continues.
- There is no way, through normal play, to reach $100,000,000.

---

## **Step 2: Looking for Bugs or Vulnerabilities**

We check each case for possible bugs:

### **Case 6: Potato Chips**
```c
case 6:
    v6 -= 30;
    printf("You bought Potato Chips!\n");
    break;
```
- **No check** if `money >= 30` before subtracting!
- If you keep buying Potato Chips after your money is less than $30, `money` will **underflow** (since `money` is `unsigned int`).

---

## **Step 3: Exploiting the Bug**

### **Unsigned Integer Underflow**

- In C, if you subtract from an unsigned integer below zero, it wraps around to a very large value.
- For example, if `money = 0` and you do `money -= 30;`, then `money` becomes `4294967266` (on a 32-bit system).

### **Attack Plan**

1. **Buy Potato Chips repeatedly** until your money underflows and becomes a huge value.
2. **Buy option 7** ("Something 7ibi toul ya kebdi")—now you have enough money!
3. The program will print the flag from `flag.txt`.

Following this approach when connect to the netcat will output the flag:

    FL1TZ{Tor9ed_Ma_Yar7mek_7ad}

***Author: OTC***