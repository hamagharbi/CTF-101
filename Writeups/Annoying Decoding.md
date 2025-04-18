# Annoying Decoding Writeup
**Description:**

Decode Decode Decode until you reach your destination.

look for cyberchef

**Attachment:**
[task](../Files/task.py)

## Solution

We notice that our file is a python file. It shows us an obfuscated code in one line. The user must enter the correct flag.

How we can understand this obfuscated code and tranform it into an understandable python code ??

After reading this python line and understanding it, we notice that our python file takes a playload and decode it 5 times in base64 and execute the python code.

All we have to do is decoding our playload in base64. We can use python to decode the playload or use an online decoder like `cyberchef` or `dcode`.

After doing that we get this python code:
```python
    def fun1(text, a, b):
    str=""
    for i in range(len(text)):
        if text[i].isalpha():
            if text[i].isupper():
                str+=chr((a*(ord(text[i])-65)+b)%26+65)
            else:
                str+=chr((a*(ord(text[i])-97)+b)%26+97)
        else:
            str+=text[i]
    return str
    def fun2(text, key):
        str=""
        j=0
        for i in range(len(text)):
            if text[i].isalpha():
                if text[i].isupper():
                    str+=chr((ord(text[i])-65+ord(key[j%len(key)])-65)%26+65)
                    j+=1
                else:
                    str+=chr((ord(text[i])-97+ord(key[j%len(key)])-65)%26+97)
                    j+=1
            else:
                str+=text[i]
        return str
    def main():
        print("==  A N N O Y I N G  D E C O D I N G  ==")
        print("--  Author: O T C  --")
        flag=input("Enter the flag: ")
        e1=fun1(flag, 2, 1)
        print("Encoded flag: ", e1)
        e2=fun2(e1, "REVERSE")
        if e2.__eq__("CB1ID{A4f_Po33j_A0fyh3n_Y3j0c3}"):
            print("Correct flag!")
        else:
            print("Incorrect flag!")
        
    if __name__ == "__main__":
        main()
```
This python code takes an input and encrypt it with a function called `fun1` then encrypt it with another function `fun2` using a key "REVERSE".

After analysing `fun1` and `fun2` we notice that `fun1` encrypts a string in an **affine cipher** with parameters a=2 and b=1.
`fun2` encrypts as string in **vigenere cipher** with the key "REVERSE".

Unfortunately, there was a mistake in this challenge. In fact the affine cipher can not be reversed when a=2 becuase a must be comprime with 26 (number of characters of the alphabet), so I included the option of a second flag when brute forcing the affince cipher to decrypt it.

We can use python to get our flag or an online decoder like `cyberchef` or `dcode`.
```python
    def inv_fun2(text, key):
    # Inverse of fun2: subtract the shift determined by the key from each letter.
    result = ""
    j = 0
    for c in text:
        if c.isalpha():
            # Determine shift from key (all key letters are considered uppercase)
            shift = ord(key[j % len(key)]) - 65
            if c.isupper():
                # Reverse the shift for uppercase letters.
                new_val = (ord(c) - 65 - shift) % 26
                result += chr(new_val + 65)
            else:
                # Reverse the shift for lowercase letters.
                new_val = (ord(c) - 97 - shift) % 26
                result += chr(new_val + 97)
            j += 1
        else:
            result += c
    return result

    def fun1(text, a, b):
    result = ""
    for i in range(len(text)):
        if text[i].isalpha():
            if text[i].isupper():
                result += chr((a * (ord(text[i]) - 65) + b) % 26 + 65)
            else:
                result += chr((a * (ord(text[i]) - 97) + b) % 26 + 97)
        else:
            result += text[i]
    return result

    def reverse_char(c, a, b):
        # Try all a-z and A-Z and see which could have produced c
        candidates = []
        for ch in range(65, 91):  # A-Z
            if fun1(chr(ch), a, b) == c:
                candidates.append(chr(ch))
        for ch in range(97, 123):  # a-z
            if fun1(chr(ch), a, b) == c:
                candidates.append(chr(ch))
        # Non-alpha characters are unchanged
        if not c.isalpha():
            candidates.append(c)
        return candidates

    cipher="CB1ID{A4f_Po33j_A0fyh3n_Y3j0c3}"
    intermediate=inv_fun2(cipher,"REVERSE")
    result = []
    for i, c in enumerate(intermediate):
        possible = reverse_char(c, a, b)
        result.append(possible)

    # Display results
    for i, chars in enumerate(result):
        print(f"{i:02d}: {intermediate[i]} <- {chars}")
```
After running our solver we got:
   
    00: L <- ['F', 'S']
    01: X <- ['L', 'Y']
    02: 1 <- ['1']
    03: N <- ['G', 'T']
    04: Z <- ['M', 'Z']
    05: { <- ['{']
    06: J <- ['E', 'R']
    07: 4 <- ['4']
    08: n <- ['g', 't']
    09: _ <- ['_']
    10: L <- ['F', 'S']
    11: x <- ['l', 'y']
    12: 3 <- ['3']
    13: 3 <- ['3']
    14: f <- ['c', 'p']
    15: _ <- ['_']
    16: F <- ['C', 'P']
    17: 0 <- ['0']
    18: b <- ['a', 'n']
    19: h <- ['d', 'q']
    20: p <- ['h', 'u']
    21: 3 <- ['3']
    22: j <- ['e', 'r']
    23: _ <- ['_']
    24: H <- ['D', 'Q']
    25: 3 <- ['3']
    26: f <- ['c', 'p']
    27: 0 <- ['0']
    28: h <- ['d', 'q']
    29: 3 <- ['3']
    30: } <- ['}']

You can either verify this challenge with 2 flags:
    
    FL1TZ{E4t_Sl33p_C0nqu3r_D3c0d3}
    FL1TZ{R4t_Fl33p_P0ndh3r_D3p0d3}

***Author: OTC***