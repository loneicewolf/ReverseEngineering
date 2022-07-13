### note please read:
dont just copy paste. I have tried to make this as simple as possible;
by (even) actually including google-queries of errors you might encounter.
the reason for this is to show absolute beginners how (at least my own) my methodology of problem solving works
and looks like; when he/she gets a error. Example; a compiler barking at something.

#### Example "notation" I will make use of:
1 a issue/question/problem is encountered

📙
2 Query Google: "(..)"
(this took us to below links)
- url1
- url2

3 apply found solutions(if any)
pros cons of different solutions?(don't only try 1 that works; try many; like 3)
(3 different that is)

make any change to the solution, and note it down, as wel las the reason behind that change/tweak.


### 📗 as I will surely get this question:
- Q: okay ; yes this is for beginners but; this kind of level of being obvious  isn't needed, is it?
- A: I would rather want it to be just little-too obvious, than the opposite. 

So; you will find this repo (not only this file) to be painfully obvious at some places; and even a bit questionable; but I am doing it this way to view it from a **beginners perspective**; **who isn't quite sure about all with what** `Endian(big little), architecture, bits bytes, executable, object code, file extensions(and so on)`  - **means**

it is maybe obvious for me; but it wasn't when I was new to it. :)


### basic ASM hello world



📙
Query Google: "hello world asm linux"

- https://tldp.org/HOWTO/Assembly-HOWTO/hello.html
- https://tldp.org/HOWTO/Assembly-HOWTO/build.html

##### replicating the Hello World  (**not GAS**)


## A
```
pwd;uname -a;date
Linux myhostname 5.18.0-kali5-amd64 #1 SMP PREEMPT_DYNAMIC Debian 5.18.5-1kali5 (2022-07-04) x86_64 GNU/Linux
Wed Jul 13 11:21:39 AM CEST 2022
```


make 2 new files, `hello.asm` and `doit.sh`

choose a editor you like ( examples" `gedit mousepad nano vi vim emacs (...)`)
- I use nano mostly: `nano hello.asm doit.sh`

### hello.asm contents

```
section .text                   ;section declaration

                                ;we must export the entry point to the ELF linker or
    global  _start              ;loader. They conventionally recognize _start as their
			                          ;entry point. Use ld -e foo to override the default.

_start:

                                ;write our string to stdout

    mov     edx,len             ;third argument: message length
    mov     ecx,msg             ;second argument: pointer to message to write
    mov     ebx,1               ;first argument: file handle (stdout)
    mov     eax,4               ;system call number (sys_write)
    int     0x80                ;call kernel

                                ;and exit

  	mov     ebx,0               ;first syscall argument: exit code
    mov     eax,1               ;system call number (sys_exit)
    int     0x80                ;call kernel

section .data                   ;section declaration

msg db      "Hello, world!",0xa ;our dear string
len equ     $ - msg             ;length of our dear string
```

### doit.sh contents

```
# TODO: (for myself only)
## shB 
## CHECKS
## sample auto make
## dbg
## opts; explab:::

## redir
### e.g error to err file
### best/good practices
### check(sh)

echo "1) [+] object code    NASM" && nasm -f elf hello.asm
echo "2) [+] executable     LD"   && ld -s -o hello hello.o
```



## B
run the doit.sh file
`bash doit.sh`

```
bash doit.sh
1) [+] object code    NASM
2) [+] executable     LD
#	ld: i386 architecture of input file `hello.o' is incompatible with i386:x86-64 output
```


❗ error; it complains about something with i386 and architecture, being incompat. with some ; obscure output `i386:x86-64 output` ?

original error: `ld: i386 architecture of input file `hello.o' is incompatible with i386:x86-64 output`
shortened down error: `ld: i386 arch input file incompatible i386:x86-64 output`

📙
Query Google "ld: i386 arch input file incompatible i386:x86-64 output"
- first things to ask ourselves; is some questions;
- that is; what OS is we on and what OS is the one asking and the one answering on e.g this result
- https://stackoverflow.com/q/19200333/14346786
- (and more questions such as , do we use the same version? same tool? same  args?)
- Is something different in what we have and they have? if so; what and can we change ours to match ?
(....)


the error is slighly different but still is Kinda like ours; doesn't hurt to try ?
- trying this answer (which isn't by the tiem writing this; accepted)
- https://stackoverflow.com/a/20881370/14346786

```
	Use 64 bits instead of 32 for your loader and compile it with the following command:
	nasm -f elf64 loader.asm -o loader.o
	This should solve your error
```

trying it; but tweaking the commanda bit, to match our `hello` context
(note: I use the word `context` just arbitrarily here; just to note our hello-file, which might be our hello executable; hello object file, and so on) (to avoid confusion)

first we try it manually;
```
## likely not needed but
## why not
rm hello
rm hello.o


## https://stackoverflow.com/a/20881370/14346786
nasm -f elf64 hello.asm
ld -s -o hello hello.o 
./hello
#	Hello, world!
```

it works! noting that down and making a permanent change to our `doit.sh` file

#### new contents of file `doit.sh`

```
## v1
	## echo "1) [+] object code NASM" && nasm -f elf hello.asm
	## echo "2) [+] executable LD" && ld -s -o hello hello.o
## v2
## fix: #	2) 	ld: i386 architecture of input file `hello.o' is incompatible with i386:x86-64 output
## https://stackoverflow.com/a/20881370/14346786

nasm -f elf64 hello.asm
ld -s -o hello hello.o
./hello
```

test it..

```
bash doit.sh
#	Hello, world!
```

- works! write back some echo's if you want; I will skip it;


### backup the project
```
mkdir done
cp * done
```

### that was   hello world in ASM  



sources:
- google
- stackoverflow
- tldp



TODO (for myself)
:warning: `summary here later`
