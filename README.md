# ROP-Emporium
Written reports detaling the solution to each 64-bit challenge at ropemporium.com
We will be using [peda](https://github.com/longld/peda) to find our gadgets, and [pwntools](https://github.com/Gallopsled/pwntools) to write our exploits.
For future reference, running the `checksec`command in peda shows if the stack is executable.
In the case that NX is DISABLED, we will not need to use ROP.

![NX enabled](nx-enabled.png)

All binaries in this challenge come with NX enabled, and have the same vulnerability that allows us to smash the stack and overwrite the RIP.
The POC of every executable is the same, a simple stack smash and RIP overwrite with the same cyclic offset each time.
To craft out payload for each executable we simply send in 40 bytes of junk (ex. the letter A 40 times) and append any addresses and values we need to.
This was found using peda's pattern tool by sending in a generated pattern of length 50 as input (`pattern create 50`), and then using `pattern search` to find the offset to the RIP.

![smashed stack](smashed-stack.png)

![offset found - 40](offset-found.png)

\pagebreak

## 1. [ret2win](https://ropemporium.com/challenge/ret2win.html)
ret2win simply requires us to overwrite the return address with a pointer leading to the ret2win function, so we overwrite the RIP with the address of the function (found using objdump)

![flag](ret2win/flag.png)

## 2. [split](https://ropemporium.com/challenge/split.html)
