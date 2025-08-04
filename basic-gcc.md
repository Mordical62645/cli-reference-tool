# GCC Basic Commands and Compilation

## What is GCC?
GCC (GNU Compiler Collection) is a collection of compilers for various programming languages, most notably C and C++. It's the standard compiler on most Linux systems and provides extensive optimization and debugging capabilities.

## Installation

### Ubuntu/Debian
```bash
sudo apt update
sudo apt install build-essential
```

### CentOS/RHEL/Fedora
```bash
sudo yum groupinstall "Development Tools"
# or
sudo dnf groupinstall "Development Tools"
```

### macOS
```bash
# Install Xcode Command Line Tools
xcode-select --install
```

### Windows
```bash
# Using MinGW
# Download and install MinGW-w64
# or use WSL for Linux-like environment
```

## Basic Compilation

### Simple Compilation
```bash
gcc [c filename].c -o [exe name]
./[exe name]
```

### Common Compilation Commands
- `gcc [c filename].c -o [exe name]` - Compile C file to executable
- `gcc [file1].c [file2].c -o [exe name]` - Compile multiple files
- `gcc -c [c filename].c` - Compile to object file (.o)
- `gcc [object file].o -o [exe name]` - Link object file to executable
- `gcc -E [c filename].c` - Preprocess only (show preprocessor output)
- `gcc -S [c filename].c` - Compile to assembly (.s file)

## Compiler Options

### Optimization Levels
- `gcc -O0 [c filename].c -o [exe name]` - No optimization (default)
- `gcc -O1 [c filename].c -o [exe name]` - Basic optimization
- `gcc -O2 [c filename].c -o [exe name]` - More optimization (recommended)
- `gcc -O3 [c filename].c -o [exe name]` - Maximum optimization
- `gcc -Os [c filename].c -o [exe name]` - Optimize for size

### Warning Options
- `gcc -Wall [c filename].c -o [exe name]` - Enable all common warnings
- `gcc -Wextra [c filename].c -o [exe name]` - Enable extra warnings
- `gcc -Werror [c filename].c -o [exe name]` - Treat warnings as errors
- `gcc -Wpedantic [c filename].c -o [exe name]` - Strict ISO C compliance

### Debugging Options
- `gcc -g [c filename].c -o [exe name]` - Include debug information
- `gcc -g3 [c filename].c -o [exe name]` - Include maximum debug info
- `gcc -ggdb [c filename].c -o [exe name]` - Include GDB-specific debug info

### Include and Library Options
- `gcc -I[include path] [c filename].c -o [exe name]` - Add include path
- `gcc -L[library path] [c filename].c -o [exe name]` - Add library path
- `gcc -l[library name] [c filename].c -o [exe name]` - Link with library
- `gcc -static [c filename].c -o [exe name]` - Static linking

## Advanced Compilation

### Preprocessor Directives
```bash
# Define macros
gcc -DDEBUG [c filename].c -o [exe name]
gcc -DMAX_SIZE=100 [c filename].c -o [exe name]

# Undefine macros
gcc -UDEBUG [c filename].c -o [exe name]

# Include paths
gcc -I[include path] [c filename].c -o [exe name]
```

### Multiple Source Files
```bash
# Compile multiple files together
gcc [file1].c [file2].c [file3].c -o [exe name]

# Or compile separately and link
gcc -c [file1].c
gcc -c [file2].c
gcc -c [file3].c
gcc [file1].o [file2].o [file3].o -o [exe name]
```

### Library Linking
```bash
# Link with math library
gcc [c filename].c -lm -o [exe name]

# Link with pthread
gcc [c filename].c -lpthread -o [exe name]

# Link with multiple libraries
gcc [c filename].c -lm -lpthread -lssl -lcrypto -o [exe name]
```

## Debugging with GDB

### Compile for Debugging
```bash
gcc -g -Wall [c filename].c -o [exe name]
```

### Basic GDB Commands
```bash
gdb program                    # Start GDB with program
(gdb) break main              # Set breakpoint at main
(gdb) run                     # Run program
(gdb) next                    # Step over
(gdb) step                    # Step into
(gdb) continue                # Continue execution
(gdb) print variable          # Print variable value
(gdb) bt                      # Show backtrace
(gdb) quit                    # Exit GDB
```

## Common Compilation Examples

### 1. Basic C Program
```bash
# hello.c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}

# Compile and run
gcc [c filename].c -o [exe name]
./[exe name]
```

### 2. Program with Math Library
```bash
# math_program.c
#include <stdio.h>
#include <math.h>

int main() {
    double x = 2.0;
    printf("sqrt(%f) = %f\n", x, sqrt(x));
    return 0;
}

# Compile with math library
gcc [c filename].c -lm -o [exe name]
./[exe name]
```

### 3. Multi-file Program
```bash
# main.c
#include <stdio.h>
#include "helper.h"

int main() {
    int result = add(5, 3);
    printf("5 + 3 = %d\n", result);
    return 0;
}

# helper.h
int add(int a, int b);

# helper.c
#include "helper.h"

int add(int a, int b) {
    return a + b;
}

# Compile
gcc [file1].c [file2].c -o [exe name]
./[exe name]
```
```

## Makefile Examples

### Simple Makefile
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -g
TARGET = program
SOURCES = main.c helper.c
OBJECTS = $(SOURCES:.c=.o)

$(TARGET): $(OBJECTS)
	$(CC) $(OBJECTS) -o $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJECTS) $(TARGET)

.PHONY: clean
```

### Advanced Makefile
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -O2
DEBUG_CFLAGS = -Wall -Wextra -g -DDEBUG
LIBS = -lm -lpthread

TARGET = program
SOURCES = $(wildcard *.c)
OBJECTS = $(SOURCES:.c=.o)

all: $(TARGET)

debug: CFLAGS = $(DEBUG_CFLAGS)
debug: $(TARGET)

$(TARGET): $(OBJECTS)
	$(CC) $(OBJECTS) $(LIBS) -o $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJECTS) $(TARGET)

install: $(TARGET)
	cp $(TARGET) /usr/local/bin/

uninstall:
	rm -f /usr/local/bin/$(TARGET)

.PHONY: all debug clean install uninstall
```

## Troubleshooting

### Common Errors and Solutions
```bash
# "command not found" - Install build tools
sudo apt install build-essential

# "undefined reference" - Link with required library
gcc [c filename].c -lm  # for math functions

# "fatal error: header.h: No such file" - Add include path
gcc -I[include path] [c filename].c -o [exe name]

# "undefined reference to main" - Check for main function
# Ensure main function exists and is properly declared

# Segmentation fault - Compile with debug info
gcc -g -Wall [c filename].c -o [exe name]
# Then use GDB to debug
```

### Performance Optimization
```bash
# Profile-guided optimization
gcc -fprofile-generate [c filename].c -o [exe name]
./[exe name]  # Run with typical input
gcc -fprofile-use [c filename].c -o [exe name]

# Link-time optimization
gcc -flto -O2 [c filename].c -o [exe name]

# Auto-vectorization
gcc -O3 -march=native [c filename].c -o [exe name]
```

## Best Practices

### Compilation
- Always use `-Wall -Wextra` for warnings
- Use `-g` for debugging builds
- Use `-O2` for release builds
- Use `-std=c99` or `-std=c11` for modern C

### Code Organization
- Separate interface (.h) from implementation (.c)
- Use header guards to prevent multiple inclusion
- Keep functions small and focused
- Use meaningful variable and function names

### Debugging
- Use GDB for debugging, not printf statements
- Set breakpoints at function entry points
- Use conditional breakpoints for specific conditions
- Examine memory with GDB's memory inspection tools 