---
layout: post
title: Unicode Encoding Exercises
excerpt: "Exercises to refresh Unicode Encodings."
categories: [digital logic, tutorial, character encodings, unicode, utf]
tags:
      - digital logic
      - tutorial
      - character encodings
      - unicode
      - utf
comments: true
image:
  feature: /posts/unicode/1.png
  credit: Wikipedia
  creditlink: https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Unicode_logo.svg/1200px-Unicode_logo.svg.png
---

We had discussed about the Character Encodings, Unicode character sets and UTF encodings in previous posts. Let us enhance our understanding of the UTF encoding using some exercises.

#### Exercises:

- Find the UTF-8 representation of code point U+0041.

  U+0041 stands for code point 41. In binary, 41 can be written as 100 0001 whose size is 7-bits. A 7-bit number in the UTF-8 can be stored in a single byte as **0xxxxxxx**, hence U+0041 can be stored in UTF-8 as 01000001 or 41 in hex.

- Find the UTF-8 representation of code point U+05D0.

  U+05D0 stands for code point 5D0. In binary, 5D0 can be written as 101 1101 0000 whose size is 11-bits. An 11-bit number in the UTF-8 can be stored in 2 bytes as **110xxxxx 10xxxxxx**. The first byte is called the **leading byte**, while the second byte is called the **continuation byte**. We take the actual value 10111010000, prepend a 0 to make it a 12-bit number: 010111010000, then we split it equally into 2 parts: 010111 and 010000, 6-bits each. We take the last 6-bits and put them into the 2nd byte and we take the first 6-bits and put them into the first byte in the places of *x*. We get the UTF-8 representation of U+0500: 110010111 10010000 or D7 90 in hex.

- Find the UTF-8 representation of code point U+597D.

  U+597D stands for code point 597D. In binary, 597D can be written as 101 1001 0111 1101 whose size is 15-bits. A 15-bit number in the UTF-8 can be stored in 3 bytes as **1110xxxx 10xxxxxx 10xxxxxx**. The first byte is called the **leading byte**, followed by two **continuation bytes**. We take the actual value 101100101111101, prepend a 0 to make it a 16-bit number: 0101100101111101, then we split it into 3 parts of 4-6-6 bytes to get 0101 100101 111101. We take the last 6-bits and put them into the 3rd byte, we take the middle 6-bits and put them into the second byte and then we take the first 4-bits and put them into the first byte in the places of *x*. We get the UTF-8 representation of U+597D: 11100101 10100101 10111101 or E5 A5 BD in hex.

- Find the UTF-8 representation of code point U+233B4.

  U+233B4 stands for code point 233B4. In binary, 233B4 can be written as 10 0011 0011 1011 0100 whose size is 18-bits. A 18-bit number in the UTF-8 can be stored in 4 bytes as **1110xxxx 10xxxxxx 10xxxxxx 10xxxxxx**. The first byte is called the **leading byte**, followed by three **continuation bytes**. We take the actual value 100011001110110100, prepend a three 0s to make it a 21-bit number: 000100011001110110100, then we split it into 4 parts of 3-6-6-6 bytes to get 000 100011 001110 110100. We take the last 6-bits and put them into the 4th byte, we take the second last 6-bits and put them into the 3rd byte, we take the third last 6-bits and put them into the 2nd byte and then we take the first 3-bits and put them into the first byte in the places of *x*. We get the UTF-8 representation of U+597D: 11110000 10100011 10001110 10110100 or F0 A3 8E B4 in hex.

- Find the UTF-16 representation of code point U+0041.

  U+0041 stands for code point 41. In binary, 41 can be written as 100 0001 whose size is 7-bits. A 7-bit number in the UTF-16 can be stored in a 2 bytes as **00000000 0xxxxxxx** in a big-endian machine and **0xxxxxxx 00000000** in a little-endian machine, hence U+0041 can be stored in UTF-16BE as 00000000 01000001 or 00 41 in hex and in UTF-16LE as 01000001 00000000 or 41 00 in hex.

- Find the UTF-16 representation of code point U+05D0.

  U+05D0 stands for code point 5D0. In binary, 5D0 can be written as 101 1101 0000 whose size is 11-bits. An 11-bit number in the UTF-16 can be stored in a 4 bytes as **00000xxx xxxxxxxx** in a big-endian machine and **xxxxxxxx 00000xxx** in a little-endian machine, hence U+05D0 can be stored in UTF-16BE as 00000101 11010000 or 05 D0 in hex and in UTF-16LE as 11010000 00000101 or D0 05 in hex.

- Find the UTF-16 representation of code point U+597D.

  U+597D stands for code point 597D. In binary, 597D can be written as 101 1001 0111 1101 whose size is 15-bits. A 15-bit number in the UTF-16 can be stored in a 4 bytes as **0xxxxxxx xxxxxxxx** in a big-endian machine and **xxxxxxxx 0xxxxxxx** in a little-endian machine, hence U+597D can be stored in UTF-16BE as 01011001 01111101 or 59 7D in hex and in UTF-16LE as 01111101 01011001 or 59 7D in hex.

- Find the UTF-16 representation of code point U+233B4.

  U+233B4 stands for code point 233B4. In binary, 233B4 can be written as 10 0011 0011 1011 0100 whose size is 18-bits. If the binary representation of the code point exceeds 16 bits, then it cannot be stored within 2-bytes in the UTF-16 system. We need to store them in 4 bytes in a particular pattern of two 2-byte numbers. The first 2-bytes are called the **High Surrogate** and the next 2-bytes are called the **Low Surrogate**. The format must strictly follow this pattern where the High Surrogates will be followed by the Low Surrogates. Each Surrogate consists of 2-bytes and the endianness of the machine determines how the two bytes within each Surrogate is ordered but endianness of the machine doesn't determine the order of the Surrogates themselves. It is always High Surrogate followed by the Low Surrogate.

  We take the code point *X*, *Subtract hexadecimal number 10000* from it to get some code point *Y*. *Y* will always be a *20-bit number*. We split the *20-bit Y* into *two 10-bit numbers A* and *B*. We add *D800* to *A* to get the High Surrogate *A'* and then we add *DC00* to *B* to get the Low Surrogate *B'*. The number is thus stored as *A' B'*.

  Thus, we take the code point 233B4 and subtract 10000 from it, we get 133B4. Binary of 133B4 is 0001 0011 0011 1011 0100. We split this into 2 equal parts of 10-bits each 0001001100 1110110100 i.e. 4C and 3B4. We add D800 to the first part 4C to get the High Surrogate D8 4C or 11011000 01001100, then we add DC00 to the second part 3B4 to get the Low Surrogate DF B4 or 11011111 10110100. So our UTF-16 representation will be the combination of the Higher Surrogate followed by the Lower Surrogate i.e. D8 4C DF B4.

  Now, this can be represented in UTF-16BE as D8 4C DF B4 or 11011000 01001100 11011111 10110100, in UTF-16LE as 4C D8 B4 DF or 01001100 11011000 10110100 11011111.

- Find the UTF-32 representation of code point U+0041.

  U+0041 stands for code point 41. In binary, 41 can be written as 100 0001 whose size is 7-bits. A 7-bit number in the UTF-32 can be stored in a 4 bytes as **00000000 00000000 00000000 0xxxxxxx** in a big-endian machine and **0xxxxxxx 00000000 00000000 00000000** in a little-endian machine, hence U+0041 can be stored in UTF-32BE as 00000000 00000000 00000000 01000001 or 00 00 00 41 in hex and in UTF-32LE as 01000001 00000000 00000000 00000000 or 41 00 00 00 in hex.

- Find the UTF-32 representation of code point U+05D0.

  U+05D0 stands for code point 5D0. In binary, 5D0 can be written as 101 1101 0000 whose size is 11-bits. An 11-bit number in the UTF-32 can be stored in a 4 bytes as **00000000 00000000 00000xxx xxxxxxxx** in a big-endian machine and **xxxxxxxx 00000xxx 00000000 00000000** in a little-endian machine, hence U+05D0 can be stored in UTF-32BE as 00000000 00000000 00000101 11010000 or 00 00 05 D0 in hex and in UTF-32LE as 11010000 00000101 00000000 00000000 or D0 05 00 00 in hex.

- Find the UTF-32 representation of code point U+597D.

  U+597D stands for code point 597D. In binary, 597D can be written as 101 1001 0111 1101 whose size is 15-bits. A 15-bit number in the UTF-32 can be stored in a 4 bytes as **00000000 00000000 0xxxxxxx xxxxxxxx** in a big-endian machine and **xxxxxxxx 0xxxxxxx 00000000 00000000** in a little-endian machine, hence U+597D can be stored in UTF-32BE as 00000000 00000000 01011001 01111101 or 00 00 59 7D in hex and in UTF-32LE as 01111101 01011001 00000000 00000000 or 59 7D 00 00 in hex.

- Find the UTF-32 representation of code point U+233B4.

  U+233B4 stands for code point 233B4. In binary, 233B4 can be written as 10 0011 0011 1011 0100 whose size is 18-bits. A 18-bit number in the UTF-32 can be stored in a 4 bytes as **00000000 000000xx xxxxxxxx xxxxxxxx** in a big-endian machine and **xxxxxxxx xxxxxxxx 000000xx 00000000** in a little-endian machine, hence U+233B4 can be stored in UTF-32BE as 00000000 00000010 00110011 10110100 or 00 02 33 B4 in hex and in UTF-32LE as 10110100 00110011 00000010 00000000 or B4 33 02 00 in hex.
