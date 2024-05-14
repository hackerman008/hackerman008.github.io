---
layout: post
title: "Offensive Development in Assembly and C: Understanding and Using Common Assembly Instructions"
date: 2024-5-14 15:36:00 +0530
category: offensive
---

<!--Date started: 2024-5-12 09:55:00-->

![cover](/files/images/Offensive_Development_In_Assembly_And_C_Intro/Cover1_Offensive_Development.jpeg)

# **Contents**
- Things to remember
- Using WindDbg to debug and test the code
- mov, movzx, movsx instruction
- add instruction
- sub instruction
- mul instruction
- div instruction
- not instruction
- or instruction
- and instruction
- xor instruction
- push and pop instruction
- ror and rol instruction
- shr and shl instruction
- call instruction
- RIP relative addressing and absolute addressing
- After Thoughts
- Download

# **Things to remember**

This article will go through the most common instruction we require and how to use them. For a complete description of the instruction refer to the manual. The code is provided in the **Download** section at the end of the article and I encourage you to download the code and read it, debug it and check how every instruction works yourself. 

# **Using WinDbg to debug and test the code**

If you installed the windows SDK or already had it installed, then you will have **x64 Native Tools Command Prompt** and **WinDbg** installed. I will use WinDbg for debugging as it is good for source code debugging. If you are not comfortable with WinDbg and its commands you can also use x64dbg if you want. If you use x64dbg you do not need the .pdb file and therefore you can assemble the code without it in the Compile.bat file. 

Follow the following steps to start debugging.
- First open **x64 Native Tools Command Prompt** from the start menu or where it is installed. 
- Navigate to the directory in the **x64 Native Tools Command Prompt** where you downloaded the source code and run the **Compile.bat** file. This will generate the final executable **Output.exe**.
- Open WinDbg and goto **File -> Open source file** option. Then from the bottom right drop down menu select **All files**. Now select the main.asm file.
- Goto **File -> Launch Executable** and select **Output.exe**. 

The above steps should load the executable and the source file, the symbols will be automatically loaded since we have the .pdb file in the same folder. Now you can put breakpoint or debug line by line. Below is an image from my system both for source level (assembly with comments in our case) and assembly level.

![image_WingDbg_SourceLevelDebugging](/files/images/Offensive_Development_In_Assembly_And_C_Intro/blog2/SourceLevelDebugging.png)

![image_WingDbg_AssemblyLevelDebugging](/files/images/Offensive_Development_In_Assembly_And_C_Intro/blog2/AssemblyLevelDebugging.png)

# **mov, movzx, movsx instruction**

**The 'mov' instruction copies the source operand (second operand) to the destination operand (first operand)**. The source operand can be an immediate value, a general purpose register, segment register, memory location. The destination register can be a general purpose register, segment register or memory location. Both operand must be of the same size **i.e you can't do mov dword ptr[rax], cl**. **Both operand also cannot be memory locations**.

```
mov rax, 0A1A2A3A4A5A6A7A8h           ; mov immediate hex value A1A2A3A4A5A6A7A8 to rax
xor rax, rax                          ; clears rax register
mov eax, dword ptr[tempVar]           ; moves the value of tempVar variable to eax
xor eax, eax
mov cx, 0A1A2h                        ; mov immediate hex value A1A2 to cx
mov ax, cx
xor ax, ax
mov al, 0A1h                          ; mov immediate hex value A1 to al
xor al, al
```

>Note: When you do **mov eax, dword ptr[tempVar]** the higher 32 bits of the rax will zero out, because in x64 architecture writing to a 32 bit register zero extends into the 64 bit register.

**The 'movzx' instruction copies the contents of the source operand(register or memory) to the destination operand(register)** and zero extends the value. The size of the source operand has to be byte or word.

```
mov al, 8
movzx rax, al                         ; copies the value in al to rax and zero extends it
```

**The 'movsx' instruction copies the contents of the source operand(register or memory) to the destination operand(register)** and sign (MSB of source operand) extends it. 

```
mov al, -8
movsx eax, al                         ; copies the value in al to rax and sign extends it
```

#  **add instruction**

**Adds the destination operand (first operand) and the source operand (second operand) and stores the final result in the destination operand**. The destination operand can be register or memory location. The source operand can be an immediate value, register or memory location. Both operands cannot be memory operands.

```
mov rcx, 20
add rcx, 10                         ; add 10 to the value stored in the rcx register
```

# **sub instruction**

**Subtracts the source operand (second operand) from the destination operand (first operand)** and stores the result in the destination operand. The destination operand can be register or memory location. The source operand can be an immediate value, register or memory location. Both operands cannot be memory operands.

```
mov rcx, 20
sub rcx, 10                         ; subtract 10 from the value stored in rcx
```

# **mul instruction**

**Performs an unsigned multiplication of the first operand (destination operand/source operand) and the second operand (source operand) and stores the final result in the destination operand**. The below table shows the destination operand depending on the source operands. The r/m notation signifies a register or memory.

| Operand size  | Destination   | Source1   | Source2   | 
| byte          | AX            | AL        | r/m8      |
| word          | DX:AX         | AX        | r/m16     |
| dword         | EDX:EAX       | EAX       | r/m32     |
| qword         | RDX:RAX       | RAX       | r/m64     |

**The high bits of the multiplication are stored in DX, EDX, RDX and lower bits of the multiplication are stored in AX, EAX, RAX, depending on the Source1 operand**. The Source2 can be a register or a memory location.

```
mov rax, 6                              
mov rcx, 10
mul rcx                             ; rax is Source1, rcx is Source2, rdx:rax is destination operand
```

# **div instruction**

**'div' perform unsigned division. It divides the value in AX, DX:AX, EDX:EAX or RDX:RAX registers by the source operand and stores the result in AX (AH:AL), DX:AX, EDX:EAX or RDX:RAX registers**. The source operand can be a general purpose register or a memory location. The table below shows the action of the div instruction depending on the size of the source operand (Divisor). The r/m notation signifies a register or memory.

| Opernad Size          | Dividend      | Divisor       | Quotient      | Remainder     |  
| word/byte             | AX            | r/m8          | AL            | AH            |
| dword/word            | DX:AX         | r/m16         | AX            | DX            |
| qword/dword           | EDX:EAX       | r/m32         | EAX           | EDX           |
| dword/qword           | RDX:RAX       | r/m64         | RAX           | RDX           |

In the example below the value in rax is divided by the value in rcx and the quotient is stored in rax and the remainder is stored in rdx.

```
xor rdx, rdx
mov rax, 35
mov rcx, 5
div rcx                             ; rcx is divisor, rax is dividend, rax = Quotient, rdx = remainder
```

# **not instruction**

**The 'not' instruction performs a one's compliment operation (every 1 is set to 0 and every 0 is set to 1) on the destination operand (first operand) and stores the result in the destination operand**.
The operand can be a register or a memory location. Below table shows the resulting bit based on the destination operand bit.

| A     | NOT A   |
| 1     | 0       |
| 0     | 1       |

```
mov eax, 8
not eax                             ; performs one's compliment (all bits are flipped) of value in eax 
```

# **or instruction**

**The 'or' instruction performs a bitwise inclusive or operation of the destination (first operand) and the source (second operand)**. The source operand can be an immediate value, a register  or a memory location. The destination operand can be a register or a memory location. In **or** operation each bit of the result of the **or** instruction is set to 0 if bit of source and destination operand is 0, else the resulting bit is set to 1 if either source or destination operand bit is 1. Below table shows the resulting bit based on the source and destination operand bit.

| A     | B     | A OR B    |
| 0     | 0     | 0         |
| 0     | 1     | 1         |
| 1     | 0     | 1         |
| 1     | 1     | 1         |

```
mov rax, 7
mov rcx, 8
or rax, rcx                         ; performs bitwise inclusive or of rax (destination) and rcx (source) and stores result in rax (destination)
```

# **and instruction**

**The 'and' operation performs bitwise and of the destination (first operand) and the source (second operand) and stores the result in the destination operand**. The source operand can be an immediate value, a register  or a memory location. The destination operand can be a register or a memory location. In **and** operation the resulting bit is 1 if and only if both the source and destination operand bit is 1. Below table shows the relation and the resulting bit.

| A     | B     | A AND B   |
| 0     | 0     | 0         |
| 0     | 1     | 0         |
| 1     | 0     | 0         |
| 1     | 1     | 1         |

```
mov rax, 5
mov rcx, 7
and rax, rcx                        ; performs bitwise and of rax (destination)  and rcx (source) and stores  the result in rax (destination) 
```

# **xor instruction**

**The 'xor' instruction performs bitwise exclusive or between the destination (first operand) and the source (second operand)**. The source operand can be an immediate value, a register  or a memory location. The destination operand can be a register or a memory location. In **xor** operation the resulting bit is 1 if both the destination and source operand bits are different. Below table shows the relation and the resulting bit.

| A     | B     | A XOR B   |
| 0     | 0     | 0         |
| 0     | 1     | 1         |
| 1     | 0     | 1         |
| 1     | 1     | 0         |

```
mov rax, 7
mov rcx, 8
xor rax, rcx                        ; performs bitwise exclusive or of rax (destination) and rcx (source) and stores the result in rax (destination)
```

One thing to note is, we can use instructions like **xor rax, rax** to zero out the rax register or any other register, this happens because the bits in both destination and source are same and the result of the **or** operation would be 0.

# **push and pop instruction**

The push instruction can act differently based on 32 bit mode or 64 bit mode. We care about 64 bit mode. So in 64 bit, **the push instruction will only push 64 bit or 16 bit values on to the stack**. To avoid misaligning the stack it is better to push only 64 bit values to the stack. Whenever the push instruction is executed the value in RSP register is reduced by the size of operand pushed to stack. The table below shows valid and invalid usage of push instruction.

| Instruction             |                                                   |
| push rax                | valid                                             |
| push eax                | invalid                                           |
| push 8                  | valid (value sign extended to 64 bit)             |
| push qword ptr[address] | valid                                             |
| push dword ptr[address] | invalid                                           |
| push word ptr[address]  | valid (value sign extended to 64 bits)            |                


The 'pop' instruction basically does the opposite of the push instruction i.e it pops the value at top of the stack and stores it in the destination operand.
**If you push 64 bit value, you should pop a 64 bit value. If you push a 16 bit value, you should pop a 16 bit value**.

```
mov rax, 8
push rax                            ; push value in rax to the stack - only push 64 bit values to stack to maintain stack alignment
pop rcx                             ; pop 64 bit value at top of stack and store it in rcx

```

# **ror and rol instruction**

**The ror (rotate right) instruction shifts bits of the destination operand to the right, the least significant bit is shifted to the most significant bit position, this bit is also copied to the CF flag**. **The rol  (rotate left) shifts bits of the destination operand to the left, the most significant bit is shifted to the least significant bit position, this bit is also copied to the CF flag**. The destination operand can be a register or a memory location. The source operand (the count operand) can be an immediate or a value in the cl register. 

```
; ror -> rotate right
mov al, 1 
ror al, 1                           ; rotate the value in al register by 1 bit to the right 

; rol -> rotate left
mov al, 8
rol al, 1                           ; rotate the value in al register by 1 bit to the left
```


# **shr and shl instruction**

**The shr (shift logical right) instruction shifts the bits of the destination operand to the right, the least significant bit is shifted to the CF flag and the most significant bit position is filled with 0**. **The shl (shift logical left) instruction shifts the bits of the destination operand to the left, the most significant bit is shifted over to the CF flag and the least significant bit position is filled with 0**. The destination operand can a register or a memory location. The count operand can be an immediate value or a value in the cl register. 

```
; shl -> shift logical left
mov al, 8
shl al, 2                           ; shift all bits to the left by 1, move the MSB to the carry flag and fill the LSB with 0

; shr -> shift logical right
mov al, 8
shr al, 1                           ; shift all bits to the right by 1, move the LSB to the carry falg and fill the MSB with 0
```

# **call instruction**

The **call** instruction is used to call a subroutine i.e a function. The call instruction is a bit complicated if we consider privilege levels, segments etc. But we only care about 64 bit user mode calls within our code and process space. So from that perspective we only care about **near call**. The **near call** basically refers to calling any function within our process space either directly or indirectly. In direct call the address/offset is specified directly such as **call \<FunctionName\>**. In indirect call the address is specified in a register or a memory location like **call rax** or **call qword ptr[rax]**. **whenever the call instruction is executed the address of the next instruction is pushed to the stack and the RIP is changed to the address of the subroutine**. 

>Note: Check the code to see how the call instruction works

# **RIP relative addressing and absolute addressing**

The concept of relative addressing is important because it helps us write position independent code. **In relative addressing, the exact address of the variables location is not encoded in the machine instruction. This means there is no relocation required and we can execute our code as shellcode from any address. The reference to the location of the variable is done based on an offset from the next instruction**.

In the code below both instructions uses relative addressing.
```
.data
string1        byte  "Hello World!", 0
tempVar        dword  5  

.code
mov eax, dword ptr[tempVar]         ; moves the value of tempVar to eax
lea rcx, [string1]                  ; moves the address of string1 variable to rcx 
```

**In absolute addressing the explicit address of the variable location is encoded in the machine instruction** and therefore the code needs to be located at a specified base address otherwise the instruction would need to be fixed and this is why we have a relocation section. **Shellcode is independent and does not require relocation since all memory references are relative**.

# **After Thoughts**

It is not necessary to remember and mug up all the instructions and what they do from the start. From my experience its better to have an idea of what an instruction can do and as you use them in your code and debug it, it will make much more sense and also be easy to remember. x64 architecture has a lot of instructions most of them you'll never learn and therefore the most common once should always be kept in mind.

# **Download**

- [Code](https://github.com/hackerman008/Offensive_Development_in_Assembly_and_C)
