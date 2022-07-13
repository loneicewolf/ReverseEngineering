### note please read:
dont just copy paste. I have tried to make this as simple as possible;
by (even) actually including google-queries of errors you might encounter.
the reason for this is to show absolute beginners how (at least my own) my methodology of problem solving works
and looks like; when he/she gets a error. Example; a compiler barking at something.

#### Example "notation" I will make use of:
1 a issue/question/problem is encountered

2 Query Google: "(..)"
(this took us to below links)
- url1
- url2

3 apply found solutions(if any)
pros cons of different solutions?(don't only try 1 that works; try many; like 3)
(3 different that is)

make any change to the solution, and note it down, as wel las the reason behind that change/tweak.


### 📗 as I will surely get this question:
  Q: okay ; yes this is for beginners but; this kind of level of being obvious  isn't needed, is it?
  A: I would rather want it to be too obvious, than the opposite.




### basic ASM hello world



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


make 2 new files, `hello.asm` and 'asm_doit.sh'

choose a editor you like
- `gedit mousepad nano vi vim emacs (...)`  hello.asm doit.sh
- I use nano mostly: `nano hello.asm doit.sh`


### hello.asm contensts

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
ld: i386 architecture of input file `hello.o' is incompatible with i386:x86-64 output
```




























