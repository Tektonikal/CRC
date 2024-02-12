<a name="top"></a>
<h1 align="center">
CRC
</h1>
<h3 align="center">
Everything you need to know about Cylic Redundancy Checks
</h3>

### Table of contents:
- (i) &nbsp;Table of contents (You are here!)
- (ii) &nbsp;Intro to this project / Vision / Current progress
- (iii) Contributing
- (iv) Terminology (What the hell is a modulo?)
 1. Intro / The Idea Behind CRC (And why use them in the first place?)
2. Various starting approaches
- ...
- Credits / Outro (Braincells lost during writing)

By the time I'm done with this, I'm hoping that I'll have a repository (and maybe even a video, someday) that makes CRC easier to understand (and for the lazier fellas, easier to "implement" into your own code) and make the concept more accessible to newer developers. A good understanding of CRC can entirely change the way you think about your code and the underlying hardware you're working with. I'll add more yap here later, but for now, I need to get the basic outline done and add some implementations. This is inspired by [Ross William's paper on CRC](http://www.ross.net/crc), and I wanted to write my own, more modern and practical version of it.

---

### Contributing
---
If you'd like to contribute either changes to the document or code examples, feel free to make a pull request. If you need to contact me regarding something else, I have a [website](http://tektonik.al) with my contact information.

If you don't have anything to contribute, you should still consider supporting me and my work!

### Terminology
---

Checksum - Something derived from a block of data, in order to verify the data. Back in the day, data would be verified by taking the sum of all the byte values, which is how the checksum got its name. The term is now used in a wider range of applications wherever you need to verify data integrity (e.g. file hashes).

### 1. The Idea Behind CRC
---

Computers rarely make mistakes these days, but what good is information if it stays on your device? In this day and age, communication between devices is what makes the earth go round. That's where the problem forms: Computers rarely make mistakes internally, but when we try to transmit this information, it's very likely to be corrupted and modified on its way. So, how do we know we got the message in its original state?

There's a ton of ways how the recieved data can be corrupted, ranging from simple bit flips to chunks of the message being completely missed. There's a lot of approaches, but we need a method that meets the following criteria:
- The checksum needs to be as short as possible, to avoid being corrupted and to conserve bandwidth (That stuff doesn't grow on trees!)
- In the off chance that the message is corrupted, the odds that the corrupted message and checksum are valid needs to be as low as possible.
- It is also preferable that it's fast and easy to calculate and validate a checksum, since we need to include one with every message sent.
---

### 2. Various Starting Approaches



### Credits / Outro
---
I started writing this on February 9, 2024. At the time, I was trying to make a C program to parse PNG files as a way to pass the time in my boring classes. I've known about CRC for a long time, but never paid much mind until I attempted to verify it by hand in my program. A lot of research and exploration led me to this old and abandoned paper on the topic, which I finished reading on that fateful day. I decided that it would be nice to have a more modern version of the paper, and a repository of examples in various languages to be more accessible to begginers, so here we are.

["A Painless Guide to CRC Error Detection Algorithms" by Ross N. Williams](http://www.ross.net/crc/download/crc_v3.txt)

[Back to top](#top)