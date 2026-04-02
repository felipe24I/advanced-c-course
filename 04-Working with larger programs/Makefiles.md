## Makefile
A Makefile is a build automation file that defines how to compile and link a program. 
It specifies dependencies between files and the commands needed to create targets, allowing incremental builds (only recompiling what changed).

### Basic Makefile Structure
```makefile
# Comments start with hash symbol
target: dependencies
	commands to build target
# Note: commands MUST be preceded by a TAB character, not spaces!
```

## Simple Examples
### Example 1: Single File
```makefile
# Simplest Makefile
program: main.c
	gcc main.c -o program

# Usage: make program
```

### Example 2: Multiple Files
```makefile
# Basic multi-file Makefile
program: main.o utils.o
	gcc main.o utils.o -o program

main.o: main.c utils.h
	gcc -c main.c -o main.o

utils.o: utils.c utils.h
	gcc -c utils.c -o utils.o

clean:
	rm -f *.o program
```

## Essential Makefile Components
### 1. Variables
```makefile
# Defining variables
CC = gcc
CFLAGS = -Wall -Wextra -O2
LDFLAGS = -lm
TARGET = myprogram
SRCS = main.c utils.c math.c
OBJS = $(SRCS:.c=.o)

# Using variables
$(TARGET): $(OBJS)
	$(CC) $(OBJS) -o $(TARGET) $(LDFLAGS)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(TARGET)
```

### 2. Automatic Variables
```makefile
# Common automatic variables:
# $@ - target name
# $< - first dependency
# $^ - all dependencies
# $? - dependencies newer than target
# $* - stem (part matching % pattern)

example: file1.c file2.c file3.c
	gcc -o $@ $^          # $@ = example, $^ = all .c files
	echo "Target: $@"
	echo "First dependency: $<"
	echo "All dependencies: $^"

pattern: %.o: %.c
	gcc -c $< -o $@       # $< = first .c file, $@ = .o file
```

### 3. Pattern Rules
```makefile
# Generic rule for compiling .c to .o
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

# For specific patterns
src/%.o: src/%.c
	$(CC) $(CFLAGS) -c $< -o $@

# Multiple patterns
%.o: %.c %.h
	$(CC) $(CFLAGS) -c $< -o $@
```

## Practical Makefile Examples
### Example 1: Simple C Project
```makefile
# ============================================
# Simple C Project Makefile
# ============================================

# Compiler and flags
CC = gcc
CFLAGS = -Wall -Wextra -Werror -O2 -g
LDFLAGS = -lm

# Project files
TARGET = myapp
SRCS = main.c calculator.c io.c
OBJS = $(SRCS:.c=.o)
DEPS = $(SRCS:.c=.d)

# Default target
all: $(TARGET)

# Link object files
$(TARGET): $(OBJS)
	$(CC) $(OBJS) -o $@ $(LDFLAGS)
	@echo "Build complete: $@"

# Compile C files
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

# Generate dependencies
%.d: %.c
	@$(CC) -MM $< > $@.tmp
	@sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.tmp > $@
	@rm -f $@.tmp

# Include dependency files
-include $(DEPS)

# Clean build artifacts
clean:
	rm -f $(OBJS) $(DEPS) $(TARGET)
	@echo "Cleaned"

# Run program
run: $(TARGET)
	./$(TARGET)

# Debug build
debug: CFLAGS += -DDEBUG -g
debug: clean all

# Phony targets (not actual files)
.PHONY: all clean run debug

# Help
help:
	@echo "Available targets:"
	@echo "  all    - Build program (default)"
	@echo "  clean  - Remove build artifacts"
	@echo "  run    - Build and run program"
	@echo "  debug  - Build with debug symbols"
	@echo "  help   - Show this help"
```

### Example 2: Multiple Directory Project
```makefile
# ============================================
# Multi-Directory Project Makefile
# ============================================

# Project structure:
# .
# ├── Makefile
# ├── src/
# │   ├── main.c
# │   ├── module1.c
# │   └── module2.c
# ├── include/
# │   ├── module1.h
# │   └── module2.h
# ├── lib/
# │   └── external.a
# └── build/
#     ├── obj/
#     └── bin/

# Compiler settings
CC = gcc
CFLAGS = -Wall -Wextra -O2 -I./include
LDFLAGS = -L./lib -lexternal -lm

# Directories
SRC_DIR = src
INC_DIR = include
LIB_DIR = lib
BUILD_DIR = build
OBJ_DIR = $(BUILD_DIR)/obj
BIN_DIR = $(BUILD_DIR)/bin

# Files
TARGET = $(BIN_DIR)/program
SRCS = $(wildcard $(SRC_DIR)/*.c)
OBJS = $(SRCS:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)
DEPS = $(OBJS:.o=.d)

# Create directories
$(shell mkdir -p $(OBJ_DIR) $(BIN_DIR))

# Default target
all: $(TARGET)

# Link
$(TARGET): $(OBJS)
	$(CC) $(OBJS) -o $@ $(LDFLAGS)
	@echo "Program built: $@"

# Compile
$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c
	$(CC) $(CFLAGS) -c $< -o $@
	@echo "Compiled: $< -> $@"

# Generate dependencies
$(OBJ_DIR)/%.d: $(SRC_DIR)/%.c
	@$(CC) -MM $(CFLAGS) $< | \
		sed 's|\(.*\)\.o:|$(OBJ_DIR)/\1.o $(OBJ_DIR)/\1.d:|' > $@

# Include dependencies
-include $(DEPS)

# Clean
clean:
	rm -rf $(BUILD_DIR)
	@echo "Cleaned build directory"

# Run
run: $(TARGET)
	./$(TARGET)

# Install
install: $(TARGET)
	cp $(TARGET) /usr/local/bin/

# Debug
debug: CFLAGS += -DDEBUG -g
debug: clean all

.PHONY: all clean run install debug
```

### Example 3: Library Project
```makefile
# ============================================
# Static Library Makefile
# ============================================

# Library settings
LIB_NAME = mymath
LIB_VERSION = 1.0
LIB_SO_VERSION = 1

# Compiler settings
CC = gcc
CFLAGS = -Wall -Wextra -O2 -fPIC
AR = ar
ARFLAGS = rcs

# Directories
SRC_DIR = src
INC_DIR = include
LIB_DIR = lib
BUILD_DIR = build

# Files
SRCS = $(wildcard $(SRC_DIR)/*.c)
OBJS = $(SRCS:$(SRC_DIR)/%.c=$(BUILD_DIR)/%.o)

# Library targets
STATIC_LIB = $(LIB_DIR)/lib$(LIB_NAME).a
SHARED_LIB = $(LIB_DIR)/lib$(LIB_NAME).so.$(LIB_VERSION)
SHARED_LIB_MAJOR = $(LIB_DIR)/lib$(LIB_NAME).so.$(LIB_SO_VERSION)
SHARED_LIB_LINK = $(LIB_DIR)/lib$(LIB_NAME).so

# Create directories
$(shell mkdir -p $(BUILD_DIR) $(LIB_DIR))

# Default target
all: static shared

# Static library
static: $(STATIC_LIB)

$(STATIC_LIB): $(OBJS)
	$(AR) $(ARFLAGS) $@ $^
	@echo "Static library created: $@"

# Shared library
shared: $(SHARED_LIB)

$(SHARED_LIB): $(OBJS)
	$(CC) -shared -Wl,-soname,$(SHARED_LIB_MAJOR) -o $@ $^
	ln -sf $(notdir $@) $(SHARED_LIB_MAJOR)
	ln -sf $(notdir $@) $(SHARED_LIB_LINK)
	@echo "Shared library created: $@"

# Compile object files
$(BUILD_DIR)/%.o: $(SRC_DIR)/%.c
	$(CC) $(CFLAGS) -I$(INC_DIR) -c $< -o $@

# Install libraries
install: static shared
	cp $(STATIC_LIB) /usr/local/lib/
	cp $(SHARED_LIB) /usr/local/lib/
	cp $(SHARED_LIB_MAJOR) /usr/local/lib/
	cp $(SHARED_LIB_LINK) /usr/local/lib/
	cp $(INC_DIR)/*.h /usr/local/include/
	ldconfig

# Clean
clean:
	rm -rf $(BUILD_DIR) $(LIB_DIR)

# Test program
test: static
	$(CC) -o test test.c -I$(INC_DIR) -L$(LIB_DIR) -l$(LIB_NAME) -lm
	./test

.PHONY: all static shared clean install test
```

## Makefile Functions
```makefile
# String functions
FILES = main.c utils.c math.c
OBJS = $(patsubst %.c, %.o, $(FILES))      # main.o utils.o math.o
NAMES = $(basename $(FILES))                # main utils math
DIR = $(dir $(FILES))                       # ./ ./ ./
EXT = $(suffix $(FILES))                    # .c .c .c

# Wildcard function
C_FILES = $(wildcard src/*.c)               # All .c files in src/
H_FILES = $(wildcard include/*.h)           # All .h files in include/

# Shell function
CURRENT_DIR = $(shell pwd)
DATE = $(shell date +%Y-%m-%d)
UNAME = $(shell uname)

# Conditional functions
ifeq ($(UNAME), Linux)
	LDFLAGS += -lrt
else ifeq ($(UNAME), Darwin)
	LDFLAGS += -framework CoreFoundation
endif

# Foreach function
SRCS = main.c utils.c math.c
OBJS = $(foreach file, $(SRCS), $(BUILD_DIR)/$(file:.c=.o))

# Call function
reverse = $(2) $(1)
RESULT = $(call reverse, hello, world)     # world hello
```

## Makefile Conditionals
``` Makefile
# Conditional compilation based on variables
DEBUG = 1
RELEASE = 0

ifeq ($(DEBUG), 1)
	CFLAGS += -g -O0 -DDEBUG
else
	CFLAGS += -O2 -DNDEBUG
endif

# Check if file exists
ifneq ($(wildcard config.local),)
	include config.local
endif

# Check compiler
ifeq ($(CC), gcc)
	CFLAGS += -Wno-unused-result
else ifeq ($(CC), clang)
	CFLAGS += -Wno-unused-command-line-argument
endif

# Multi-condition
ifdef DEBUG
	CFLAGS += -g
endif

ifndef RELEASE
	CFLAGS += -Wall
endif
```
