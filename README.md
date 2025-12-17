*This project has been created as part of the 42 curriculum by **pdiniz-l**.*

# get_next_line

## Description

**get_next_line** is a C library that provides a function to read a file descriptor **line by line**, returning one line per function call.

The goal of this project is to implement a robust, memory-safe solution that handles buffered reading, preserves state between calls, and correctly manages edge cases such as end-of-file, missing newline characters, invalid file descriptors, and allocation failures.

This implementation follows the specifications of **Get Next Line v13** from the 42 curriculum and is written in compliance with the project constraints and allowed functions.

---

## Instructions

### Compilation

The project must be compiled while defining the `BUFFER_SIZE` macro, which controls how many bytes are read per call to `read`.

Example:

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line.c get_next_line_utils.c
```
You may change the value of `BUFFER_SIZE` to test different buffer behaviors.

### Library Description

This project implements the following public function:
```c
char *get_next_line(int fd);
```

### Behavior

Reads from the given file descriptor and returns one line per call

Returns the line including `\n` if a newline is present

Returns the last line without `\n` if EOF is reached without a trailing newline

Returns NULL on error or when there are no more lines to read

Preserves unread data between calls using an internal static variable

The caller is responsible for freeing the returned string

### File Structure

`get_next_line.h`
Header file containing prototypes and include guards

`get_next_line.c`
Core implementation of get_next_line

`get_next_line_utils.c`
Utility functions used internally by the main function

### Utility Functions

The following helper functions are reimplemented and used internally:

`ft_strlen(const char *str)` — returns string length

`ft_strdup(const char *s)` — duplicates a string

`ft_substr(char const *s, unsigned int start, size_t len)` — creates a substring

`ft_strjoin(char const *s1, char const *s2)` — concatenates two strings

`ft_strchr(const char *s, int c)` — searches for a character in a string

### Internal Logic

The implementation relies on internal static helpers to manage state and memory:

`memorize_line` — appends the read buffer to the accumulated string

`line_with_nl` — extracts a line up to \n and stores the remaining data

`make_line` — handles read return values and builds the final output

`safe_free_dp` — safely frees dynamically allocated pointers

### Error Handling and Guards

Returns `NULL` if:

`fd < 0`

`BUFFER_SIZE <= 0`

read fails

memory allocation fails

All allocated memory is properly freed on failure

No memory leaks are introduced

## Resources

42 School — Get Next Line v13 subject

POSIX documentation:

read(2)

Manual pages:

man read

man open

C standard library documentation
