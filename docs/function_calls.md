# Assembly function calls
## Example:
```asm
global _start:
_start:
	call func

	mov rax, 60
	mov rdi, 0
	syscall
func:
	push rbp ; remember to store the stack pointer - it's the base for the caller (_start)
	mov rbp, rsp ; change stack base to current stack pointer

  	; do something
	sub rsp, 8 ; reserve 8 bytes on the stack
	mov qword [rsp] 1234

	mov rsp, rbp ; free space on the stack
	pop rbp ; pop caller's stack pointer off the stack
	ret

```
## Stack alignment
When calling a function the stack must be aligned to 16 bytes. This is for AVX and SSE instructions

## Prologue and Epilogue
When a function is called, you need to set up the stack frame. This is done by pushing the stack base pointer `rbp` to the stack, then setting it to `rsp` (current stack pointer). \
Before returning, `rsp` should be set back to what it was before the function, which happens to be `rbp`. `rbp` then needs to be set back to how the caller left it, so it is popped from the stack
### Prologue
This code should be at the start of most functions:
```asm
push rbp
mov rbp, rsp
```
### Epilogue
This code should be at the end of the function if the prologue is used
```asm
mov rsp, rbp
pop rbp
```


### Stack base and stack pointer
`rsp` (stack pointer) points to the top of the stack. Because the stack grows downward in memory, a push operation decrements rsp, and a pop operation increments rsp. 

`rbp`, (stack base pointer), is used used to define the base of a function's stack frame. It is set once at the beginning of a function call and remains constant throughout the function's execution. 

***

## `jmp` vs `call`
### The following 2 snippets are (mostly) equivalent:

```asm
call label

; code here

ret
```
```asm
push rip
jmp label

; code here

ret
```
The second code snippet is invalid and would not work, because `ret` would return to _start. This happens because `ret` expects a proper `call`. While it might be tempting to just jmp to a label as a function, it makes things a lot easier if you use call and obey the System-V ABI
while calling a function in this way would in theory work, it is not the same as call.

***

## Function arguments
Syscalls and functions use arguments in a different way. Each support a max of 6 arguments. The rest of the arguments go on the stack
### Syscalls
- `rax`: syscall number
- `rdi`: argument 1
- `rsi`: argument 2
- `rdx`: argument 3
- `r10`: argument 4
- `r8`: argument 5
- `r9`: arg 6
  


### Functions
- `rdi`: argument 1
- `rsi`: argument 2
- `rdx`: argument 3
- `rcx`: argument 4
- `r8`: argument 5
- `r9`: argument 6
