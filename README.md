<a name="top"></a>
<h1 align="center">
CRC
</h1>
<h3 align="center">
Everything you need to know about Cyclic Redundancy Checks
</h3>

### Table of contents:
- (i) &nbsp;Table of contents (You are here!)
- (ii) &nbsp;Intro to this project / Vision / Current progress
- (iii) Contributing
- (iv) Glossary
 1. Intro / The Idea Behind CRC (And why use them in the first place?)
2. Various Starting Approaches
3. CRC at Its Core
- ...
- Credits / Outro (Braincells lost during writing)

By the time I'm done with this, I'm hoping that I'll have a repository (and maybe even a video, someday) that makes CRC easier to understand (and for the lazier fellas, easier to "implement" into your own code) and make the concept more accessible to newer developers. A good understanding of CRC can entirely change the way you think about your code and the underlying hardware you're working with. I'll add more yap here later, but for now, I need to get the basic outline done and add some implementations. This is inspired by [Ross William's paper on CRC](http://www.ross.net/crc), and I wanted to write my own, more modern and practical version of it.

---

### Contributing
---
If you'd like to contribute either changes to the document or code examples, feel free to make a pull request. If you need to contact me regarding something else, I have a [website](http://tektonik.al) with my contact information.

If you don't have anything to contribute, you should still consider supporting me and my work!

### Glossary
---

Checksum - A block of data derived from an input block of data, used in order to verify the data's integrity. (e.g., file hashes). The name "check-sum" allegedly comes from early days of error-checking where it was verified by checking the sum of the values. Nowadays, the term is used to describe any error-checking value.

### 1. The Idea Behind CRC
---

Computers rarely make mistakes these days, but what good is information if it stays on your device? In this day and age, communication between devices is what makes the earth go round. That's where the problem comes in: Computers rarely make mistakes internally, but when we try to transmit this information, it could be corrupted and modified on its way by interference. So, how do we know we got the message in its original state?

There's a ton of ways how the recieved data can be corrupted, ranging from simple bit flips to chunks of the message being completely missed. There's a lot of approaches, but we need a method that meets the following criteria:
- The checksum needs to be as short as possible, to avoid being corrupted itself and to conserve bandwidth (That stuff doesn't grow on trees!)
- In the off chance that the message is corrupted, the odds that the corrupted message and checksum are valid needs to be as low as possible.
- It is also preferable that it's fast and easy to calculate and validate a checksum, since we need to include one with every message sent.

For the sake of this article, we'll restrict ourselves to only appending whatever data we want to the start or end of the message. There are algorithms that insert data into the payload of the message in order to check for errors later, but that's way more convoluted and isn't how CRC works.

---

### 2. Various Starting Approaches

Note to self: I might have to include a `\0` in the message to signify the ending of it to make for a more practical example. Oh well, that's a problem for future me to worry about!


Let's establish a message that we'll use for the rest of this document:

    Binary:
    01001000 01100101 01101100 01101100 01101111 00100000 01110111 01101111 01110010 01101100 01100100 00100001
    
    Decimal:
    72 101 108 108 111 32 119 111 114 108 100 33

    Hex:
    48 65 6C 6C 6F 20 77 6F 72 6C 64 21

    ASCII:
    Hello world!

This message is 12 bytes, so let's try to come up with a algorithm that can fit into 4 bytes or less, making for a total of 16 bytes for the message.

There's also probably a lot of mistakes I'm making by representing bits this way, womp womp!

The first approach that comes to mind, and has allegedly been used in the early days of error-checking, is simply taking the sum of the values. If the sum doesn't match, we can say that say that something went wrong and the message got corrupted, and request it to be sent again. Using the message above, we get a sum of 1117. That value can fit into 11 bits, which would work, because we have 4 to spare. however, what if the value is too big to fit into four bytes? What if we want to fit our value into only one byte? To combat this, we can use modulo (`%`), and see the remainder from dividing by 256, which is the maximum value a byte can hold. After doing `1117 % 256`, we get 93, which can be encoded into one bit, no matter how big our message / sum gets. However, there are drawbacks to doing this. Shown in the examples below, are some way that the message can get corrupted yet be verified as valid.

    In decimal:

        Example 1: Message value + / - 256, checksum still valid

            Message:
            72 101 108 108 111 32 119 111 114 108 100 33 (93)
            Checksum (Without modulo):
            1117
            Checksum (With modulo):
            93
            Corrupted message:
            0 101 108 108 111 0 0 111 114 108 100 0 (93)
            ^                 ^ ^                 ^
            Checksum (Without modulo):
            861
            Checksum (With modulo):
            Still 93!

        Example 2: Message value and checksum get increased by the same amount, message retains validity

            Message:
            72 101 108 108 111 32 119 111 114 108 100 33 (93)
            Checksum (Without modulo):
            1117
            Checksum (With modulo):
            93
            Corrupted Message:
            72 101 108 108 111 32 119 111 114 108 100 34 (94)
                                                       ^   ^
            Checksum (Without modulo):
            1118, but it's valid!
            Checksum (With modulo):
            94, still valid!

In theory, the odds of the message getting corrupted in these specific ways are low, but when we're sending very big messages or a large volume of them, these are odds we don't want to risk, especially the second example. One thing we really want to avoid in our algorithm is having the least significant bit(s) of the message having the least effect on the checksum. Thankfully, the CRC algorithm does not rely on overly complicated steps and instead uses the next layer in mathmatical operations: divison.

---

### 3. CRC at Its Core

At the most basic level, CRC just consists of reading our message as one big number and dividing it by some other big number. The remainder can then be used as the checksum. 

---

### Credits / Outro

---

I started writing this on February 9, 2024. At the time, I was trying to make a C program to parse PNG files as a way to pass the time in my boring classes. I've known about CRC for a long time, but never paid much mind until I attempted to verify it by hand in my program. A lot of research and exploration led me to this old and abandoned paper on the topic, which I finished reading on that fateful day. I decided that it would be nice to have a more modern version of the paper, and a repository of examples in various languages to be more accessible to begginers, so here we are.

["A Painless Guide to CRC Error Detection Algorithms" by Ross N. Williams](http://www.ross.net/crc/download/crc_v3.txt)

[Back to top](#top)