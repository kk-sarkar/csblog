---
layout: post
title: Character Encodings
excerpt: "Discussion on Character Encodings."
categories: [digital logic, character encodings, unicode, ascii, tutorial, binary]
tags:
      - digital logic
      - character encodings
      - unicode
      - ascii
      - tutorial
      - binary
comments: true
image:
  feature: /posts/character_encodings/1.png
  credit: Quora
  creditlink: https://qph.ec.quoracdn.net/main-qimg-1f98a83f3ffcb47b816497a356aeebac
---

### Introduction

Data is represented as **Octets** inside computers. Octets are also called **Bytes** which consist of ***8 bits*** or ***8 binary digits*** where each bit can have a single value **0** or **1**. So, in effect, data is stored or transmitted by computers in the form of a sequence of 8 bits, eight 0s, and 1s. Since there are 8 bits in an octet and each bit can store one of the 2 objects i.e. either 0 or 1, thus potentially we can have $$ \begin{align*} 2^{8} \end{align*} $$ or **256** *distinct combinations* of 0s and 1s i.e. 256 *distinct bit patterns*. An example of one of these 256-bit patterns would be the sequence 0000 0000 (eight 0s), 1111 1111(eight 1s), 1010 1010(a 1 followed by a 0, four times each) etc. A sequence of 8 bits by itself doesn't have any special meaning, for example, the sequence 1111 1111 is just eight 1s. But depending on the established conventions, one can interpret an octet like 1111 1111 to derive a meaning out of it. This process of mapping an octet or a bit pattern of fixed length to a human intelligible symbol or information is called **Encoding**. Think of it as a mapping table where the various octet patterns are key and the symbol is a value, so by querying the table using the key we will be able to retrieve the value represented by the octet. Since an octet can have at most 256 unique bit patterns, we can map 256 distinct symbols to the bit patterns in an octet, hence our mapping table will have just 256 entries. It is important to note that there can be multiple mapping tables which map a particular octet to different symbols depending on the rules of the particular mapping scheme.

Here, we are interested in **Character Encodings** i.e. the techniques by which we can map various human intelligible characters to equivalent  sequences of bits. Since computers can store and process information only in bits, so it is imperative to devise a rule by which texts, characters or  strings can be represented by bits which can then be processed by computers. Every letter, number or symbol that you see on screen, that you type  on keyboard or that you receive as an output from the computer, like the printed document etc. are internally represented in some binary  sequence of 0s and 1s. As discussed earlier, if we have an octet or 8 bits to represent a character, then we can represent only 256 different  characters.

##### What is a character?

In linguistics, a **grapheme** is the smallest unit of a writing system of any given language. The concept of graphemes is an abstract one. But in contrast, a specific shape that represents any particular grapheme in a specific **typeface** is called a **glyph**. Typeface represents a family of **fonts**, a particular set of writing designs to represent a symbol. So, a glyph is a concrete implementation of a grapheme. Similarly, in computing, a **character** is an atomic unit of information just like a grapheme. It could be letters, digits, special symbols, whitespaces etc. But it needs to be represented in a  computer in some binary form. Thus, we need Character Encoding. Character Encoding is a table that maps a character from a set of characters to some binary representation understood by a computer. Once the computer gets hold of the bit pattern representing a character and knows about the Encoding system that has been used to encode that character, then it can derive the character from the bit pattern, use one of the fonts installed in the system to print the glyph of the character on the monitor which is then visible to the user.

##### Some important definitions:

**Character Repertoire:** A set of distinct characters used for a particular purpose. For example, the English alphabets can form a Character repertoire. The repertoire per se does not even define an ordering for the characters; ordering for sorting and other purposes are to be specified separately. A character repertoire is usually defined by specifying names of characters and a sample (or reference) presentation of characters in visible form. A character repertoire may contain characters which look the same in some presentations but are regarded as logically distinct, such as Latin uppercase *A*, Cyrillic uppercase *A*, and Greek uppercase *A*.

**Character Code or Code Points:** A mapping, often presented in tabular form, which defines a one-to-one correspondence between characters in a character repertoire and a set of nonnegative integers is called the Character Code. That is, it assigns a unique numerical code, a code position, to each character in the repertoire. Each such unique mapping is called a Code Point. We can devise a system where the English character *A* maps to code point decimal *65* or hexadecimal *41*.


**Character Encoding:** An algorithm for presenting characters in digital form by mapping sequences of code points of characters into sequences of octets. In the simplest case, each character is mapped to an integer in the range *0-255* according to a character code and these are used as such as octets. Naturally, this only works for character repertoires with at most 256 characters. For larger sets, more complicated encodings are needed. Encodings have names,  which can be registered. Without an encoding scheme, an octet can best be described as a random sequence of 0s and 1s. Encodings give meaning to that sequence. The correct interpretation and processing of character data represented in binary format, therefore, requires knowledge about the encoding used.

![Character Repertoire to Code to Encoding Map]({{ site.url }}/img/posts/character_encodings/2.png)

Designing a character representation is a layered approach like the *OSI Networking Model*.

1. We define a set of usable characters called the **Character Repertoire** or the **Character Set**. In this stage, we group all the characters that we need.
2. Then we take the Character Set and map them to a set of *non-negative integers* called **Coded Character Set**. Each character from the Character Set is mapped to a unique integer called its **Code point**. The set of all possible code points is called the **Code space**.
3. The set of coded characters then needs to be mapped to a set of fixed length bit patterns called the **Character Encoding**.

### ASCII

**ASCII** stands for *"American Standard Code for Information Interchange"*. It denotes an old character repertoire, code, and encoding. The list of printable ASCII characters are as follows(starting with space, blank character):

![Printable ASCII characters]({{ site.url }}/img/posts/character_encodings/3.png)

ASCII uses 7 bits to encode characters. Hence, it can represent $$ \begin{align*} 2^{7} \end{align*} $$ or *128* distinct characters i.e. code points 0 to 127. In the character code defined by the ASCII standard, the code values are assigned to characters consecutively in the order in which the characters are listed, starting from *32* (assigned to the blank) and ending up with *126* (assigned to the tilde character ~). Positions 0 through 31 and 127 are reserved for *control codes* such as *linefeed(LF)* and *escape(ESC)*.  For example, the English alphabet *A* is mapped to decimal code *65* or hexadecimal code *41* and can be represented in binary as *100 0001*.

Since computers usually process information in the form of octets or 8 bits, sometimes the free 8th bit is used as a **parity checker bit** in some ASCII representations. The table below lists all ASCII characters:

![ASCII codes]({{ site.url }}/img/posts/character_encodings/4.png)

In this scheme, each character is encoded in a byte and hence *byte ordering* i.e. **Big Endian** or **Little Endian** is not an issue. This makes text files in ASCII format *platform independent* as the interpretation of characters are not dependent on the *Endianness* of the machine.

### ISO Latin 1 or ISO 8859-1

The **ISO 8859-1** standard defines a character repertoire as well as a character code for it. The repertoire contains the ASCII repertoire as a subset, and the code numbers for those characters are the same as in ASCII. The standard also specifies an encoding, which is similar to that of ASCII: each code number is presented simply as one *octet*. It uses *8* bits to encode character codes, hence it has the provision to encode $$ \begin{align*} 2^{8} \end{align*} $$ or *256* character codes. Thus, in addition to the ASCII characters, ISO Latin 1 contains various accented characters and other letters needed for writing languages of Western Europe, and some special characters. These characters occupy code positions *160-255*, and they are:

![ISO Latin 1 characters]({{ site.url }}/img/posts/character_encodings/5.png)

*ISO 8859-1* is a member of the **ISO 8859 family of character codes**. ISO 8859 family of character codes are a set of different extensions to the ASCII codes.

### Unicode or Universal Coded Character Set

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

**Note:** From HTML 4 onwards, all characters are internally converted to Unicode by the browser. Although the page can use any encoding the browsers convert the characters to its equivalent Unicode code points internally for processing. This means that any character that is not part of the Unicode repertoire cannot be processed by the browser.


##### Bibliography
[W3C](https://www.w3.org/International/questions/qa-what-is-encoding){:target="_blank"}

[ASCII](https://ascii.cl/references.htm){:target="_blank"}

[ISO Latin](https://www.iso.org/standard/28245.html){:target="_blank"}

[Unicode](http://www.unicode.org){:target="_blank"}
