---
title: "C Programing Language Notes."
categories:
  - C 
tags:
  - notes
---

List of useful content about C Language.

# Compilation process

+ Pre-processer:
    +  Remove comments.
    + Resolve the #include #define

+ Compilation 
    + It takes the output of the preprocessor, and the source code, and generates assembler source code.

+ Assembly
    + It takes the assembly source code and produces an assembly listing with offsets. The assembler output is stored in an object file.

+ Linking 
    +  It takes one or more object files or libraries as input and combines them to produce a single (usually executable) file

# Memory layout

# Header file

1. include "file.h" vs <file.h>

2. Missing "include guards"

    + Using a include guard , you can prevent a header file being included multiple times during the compilation process.

    + Prevent danger circular references between header files which can cause weird compilation failures. (multiple define).

    + Cause significant build delays in large systems.

# Static variable in c

1. Local static variables.

    + Scope: Block code that initialize static variable.
    + Define one time, remain in memory while the program is running.
    + The static variables are stored in the data segment of the memory.

    ```c
        static int a = 10; // storage in initialized data segment
        static int b; // storage in uninitialized data segment (BSS)
    ```

2. Gobal static variables.

    + Define one time on entry file.
    + Scope: only in the file that delarated global static variable.
    + extern staic --> that crazy

# Extern

> file1:

```c
    int extern_var; // Maybe initialize or 0 is default.
```

> file2: 

```c
    extern extern_var; // said that it defined some where
    // If not extern cause linker error: multiple `extern_var` defined
```

# Memory leak, corrupt
+ Double free:
    > Leads to undefined behavior, double-freeing a block of memory will corrupt the state of the memory manager, which might cause existing blocks of memory to get corrupted or for future allocations to fail.
+ Tools: Valgrind

# Calculate factorial number in single function
```C
#include<stdio.h>
#define N 10

int main(int num)
{
    static int cnt;
    static int ret = 1;

    cnt = num;

	if (cnt <= N) {
        ret = ret * num;
        main(cnt + 1);
	} else {
	    printf("%d!=%d \n", N, ret);
	    return 0;
	}
}

```
