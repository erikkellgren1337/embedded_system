# AVR Registers

## Table of Contents

- [AVR Registers](#avr-registers)
  - [Table of Contents](#table-of-contents)
  - [General purpose registers](#general-purpose-registers)
  - [SREQ (Status Register)](#sreq-status-register)
    - [I: Global Interrupt Enable](#i-global-interrupt-enable)
    - [T: Copy Storage](#t-copy-storage)
    - [H: Half Carry Flag](#h-half-carry-flag)
    - [S: Sign Flag, S = N xor V](#s-sign-flag-s--n-xor-v)
    - [V: Tow's Compliment Overflow Flag](#v-tows-compliment-overflow-flag)
    - [N: Negative Flag](#n-negative-flag)
    - [Z: Zero Flag](#z-zero-flag)
    - [C: Carry Flag](#c-carry-flag)
  - [I/O Register](#io-register)
    - [DDR (Data Direction Register)](#ddr-data-direction-register)
    - [PORT](#port)
    - [DDR and PORT](#ddr-and-port)
    - [PIN](#pin)

## General purpose registers 

Registers are special storages with 8-bit capacity and they look like this

```
[ 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 ]
```

A register can store numbers from 0 to 255.

In AVR there are 32 registers, named `R0` to `R31`.

**Note**: Only register 16 to 31 (inclusive) can be use with `LDI` instruction. For any register below that you need to first load the value into any R16 to R31 and use `MOV` to copy the value to any R0 to R15.

## SREQ (Status Register)

The Status register contains information about the result of the most recently executed arithmetic instruction. The Status register is updated after all the Arithmetic Logical Unit (ALU) operations. ([Source: Microchip Developer Help](https://microchipdeveloper.com/8avr:status))

```
      [ I | T | H | S | V | N | Z | C ]
bit:    7   6   5   4   3   2   1   0
```

### I: Global Interrupt Enable

The Global Interrupt Enable bit must be set for the interrupts to be enabled.

### T: Copy Storage

TODO

### H: Half Carry Flag

TODO

### S: Sign Flag, S = N xor V

TODO

### V: Tow's Compliment Overflow Flag

TODO

### N: Negative Flag

This indicates a negative result in an arithmetic or logic operation.

### Z: Zero Flag

This indicates a zero result in an arithmetic or logic operation.

### C: Carry Flag

This indicates a carry in an arithmetic or logic operation.

## I/O Register

The I/O Registers are **DDR**(Data Direction Register), **PIN** and **PORT**.

### DDR (Data Direction Register)

This register is used for setting the data direction of the PORT group.

| DDRx  |           Mode           |
| :---: | :----------------------: |
|   0   | High impedance / Pull-Up |
|   1   |        OUT 0 / 1         |

Example:

```asm
LDI R16, 0xFF
OUT DDRC, R16
```

Load ones into `DDRC` register.

### PORT

This is the PORT register, it is used for writing values to PORT group.

can be use

Example:

```asm
LDI R16, 0xFF     ; Set ones into R16
OUT PORTC, R16    ; Set R16 into PORTC register
```

This writes value `0xFF` into the PORT group C.

### DDR and PORT

DDR and PORT are used together to achieve different mode of the GPIO (General Purpose In Out) pins. The configuration can be seen below.

| DDRx  | PORTx |      Mode      |
| :---: | :---: | :------------: |
|   0   |   0   | high impedance |
|   0   |   1   |    Pull-up     |
|   1   |   0   |     Out 0      |
|   1   |   1   |     Out 1      |

where `x` is the port group, such as A, B, C, etc...

### PIN

This is the IN register, it stores the value of the PORT group. It's used with `IN` instruction.

Example:

```asm
IN R16, PINC
```

Copy the value in `IN` register on PORT group C.
