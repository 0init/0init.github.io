---
layout: post
title:  " gettingstarted "
description: write-up by Mohamed Labib >> { Uzomaki }
tags: first blog
---

### getting-started is an easy reverse engineering challenge .
### as it is a good challenge to get into the reverse engineering world. 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/1.png)

### it doesn't require a huge knowledge by reverse. 


## we can solve it in many ways . 


# first way : 
## after downloading the file you'll get this 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/2.png)

let's try to run it . 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/3.png.png)

### we got this output " flag{ " and nothing else . 

lets try an easy way so we may get the flag . 

at first we need to know the what is the file we are treated with , so we can decide what to do in the next step  .

we can search for it a tool or a command to identify the file format .

in Linux there is a command called "file" that give much information about any file , i advice you to use it .

you can check this [site](https://www.geeksforgeeks.org/file-command-in-linux-with-examples/) for more information 

after using [file getting-started] we get this output . 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/4.png)

# the important part is the highlighted one, which telling us that the file is ELF32 .

but what is the the ELF ? 

ELF is an "Executable and Linkable Format" and this kind of file formats is usually used for unix/Linux programs because it's flexible. 

so here we can assume this file is compiled for Linux OS. 

The next step is dissemble the program  . 

as you know, any compiled program must move from HD to ram and being translated to machine code [0 , 1] so the machine
 execute it .

this machine code can be easily translated into assembly instructions using a disassembler

you can read more about dissembler [here](https://en.wikipedia.org/wiki/Disassembler)

after searching for disassembler you can find ( Ghidra , IDA , gdb , edb , ... , etc  ) 

I'd like to use adobe to know more about the basics . 

install it , and run it by [ edb --run (filename) ]

now you can see this triple seen , don't worry after understanding it, it'll be so easy 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/5.png)

as you can see there are 5 parts in the program . 
the first one is the code section which includes the instruction of the program in assembly.

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/6.png)

the secound is the registers section . 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/7.png)

mmm , what is regesters  ? 

the register is a very small and fast memory space in the CPU, some of the has a specific function . 

you can find more information about registers [here](https://whatis.techtarget.com/definition/register] 


then we have this small part under code part, which show all the values which is being used in each instruction, one by one
, to make code analysing more simple .

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/8.png)

the next part is the hex dump part 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/9.png)

it shows the program source and the values in memory in hex 

the last part is the stack part . 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/10.png)

the stack is a place where the values are being saved to be reused again in function calls.. 

it works in first in, last out technology. 

you can read more about stack [here](https://users.ece.cmu.edu/~koopman/stack_computers/sec1_2.html)

know you know what you looking to . 

lets start analyzing .

here we'll use only code and stack parts . 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/11.png)

in code part you'll see this . 

there are many instructions 

push > to add value to the stack 

mov > get the value for the left side and put it in the right one 

sub  > rigth side = sight side - left 

jmp > jump to the address in the left 

add > rigth side = sight side + left 

cmp > to compare the two values 

jle > to jump if the previse compare rigth < left part 

note : 

you'll see this in almost every program start . 

push ebp 

move ebp , esp 

this two instrctions is creating the stack 

Before moving into the instructions you can realise that there is many push hex values . 

the program is pushing a tall thing to the stack , it may be the flag . 

now press F8 to move in the program step by step . 

and look to the stack part after every push step . 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/12.png)

by the last push you can see the flag in stack part , from top to bottom . 

we can now get the all flag , but lets continue . 

at the output we only get the first five cars from the flag  . 

look to the part after the jmp , if you keep running it , it'll jump pack from jle four times , and in the fifth it leave this part . 

look close to :

mov dword [ebp-4] , 0 >> it's like             i = 0  

add dword [ebp-4] , 1 >>                       i++

cmp dword [ebp-4] , 4 

jle                   >> this 2 comands like  if( i <= 4)
 
so it become Obvious why it outputs only fine chars . 

to get the all flag in the output just change the 4 in cmp to 23 . 

![images](https://raw.githubusercontent.com/0init/0init.github.io/master/images/test/13.png)

