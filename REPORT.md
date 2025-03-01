Name: Darren Ang\
Email: dang004@ucr.edu\
Assignment name: Lab05-PipelinesAndHazards\
Lab section: 21\
TA: Nurlan Nazaraliyev

## Section 1

| PC | Opcode | src1_addr | src1_out   | src2_addr | src_out    |dst_addr | dst_data   |
|---:|-------:|----------:|-----------:|----------:|-----------:|--------:|-----------:|
|   0| 0x23   | 0         | 0x00000000 | 2         | 0x00000000 | 0       | 0x00000000 |
|   4| 0x23   | 0         | 0x00000000 | 2         | 0x00000000 | 0       | 0x00000000 |
|   8| 0x00   | 2         | 0x00000000 | 2         | 0x00000000 | 0       | 0xXXXXXXXX |
|  12| 0x2B   | 0         | 0x00000000 | 3         | 0x00000000 | 2       | 0x00000056 |
|  16| 0x00   | 3         | 0x00000000 | 2         | 0x00000056 | 2       | 0x00000056 |
|  20| 0x08   | 3         | 0x00000000 | 5         | 0x00000000 | 3       | 0x00000000 |
|  24| 0x00   | 5         | 0x00000000 | 3         | 0x00000000 | 3       | 0x00000084 |
|  28| 0x00   | 6         | 0x00000000 | 2         | 0x00000056 | 4       | 0xFFFFFFAA |
|  32| 0x00   | 6         | 0x00000000 | 2         | 0x00000056 | 5       | 0x0000000C |
|  36| 0x00   | 5         | 0x0000000C | 4         | 0xFFFFFFAA | 6       | 0x00000000 |
|  40| 0x04   | 5         | 0x0000000C | 0         | 0x00000000 | 7       | 0x00000056 |
|  44| 0x23   | 0         | 0x00000000 | 8         | 0x00000000 | 8       | 0xFFFFFFA9 |
|  48| 0x00   | 0         | 0x00000000 | 0         | 0x00000000 | 6       | 0x00000000 |
|  52| 0x00   | 0         | 0x00000000 | 0         | 0x00000000 | 31      | 0x0000000C |
|  56| 0x00   | 0         | 0x00000000 | 0         | 0x00000000 | 8       | 0x00000000 |

## Section 2

```asm
lw $v0 31($zero)
add $v1 $v0 $v0
sw $v1 132($zero)
sub $a0 $v1 $v0
addi $a1 $v1 12
and $a2 $a1 $v1
or $a3 $a2 $v0
nor $t0 $a2 $v0
slt $a2 $a1 $a0
beq $a2 $zero -8
lw $t0 132($zero)
```

Data Hazards:
- Instruction 2 `add $v1 $v0 $v0` depends on Instruction 1 `lw $v0 31($zero)` as $v0 is being loaded from memory but the next instruction needs to use $v0 immediately.\
- Instruction 4 `sub $a0 $v1 $v0` depends on Instruction 2 `add $v1 $v0 $v0` as $v1 is computed in the add instruction but is also used in sub.

Control Hazards:
- Instruction 10 `beq $a2 $zero -8` is a control hazard as the processor does not know if the branch will be taken or not until $a2 is computed.

## Section 3

Example Instruction: `add $v2 $v2 $zero`\
This instruction would help to avoid both a stall and a hazard as it gives the processor an extra cycle to load the value into $v0 before add needs $v0.