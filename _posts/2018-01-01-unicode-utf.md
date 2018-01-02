---
layout: post
title: Unicode and UTF
excerpt: "Introduction to Unicode and UTF."
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

**Unicode** is a character repertoire. It is just a character set, but currently, the most comprehensive and dominant character set in the world. It defines the different set of characters and also maps each of these distinct characters from different sets of unique *non-negative integers* or *code points*. Unicode can be implemented by different character encodings called **UTF( Unicode Transformation Format)**. It is important to note that the *first 127 code points* are exactly the same as is defined in ASCII system.

Unicode uses multiple encoding schemes unlike ASCII, hence the English alphabet *A* whose code point is *65* in decimal and *41* in hexadecimal can be encoded into various bit patterns depending on the Encoding schemes used. It can be encoded using a single byte, double bytes or multiple bytes. The representations can be *big-endian* or *little-endian*. The code point is an integer in the range of *0* to *10FFFF* hexadecimal numbers. It is usually written prefixed by **“U+”**, and is made of four to six hexadecimal digits(0-padded), for example, *U+0041*, *U+05D0* etc.

The full set of code points is organised into *17 sets of 64k characters each*. Each such set is called a **Plane**. The first $$ \begin{align*} 2^{16} \end{align*} $$  or *65536* code points actually covers almost all the frequently used characters. It is called the **Basic Multilingual Plane(BMP)**. The next million or so integers are used to map other characters referred to as **Supplementary characters**. The full list of planes are:

**Plane 0 (U+0000 - U+FFFF)** is called the **Basic Multilingual Plane(BMP)** and it contains the most frequent characters. Since it maps to hexadecimals of *4 digits* viz. *0000* to *FFFF*, it can be encoded using *4* bytes at maximum.

**Plane 1 (U+10000 - U+1FFFF)** is called the **Supplementary Multilingual Plane(SMP)** and it contains infrequently used scripts, such as *Deseret*.

**Plane 2 (U+20000 - U+2FFFF)** is called the **Supplementary Ideographic Plane(SIP)** and it contains ideographic characters, most of which are infrequent.

**Planes 3 to 13 (U+30000 - U+DFFFF)** are currently unassigned i.e. not in use.

**Plane 14 (U+E0000 - U+EFFFF)** is called the **Supplementary Special-purpose Plane(SSP)**.

Planes 1 to 14 maps to hexadecimals of *5 digits* viz. *10000* to *EFFFF*, we need at least *5* bytes to encode the code points in these planes.

**Planes 15 and 16 (U+F0000 - U+10FFFF)** are **Private Use planes**. We need *5* or *6* bytes to encode the code points in these planes.

The most commonly used encoding schemes are **UTF-8**, **UTF-16**, and **UTF-32**. Depending on the encoding scheme used a Unicode code point can be represented by 1, 2 or 4 bytes or sometimes up to 6 bytes.

The illustration below shows the UTF encodings of some of the Unicode code points:

![UTF encodings]({{ site.url }}/img/posts/unicode/2.png)

#### UTF-8

**UTF-8** uses 1 byte to represent characters in the ASCII set, two bytes for characters in several more alphabetic blocks, and three bytes for the rest of the BMP. Supplementary characters use 4 bytes. So UTF-8 can use 1, 2, 3 or 4 bytes depending on the character. It can be extended up to 6 bytes. The encoding scheme of UTF-8 can be understood from the following illustration:

![UTF-8 encodings]({{ site.url }}/img/posts/unicode/3.png)

The most important thing to note here is that UTF-8 uses a single byte to store characters with code points 0-127. This makes it *backward compatible with ASCII*. So any ASCII text can be represented in UTF-8 encoding literally without any transformation or without any change in storage space. Another thing to notice is that UTF-8 encoding scheme doesn't depend on the endianness of the machine as the ordering of the bytes in a multi-byte UTF-8 encoding is fixed. The bytes need to be written and read in the same order as defined in the specification.

#### UTF-16

**UTF-16** uses 2 bytes for any character in the BMP, and 4 bytes for supplementary characters. So effectively it uses a chunk of 2 bytes for BMP characters and two chunks of 2 bytes each for supplementary characters. It is a variable length character set as the width of a character is not fixed.  We know that all the English alphabets have code points between 0-127, it takes less than a byte to store the binary representation of integers 0-127 but when encoded in UTF-16 format each character will be stored in 2 bytes hence the size of a simple English text file will increase 2 times. ASCII text files are incompatible with this character encoding as each character using 1 byte in ASCII encoding needs to be mapped again to 2-byte UTF-16 format. So some amount of transformation is required from plain ASCII text file to a UTF-16 text file having the same set of ASCII characters. The English alphabet A has Unicode code point U+0041. It has to represented as a sequence of 2 bytes in UTF-16.

Since UTF-16 uses at least 2 bytes to express a character code point, the endianness of the machine reading or writing the character becomes important. UTF-16 is based 16-bit code units. There are two ways to encode a 16-bit value in a byte stream: most significant byte first *(big endian)* or least significant byte first *(little endian)*. That's why there are two flavours of UTF-16 byte streams, typically called UTF-16-BE and UTF-16-LE. Remember, **Big-endian** means that the *most significant byte* or *MSB(Big)* occupies the lowest address of the memory or lowest byte index(end) being used. While **Little-Endian** means that the *least significant digit* or *LSB(Little)* occupies the lowest address of the memory or lowest byte index(end) being used. For example, code point U+0041 needs 2-bytes in UTF-16 encoding. Hexadecimal 41 can be represented as 00000000 01000001 in 16-bits or 2-bytes.

| Byte Index | Byte 0 | Byte 1 |
|:--------|:-------|:-------|
| Big endian(Hex) | 00  | 41 |
| Little endian(Hex)  | 41 | 00 |
| Big endian(Binary)  | 00000000  | 01000001 |
| Little endian(Binary)  | 01000001  | 00000000 |
|----

If the binary representation of the code point exceeds 16 bits, then it cannot be stored within 2-bytes in the UTF-16 system. We need to store them in 4 bytes in a particular pattern of two 2-byte numbers. The first 2-bytes are called the **High Surrogate** and the next 2-bytes are called the **Low Surrogate**. The format must strictly follow this pattern where the High Surrogates will be followed by the Low Surrogates. Each Surrogate consists of 2-bytes and the endianness of the machine determines how the two bytes within each Surrogate is ordered but endianness of the machine doesn't determine the order of the Surrogates themselves. It is always High Surrogate followed by the Low Surrogate.

We take the code point *X*, *Subtract hexadecimal number 10000* from it to get some code point *Y*. *Y* will always be a *20-bit number*. We split the *20-bit Y* into *two 10-bit numbers A* and *B*. We add *D800* to *A* to get the High Surrogate *A'* and then we add *DC00* to *B* to get the Low Surrogate *B'*. The number is thus stored as *A' B'*.

#### UTF-32

**UTF-32** uses 4 bytes for all characters. The problems with UTF-32 is same as that of UTF-16, mainly, incompatibility with existing ASCII formatted text files and also this encoding takes 4 times as much space as a plain ASCII encoding scheme because each byte in ASCII is converted to equivalent 4-byte code points in UTF-32. Most computers today are based on a 32 bit or 64-bit architecture, so this allows computers to manipulate Unicode values as a whole computer **"word"** of 32 bits on 32-bit architectures, or as a half computer "word" of 32 bits on 64-bit architectures. Hence, UTF-32 allows for fast computation on 32 bit and 64-bit computers.

For example, code point U+0041 needs 4-bytes in UTF-16 encoding. Hexadecimal 41 can be represented as 00000000 00000000 00000000 01000001 in 32-bits or 4-bytes.

| Byte Index | Byte 0 | Byte 1 | Byte 2 | Byte 3 |
|:--------|:-------|:-------|:-------|:-------|
| Big endian(Hex) | 00  | 00 | 00 | 41 |
| Little endian(Hex)  |  41  | 00 | 00 | 00 |
| Big endian(Binary)  | 00000000  | 00000000 | 00000000  | 01000001 |
| Little endian(Binary)  | 01000001  | 00000000 | 00000000  | 00000000 |
|----

#### BOM

**BOM** stands for **Byte Order Mark**. It is a set of predefined characters that help text parsers to determine the endianness of the UTF encoding used. In order to decide if a text uses Big-Endian or Little-Endian, the specification recommends prepending a BOM to the string. BOM can also be used in the UTF-8 scheme but it's redundant in UTF-8 as UTF-8 is endian independent. BOM tells the program consuming the text that the endianness of the encoding and also confirms that the encoding used is of a particular type of UTF. In UTF-16 and UTF-32, A BOM **(U+FEFF)** may be placed as the first character of a file or character stream to indicate the endianness of all the 16-bit code units of the file or stream. If an attempt is made to read this stream with the wrong endianness, the bytes will be swapped, thus delivering the character **U+FFFE**, which is defined by Unicode as a "non-character" that should never appear in the text. UTF-8 representation of the BOM is the hexadecimal byte sequence **0xEF,0xBB,0xBF**.

| Bytes(Hex)| Encoding Form |
|:--------|:-------|
| EF BB BF | UTF-8  |
| FE FF | UTF-16 Big Endian or UTF-16 BE  |
| FF FE | UTF-16 Little Endian or UTF-16 LE  |
| 00 00 FE FF | UTF-32 Big Endian or UTF-32 BE  |
| FF FE 00 00 | UTF-32 Little Endian or UTF-32 LE  |
|----

For example, the text "Example" can be encoded in ASCII as "69 120 97 109 112 108 101" in decimal or "45 78 61 6d 70 6c 65" in hexadecimal. In Unicode, it represents code points "U+045 U+078 U+061 U+06d U+070 U+06c U+065". We shall now see how this simple string "Example" is encoded in ASCII and various UTF encodings:

~~~ascii
Byte Index:  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31
-------------------------------------------------------------------------------------------------------------------------------------------
ASCII:       45 78 61 6d 70 6c 65
UTF-16BE:    FE FF 00 45 00 78 00 61 00 6d 00 70 00 6c 00 65
UTF-16LE:    FF FE 45 00 78 00 61 00 6d 00 70 00 6c 00 65 00
UTF-32BE:    00 00 FE FF 00 00 00 45 00 00 00 78 00 00 00 61 00 00 00 6d 00 00 00 70 00 00 00 6c 00 00 00 65
UTF-32LE:    FF FE 00 00 45 00 00 00 78 00 00 00 61 00 00 00 6d 00 00 00 70 00 00 00 6c 00 00 00 65 00 00 00

~~~

In the next post, we shall look at some examples to understand the procedures involved in encoding code points in UTF-8, UTF-16 and UTF-32 formats.

##### Bibliography

[Unicode](http://www.unicode.org){:target="_blank"}
