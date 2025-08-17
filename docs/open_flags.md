# [open](https://linux.die.net/man/2/open) syscall flags

## Description
The following table lists the file control flags used with the **[open(2)](https://linux.die.net/man/2/open)** system call, along with their octal and hexadecimal values.
These flags are used to define the access mode and behavior of a file descriptor.

## List of flags

| Flag Name      | Octal Value   | Hexadecimal Value |
|----------------|---------------|-------------------|
| O_RDONLY       | 0o00          | 0x00              |
| O_WRONLY       | 0o01          | 0x01              |
| O_RDWR         | 0o02          | 0x02              |
| O_CREAT        | 0o100         | 0x40              |
| O_EXCL         | 0o200         | 0x80              |
| O_NOCTTY       | 0o400         | 0x100             |
| O_TRUNC        | 0o1000        | 0x200             |
| O_APPEND       | 0o2000        | 0x400             |
| O_NONBLOCK     | 0o4000        | 0x800             |
| O_NDELAY       | 0o4000        | 0x800             |
| O_ASYNC        | 0o20000       | 0x2000            |
| O_SYNC         | 0o4010000     | 0x401000          |
| O_FSYNC        | 0o4010000     | 0x401000          |

