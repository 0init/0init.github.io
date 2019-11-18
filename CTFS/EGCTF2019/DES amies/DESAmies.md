---
layout: post
title:  "DESAmies is a Cryptography challenge"
description: write-up by Abdallah EL-Shinbary 
tags: first blog
img: /CTFS/EGCTF2019/DES amies/1.png
---

### First of all we have a network connection that asks	for input and gives us what looks like encrypted message.

<img src="./first.png" >

### It's just that nothing.
Looking at the challenge name I assume that It's DES encryption and we have to decrypt the message somehow.

Honestly I wasn't able to solve it without the hint which says "Strong key".</p>
Hmmmm if there is a strong key then there is a weak key.</p>

After some googling I discovered that DES implementation has some 'weak keys' that if you encrypt with them twice you will get the original message

### "Original" ---enc---> "Encrypted" ---enc---> "Original"</h3>

Now that's awesome we just have to try every key and wish it will work.
After trying the key this one worked and we got this message :)</p>
Key: 0xFFFFFFFFFFFFFFFF

<img src="./second.png" >


```
from pwn import *
from des import DesKey<br>
<br>
conn = remote('167.71.93.117', 9000)<br>
<br>
conn.send('aaaaaaaa')<br>
x = conn.recvline();<br>
s = x[41:-1]<br>
keys = [<br>
	b'\x00\x00\x00\x00\x00\x00\x00\x00',<br>
	b'\x00\x00\x00\x00\xFF\xFF\xFF\xFF',<br>
	b'\xFF\xFF\xFF\xFF\x00\x00\x00\x00',<br>
	b'\xFF\xFF\xFF\xFF\xFF\xFF\xFF\xFF',<br>
]

for k in keys:<br>
	key = DesKey(k)<br>
	y = key.encrypt(s)<br>
	print(y)<br>
	print()
```
