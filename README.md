# pwnable.tw-alivenote
+ The variable "name" is separated from the free() position in the .GOT region 108 byte (27x4byte). Addresses can be overridden to direct program to shellcode.
+ Shellcode will be placed in the heap, through "note" additions. However, there are 2 problems that are (1) shellcode must be lowercase characters and (2) shellcode must be chopped because it can only add up to 8 bytes at a time.
+ We have the instruction "jne 0x38" (corresponding to the opcode "u8", this instruction will jump to execute the next 0x3a instruction) added after 6 bits will solve the problem of having to split the shellcode. Now, when the shellcode is executed at the kth heap data area, it will jump to the k+3 heap data area. That is, when adding a shellcode piece to a chunk, we have to padding the three chunks to continue adding new shellcode.
# Shellcode Alphanumric
+ Create the string "//sh", we push 0x68734141, then pop eax, then xor 0x6e6e. Create the string "/bin", we push 0x6e696230, then pop ecx, then dec ecx. Push both in the correct order to the stack, then push esp, then pop eax. Use popad with correctly sorted stack to assign "/bin//sh" to ebx.
+ To make ecx point to the variable pointing to "/bin//sh", we just need to push ebx, push esp, pop ecx.
+ To eax = 0xb, we push 0x41 and then pop eax; xor al, x04a.
+ To get int 0x80, we assign at the end of the shellcode 02 bytes "\x37\x7a". Through calculating chunk, shellcode length we can calculate the address of 02 bytes "\x37\x7a". The shellcode header will process so that 02 bytes "\x37\x7a" become "\xcd\x80", by repeatedly dec edx, edx will decrease from 0 to 0xfffffffa, then xor [address \x37], dl ( equals \xcd) and xor [address edx], dl (in \x80).
