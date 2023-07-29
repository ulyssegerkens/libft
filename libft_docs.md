# Libft documentation


## Table of Contents

1. [Functions](#functions)
   - [memchr](#memchr)
     - [Why `ptr` is `const`?](#why-ptr-is-const)
   - [memcpy](#memcpy)
     - [Why cast to character pointers `(char *)`, doesn't it prevent copying of other types of data? Like numeric values?](#why-cast-to-character-pointers-char-doesnt-it-prevent-copying-of-other-types-of-data-like-numeric-values)
     - [Why unsigned char and not unsigned int?](#why-unsigned-char-and-not-unsigned-int)
   - [memmove](#memmove)
     - [Whats the main difference between memcpy and memmove?](#whats-the-main-difference-between-memcpy-and-memmove)
     - [`if (s < d && d < s + len)`](#if-s--d--d--s--len)
   - [strlcpy](#strlcpy)
     - [`if (dstsize > 0)`](#if-dstsize--0)
   - [strncmp](#strncmp)
     - [cast to `(unsigned char)`](#cast-to-unsigned-char)
   - [strnstr](#strnstr)
   - [strdup](#strdup)
     - [`ft_strlen + 1`](#ft_strlen--1)
   - [atoi](#atoi)
     - [`sign = 44 - *str++;`](#sign--44---str)
     - [atoi should handle overflow?](#atoi-should-handle-overflow)

2. [General standards](#general-standards)
   - [Why to check the `malloc`?](#why-to-check-the-malloc)
   - [Why do not protect functions?](#why-do-not-protect-functions)
   - [Can I pass an int variable in `malloc` without casting it to `size_t`?](#can-i-pass-an-int-variable-in-malloc-without-casting-it-to-size_t)

3. [Header & Makefile](#header--makefile)
   - [libft.h includes purpose](#libft-h-includes-purpose)
   - [Makefile](#makefile)
     - [General purpose](#general-purpose)
     - [target rule](#target-rule)
     - [Why cc and not gcc?](#why-cc-and-not-gcc)
     - [`-Wall -Wextra -Werror -I`: What's the -I flag?](#-wall--wextra--werror--i--whats-the--i-flag)
     - [`ar rcs` command](#ar-rcs-command)
     - [`.PHONY`](#phony)
     - [Why Makefile works without this rule?](#why-makefile-works-without-this-rule)

---
</br>

## Functions

- memchr
    
    - Why `ptr` is `const`?
    
		This means that the pointer points to a constant unsigned char, i.e., the value it points to cannot be modified through this pointer. However, the pointer itself is not constant and can be reassigned to point to a different memory address, as is being done in the line **`ptr = (unsigned char *)s;`**.
		
		The reason for making **`ptr`** const in this function is that the function takes a const void pointer as its first argument, **`const void *s`**. This indicates that the function does not intend to modify the data pointed to by **`s`**. By making **`ptr`** a const pointer, the function guarantees that it won't modify the data being pointed to while searching for the character **`c`**. If **`ptr`** was not defined as a const pointer, the compiler would raise a warning when assigning it to **`s`**, as this would discard the const qualifier.
    
- memcpy
    - Why cast to character pointers `(char *)`, doesn't it prevent copying of other types of data? Like numeric values?
        
        It is a common practice because it allows to consider the memory blocks to be copied as raw data blocks, that is, arrays of bytes, independently of the type of data they contain. This means that the `memcpy` function can be used to copy memory blocks containing data of any type, whether they are integer data types, floats, or pointers.
        Casting to `char *` does not limit the ability of the `memcpy` function to copy other types of data. In fact, the conversion between data types is done automatically during memory copying. However, it is important to ensure that the size of the data to be copied is correctly specified, otherwise it could lead to unpredictable results or runtime errors.
        
    - Why unsigned char and not unsigned int ?
        
        Unsigned char is used in the implementation of **`memcpy`** because **the size of the memory being copied is often measured in bytes**, which are the units of storage that can be addressed by a single byte. **An unsigned char has a size of one byte** and can store values between 0 and 255, which is sufficient to represent all possible byte values.
        
        On the other hand, using unsigned int for copying bytes would be overkill, as it would consume more memory than necessary, and could lead to undefined behavior if the source memory region contains values that are outside the range of an unsigned int.
        
- memmove
    - Whats the main difference between memcpy and memmove?
        
        memmove handle overlaping
        
    - `if (s < d && d < s + len)`
        
        Checks if there is an overlap between the source and destination memory regions. The condition `s < d && d < s + len` ensures that the destination starts somewhere in the middle of the source region:
        
        This function first checks whether the source and destination addresses overlap or not by comparing their memory addresses. If the source address is less than the destination address, and the destination address is within the range of the source memory block, then the function copies the data in reverse order from the end of the memory block to the start. This is done to avoid overwriting data before it is copied.
        
- strlcpy
    - `if (dstsize > 0)`
        
        In summary, the `if (dstsize > 0)` check is important in the implementation of `ft_strlcpy()` to prevent buffer overflows and ensure that the destination buffer is only modified if there is enough space to store at least the null-terminating character.
        
- strncmp
    - cast to `(unsigned char)`
        
        Casting the characters to unsigned char is important in the **`ft_strncmp()`** function to ensure that the result of the subtraction is always positive, since it can be represented as an unsigned value.
        
        The values of characters in the ASCII table are signed, meaning that they can have negative values. When comparing characters using subtraction, the result can be negative if the first character has a smaller ASCII value than the second character. If the result is negative, it will be interpreted as a large positive value because it is stored as an unsigned value.
        
        For example, let's assume we are comparing the strings "abc" and "abd" using **`ft_strncmp()`**. If we cast the characters to **`unsigned char`**, we will have:
        
        - s1[0] = 'a' = 97 (in ASCII)
        - s2[0] = 'a' = 97 (in ASCII)
        - ((unsigned char)s1[0] - (unsigned char)s2[0]) = 0
        - s1[1] = 'b' = 98 (in ASCII)
        - s2[1] = 'b' = 98 (in ASCII)
        - ((unsigned char)s1[1] - (unsigned char)s2[1]) = 0
        - s1[2] = 'c' = 99 (in ASCII)
        - s2[2] = 'd' = 100 (in ASCII)
        - ((unsigned char)s1[2] - (unsigned char)s2[2]) = 255
        
        If we didn't cast the characters to **`unsigned char`**, the result of the last subtraction would be -1, which would be interpreted as 255 when stored as an unsigned value.
        
        Casting the characters to **`unsigned char`** ensures that the result of the subtraction is always non-negative, avoiding any potential issues with the interpretation of the result.
        
- strnstr
    
    By casting `haystack` to `(char *)` before returning it, you explicitly tell the compiler that you are aware that you are returning a non-const pointer to the same memory location. This cast can help to avoid compiler warnings or errors in some cases, depending on the compiler and the specific compiler flags used.
    
- strdup
    - `ft_strlen + 1`
        
        The `ft_strlen()` function returns the length of the input string `s1`, not including the null-terminating character. When creating a duplicate of the string, it is necessary to allocate enough memory to store both the characters in the string and the null-terminating character.
        
- atoi
    - `sign = 44 - *str++;`
        - ASCII values of '+' and '-' are 43 and 45.
        - By subtracting the current character's ASCII value from 44, the function assigns the correct sign value (-1 or 1) to the `sign` variable,
        - while also incrementing the `str` pointer to the next character in the input string.
    - atoi should handle overflow ?
        
        [**Based on the Marcus Manley arguments.**](https://stackoverflowteams.com/c/42network/questions/2089/2090#2090)
        
        The [GNU implementation](https://github.com/lattera/glibc/blob/master/stdlib/atoi.c) of `atoi()` is basically `strtol()` cast to `int`, which I think explains the return values this question is about—as mentioned in a previous answer, `strtol()` returns `LONG_MIN` and `LONG_MAX` in case of underflow and overflow, respectively, which (more or less arbitrarily) turn to 0 and -1, respectively, when cast to `int` by `atoi()`.
        
        So I am suggesting that this behavior is not essential, and unnecessary to emulate, as it is an artifact of this one specific implementation, **not a standard feature of the function**.
        
        In contrast, there is for example the [musl implemenation](https://github.com/rofl0r/musl/blob/master/src/stdlib/atoi.c), which doesn't rely on `strtol()`. It's a direct ASCII-to-integer conversion, without any error checking or range checking.
        
        [**cppreference**](https://en.cppreference.com/w/c/string/byte/atoi) :
        
        If the value of the result cannot be represented, i.e. the converted value falls out of range of the corresponding return type, the behavior is undefined.
        
        **[ISO/IEC 9899:1999 | P306](https://www.dii.uchile.cl/~daespino/files/Iso_C_1999_definition.pdf)** : 
        
        If the value of the result cannot be represented, the behavior is undefined.
        Except for the behavior on error, they are equivalent to
        `atoi: (int)strtol(nptr, (char **)NULL, 10)`
        
        [**Linux manual page**](https://manpages.courier-mta.org/htmlman3/atoi.3.html) : “except that `atoi`() does not detect errors.”
        
<br />
<br />

## General standards

- Why to check the `malloc`?
    
    It is important to check if **`malloc`** worked and returned a valid pointer because **`malloc`** can fail in certain situations, such as:
    
    1. Insufficient memory: If the system is low on memory or there is not enough contiguous memory available, **`malloc`** may fail to allocate memory and return **`NULL`**.
    2. Memory fragmentation: If there are many small allocations and deallocations, the memory can become fragmented, making it difficult for **`malloc`** to find a large enough contiguous block of memory.
    3. Memory allocation limits: Some systems impose limits on the amount of memory that a process can allocate. If the limit is reached, **`malloc`** may fail and return **`NULL`**.
    
    If **`malloc`** fails and returns **`NULL`**, attempting to use the pointer returned by **`malloc`** can result in undefined behavior and may cause your program to crash or behave unpredictably. Therefore, it is important to check the return value of **`malloc`** to ensure that the memory allocation was successful before using the allocated memory.
    
- Why do not protect functions?
    
    > In the context of 42 I advise you to protect your functions in order to avoid long debates with your corrector.
    > 
    
    There can be disadvantages to protecting functions by checking if parameters are not `NULL`. Here are some potential drawbacks:
    
    1. **Increased code complexity**: Checking for `NULL` parameters in every function can increase the complexity of the code and make it harder to read and understand.
    2. **Performance** impact: Adding checks for `NULL` parameters can also impact performance, especially if the checks are done frequently or on large data sets.
    3. **Masking the real problem**: If `NULL` parameters are not expected or should never occur, then it may be better to let the program crash or produce an error message so that the underlying problem can be identified and fixed.
    4. **False sense of security**: Protecting functions by checking for `NULL` parameters can give a false sense of security if other problems in the code (such as memory leaks or buffer overflows) are not addressed.
    
    That being said, protecting functions by checking for `NULL` parameters can be a good practice in many cases, especially when dealing with user input or external data that may not be trustworthy. It ultimately depends on the specific requirements and constraints of the project.
    
- Can I pass a int variable in `malloc` without cast it in `size_t`?
    
    Yes, you can pass an `int` variable to `malloc()` without explicitly casting it to `size_t`. The C compiler will automatically perform an implicit conversion from `int` to `size_t` when calling the function. However, it's important to note that this could lead to potential issues if the `int` variable has a negative value, as `size_t` is an unsigned type.
    
<br />
<br />

## Header & Makefile

- libft.h includes purpose
    - `<stddef.h>`: This header file defines several useful types and macros. Some of the most important definitions include:
        - `size_t`: An unsigned integer type used to represent sizes of objects in memory, typically returned by functions like `strlen()` and `sizeof()`.
        - `NULL` macro
    - `<unistd.h>`: This header file provides access to various POSIX operating system APIs. Some of the key functions and features include:
        - `read()`, `write()`, and `close()`: Functions for working with file descriptors.
        - `sleep()`: A function to suspend the execution of the calling thread for a specified time.
    - `<stdlib.h>`: This header file provides a wide range of general-purpose functions and macros. Some of the most important features include:
        - `malloc()`, `calloc()`, `realloc()`, and `free()`: Functions for memory allocation and deallocation.
- Makefile
    - General purpose
        
        A Makefile is a build configuration file used by the `make` command to automate the build process for a project. It provides a series of rules and dependencies that describe how to build your project, including the compilation of source files, linking libraries, and cleaning up intermediate files.
        
        1. **Dependency management**: Tracks dependencies, optimizing build time.
        2. **Automation**: Streamlines compilation and linking, saving time and reducing errors.
        3. **Modularity**: Organizes separate rules and targets, simplifying management.
        4. **Reproducibility**: Ensures a consistent build process across environments.
        5. **Customization**: Allows easy adjustments to the build process.
        
        In summary, the Makefile is a powerful tool that simplifies and automates the build process for your `libft` library, making it easier to manage, maintain, and share your project with others.
        
    - target rule
        
        In a Makefile, targets usually represent file names, and the rules define how to build those files from other files (dependencies). When you run `make target`, the utility checks the file modification times to decide whether the target file needs to be rebuilt. If the target file is up-to-date (i.e., newer than its dependencies), Make doesn't execute the associated commands.
        
        ```makefile
        target : pre-requisities
        				 recipe
        ```
        
    - Why cc and not gcc ?
        
        Both `cc` and `gcc` are used to compile C programs, but they represent different compilers:
        
        1. `cc`: This is a generic name for the system's default C compiler. On many systems, `cc` is a symlink to the actual compiler, such as `gcc`, `clang`, or another compiler. Using `cc` in your Makefile provides flexibility, as it will use the default C compiler on the system where your code is being built. This can be useful if you want to ensure compatibility with multiple compilers or if you are not specifically targeting a particular compiler.
        2. `gcc`: This is the GNU Compiler Collection's C compiler. If you explicitly use `gcc` in your Makefile, you are specifying that you want to use the GCC compiler to build your project. This can be helpful if your code relies on features specific to GCC or if you want to ensure that everyone working on the project uses the same compiler.
        
        Using `cc` in your Makefile is generally recommended for portability, as it allows your code to be built with the default C compiler on the target system. However, if you have specific requirements that depend on a particular compiler (like GCC), you may choose to use that compiler explicitly in your Makefile.
        
    - `-Wall -Wextra -Werror -I` : Whats the -I flag ?
        
        The `-I` flag in your Makefile is followed by a dot (.), which specifies the current directory as an include directory for the compiler. The purpose of the `-I` flag is to tell the compiler where to look for header files when processing the C source files.
        
        In your Makefile, the line `CFLAGS = -Wall -Wextra -Werror -I.` specifies that the compiler should look for header files in the current directory (where the dot represents the current directory). This means that when your source files include a header file (for example, `#include "libft.h"`), the compiler will search for the header in the current directory.
        
        In your specific case, since all the header files are located in the same directory as the source files, you could technically remove the `-I.` flag, and the compiler would still be able to find the header files. However, **it's a good practice to keep the `-I` flag, as it explicitly tells the compiler where to look for header files and makes your Makefile more maintainable and portable.**
        
        In more complex projects, you might have header files in different directories, and you would use the `-I` flag followed by the path to those directories (e.g., `-I./include`). This way, the compiler knows where to find the header files when building the project.
        
    - `ar rcs` command
        - `ar` : archive command, a Unix 
        utility used to create, modify, and extract from archives. In this case,
         it's used to create a static library.
        - `rcs`: These are options passed to the `ar` command.
            - `r`: Replace or add the specified files to the archive. If the files don't exist in the archive, they are added; if they do exist, their content is replaced with the new content.
            - `c`: Create the archive if it doesn't already exist.
            - `s`: Write an object-file index into the archive, which can speed up linking to the library.
    - `.PHONNY`
        
        `PHONNY` is a special target in a Makefile to prevent from searching for files that have the same name as the rules (for example, a file named "all").
        
    - Why Makefile works without this rule?
		```makefile
		`%.o: %.c
		$(CC) $(CFLAGS) -c $< -o $@`
		```

        
        Instead, the Makefile relies on the built-in implicit rules provided by `make`, similar to the `%.o: %.c` rule I described earlier.
        
        However, if you want to customize the rule for building object files, you can add the `%.o: %.c` rule with a custom recipe. This will override the built-in rule, allowing you to add your own flags or other customizations to the build process. If the default behavior is sufficient for your project, there is no need to add these lines.
        
        By specifying the pattern rule, you ensure that the compilation process is consistent across different systems.
