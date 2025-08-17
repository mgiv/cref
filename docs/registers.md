# x86-64 Registers

## General-Purpose Registers

The x86-64 architecture provides **16 general-purpose registers**, each 64 bits wide. Many of these registers have specific roles in function calls and arithmetic operations.

| Register | Size    | Purpose |
|----------|---------|---------|
| **RAX**  | 64-bit  | Accumulator. Used for arithmetic and logical operations, and to store the function's return value. |
| **RBX**  | 64-bit  | Base register. Used as a base pointer for memory access. It is non-volatile. |
| **RCX**  | 64-bit  | Counter. Used for loop counters and shifts. Also used to pass the fourth integer argument. |
| **RDX**  | 64-bit  | Data register. Used for I/O port access, multiplication, division, and passing the third integer argument. |
| **RSI**  | 64-bit  | Source Index. Pointer to the source in data transfer operations. Also used to pass the second integer argument. |
| **RDI**  | 64-bit  | Destination Index. Pointer to the destination in data transfer operations. Also used to pass the first integer argument. |
| **RBP**  | 64-bit  | Base Pointer. Points to the current stack frame. It is non-volatile. |
| **RSP**  | 64-bit  | Stack Pointer. Points to the top of the stack. Automatically managed by `push` and `pop`. |
| **R8–R15** | 64-bit | General-purpose registers. Used to pass additional function arguments. |

---

## Special-Purpose Registers

### Instruction Pointer (RIP)

- Holds the memory address of the next instruction to be executed.
- Cannot be directly modified with `mov`, but is updated by:
  - **Jumps** (`jmp`)
  - **Calls** (`call`) - pushes current RIP before loading new address
  - **Returns** (`ret`) — pops return address back into RIP

---

### EFLAGS Register

The **EFLAGS** register is a collection of single-bit flags that track CPU state and arithmetic results.

Key flags:

- **ZF (Zero Flag):** Set if result is zero.
- **CF (Carry Flag):** Set on arithmetic overflow/underflow, used for multi-precision arithmetic.
- **SF (Sign Flag):** Set if result is negative (MSB = 1).
- **OF (Overflow Flag):** Set if signed result does not fit in destination.
- **DF (Direction Flag):** Controls string operation direction (`std` = decrement, `cld` = increment).

These flags guide **conditional jumps** (e.g., `je` uses ZF after `cmp`).

---

## Register Usage in Function Calls

The **System V AMD64 ABI** defines how registers are used in function calls.

### Argument Passing

First six integer/pointer arguments are passed in registers:

1. **RDI**
2. **RSI**
3. **RDX**
4. **RCX**
5. **R8**
6. **R9**

Additional arguments go on the stack.

### Return Value

- Stored in **RAX**.
- If >64 bits, may be returned via memory referenced by RAX.

---

## Volatile and Non-Volatile Registers

### Volatile (Caller-Saved)
Not preserved across calls. Caller must save them if needed later.
**Registers:** RAX, RCX, RDX, RSI, RDI, R8, R9, R10, R11

### Non-Volatile (Callee-Saved)
Preserved across calls. Callee must save/restore if modified.
**Registers:** RBX, RBP, RSP, R12, R13, R14, R15

---

## Stack Frame

The stack stores local variables, spilled registers, and extra arguments.

- **RSP** → always points to the top of the stack.
- **RBP** → often used as a frame pointer (fixed reference point inside stack frame).
  - Modern optimisations sometimes repurpose RBP as a general-purpose register.

