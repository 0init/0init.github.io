
		DESAmies is a Cryptography challeng
		writ-up by Abdallah EL-Shinbary 
		
		<img src="./1.png" >
<p>First of all we have a network connection that asks	for input and gives us what looks like encrypted message. </p>

<img src="./first.png" >

		<h3>It's just that nothing.</h3>
<p>Looking at the challenge name I assume that It's DES encryption and we have to decrypt the message somehow.</p>

		<p>Honestly I wasn't able to solve it without the hint which says "Strong key".</p>
		<p>Hmmmm if there is a strong key then there is a weak key.</p>

<p>After some googling I discovered that DES implementation has some 'weak keys' that if you encrypt with them twice you will get the original message<p>
		<h3>"Original" ---enc---> "Encrypted" ---enc---> "Original"</h3>

		<p>Now that's awesome we just have to try every key and wish it will work.</p>
		<p>After trying the key this one worked and we got this message :)</p>
		<p>Key: 0xFFFFFFFFFFFFFFFF</p>

<img src="./second.png" >








<p>from pwn import *<br>
from des import DesKey<br>
<br>
conn = remote('167.71.93.117', 9000)<br>
<br>
conn.send('aaaaaaaa')<br>
x = conn.recvline();<br>
s = x[41:-1]<br>
<br>
keys = [<br>
	b'\x00\x00\x00\x00\x00\x00\x00\x00',<br>
	b'\x00\x00\x00\x00\xFF\xFF\xFF\xFF',<br>
	b'\xFF\xFF\xFF\xFF\x00\x00\x00\x00',<br>
	b'\xFF\xFF\xFF\xFF\xFF\xFF\xFF\xFF',<br>
]<br>
<br>
for k in keys:<br>
	key = DesKey(k)<br>
	y = key.encrypt(s)<br>
	print(y)<br>
	print()</p>
