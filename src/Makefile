#------------------------------------------------------------------------------
# This fie is the Makefile of the C1M2 module in embedded software course in
# Coursera. It performs the build process management
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
# 1. %.i
# Generate the preprocessed output of all c-program implementation files (use 
# the –E flag).
#
# 2. %.asm
# Generate assembly output of c-program implementation files and the final
# output executable
# 
# 3. %.o
# Generate the object file for all c-source files (but do not link) by specifying the object
# file you want to compile.
#
# 4. compile-all
# Compile all object files, but DO NOT link.
#
# 5. Build
# Compile all object files and link into a final executable.
#
# 6. Clean
# This should remove all compiled objects, preprocessed outputs, assembly outputs, executable
# files and build output files.
#
# Platform Overrides:
#      MSP432, HOST
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
#PLATFORM = MSP432

# Common defines
TARGET = c1m2
CFLAGS = -g -Wall -Werror -std=c99

# Architectures Specific Flags
ifeq ($(PLATFORM),MSP432)
	CPU = -mcpu=cortex-m4
	ARCH = -mthumb \
		-march=armv7e-m \
		-mfloat-abi=hard \
		-mfpu=fpv4-sp-d16
	SPECS = --specs=nosys.specs
	ARM_FLAGS = $(CPU) $(ARCH) $(SPECS)
endif

# Compiler Flags and Defines
ifeq ($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	ASM = arm-none-eabi-objdump
	LINKER_PATH = $(HOME)
	LINKER = -T msp432p401r.lds
	LDFLAGS = $(LINKER) #-Map=$(TARGET).map
else
	CC = gcc
endif

OBJS = $(SOURCES:.c=.o)

################ BULD TARGET #############
# %.i
# Generate the preprocessed output of all c-program implementation files

%.i : %.c
ifeq ($(PLATFORM), MSP432)
	$(CC) $(INCLUDES) $(CFLAGS) $(ARM_FLAGS) -E $< -DMSP432 -o $@
else
	$(CC) $(INCLUDES) $(CFLAGS) -E $< -DHOST -o $@
endif


# %.asm
# Generate assembly output of c-program implementation files and the final output executable
%.asm : %.out
ifeq ($(PLATFORM), MSP432)
	$(ASM) -S $< -DMSP432
endif


# %.o
# Generate the object file for all c-source files (but do not link) by specifying the object
# file you want to compile.
%.o : %.c
ifeq ($(PLATFORM), MSP432)
	$(CC) $(INCLUDES) $(CFLAGS) $(ARM_FLAGS) -c $< -DMSP432 -o $@
else
	$(CC) $(INCLUDES) $(CFLAGS) -c $< -DHOST -o $@
endif


# compile-all
# Compile all object files, but DO NOT link.
.PHONY: compile-all
compile-all:
ifeq ($(PLATFORM), MSP432)
	$(CC) $(INCLUDES) $(CFLAGS) $(ARM_FLAGS) -c $(SOURCES) -DMSP432
else
	$(CC) $(INCLUDES) $(CFLAGS) -c $(SOURCES) -DHOST
endif


# Build
# Compile all object files and link into a final executable.
.PHONY : build
build : $(TARGET).out
ifeq ($(PLATFORM), MSP432)
$(TARGET).out: $(OBJS)
	$(CC) -L $(LINKER_PATH) $(LDFLAGS) $(ARM_FLAGS) $(OBJS) -DMSP432 -o $@
else
$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) -DHOST -o $@
endif


# Clean
# This should remove all compiled objects, preprocessed outputs, assembly outputs, executable
# files and build output files.
.PHONY:clean
clean :
	rm -f $(OBJS) $(TARGET).out $(TARGET).map
	rm -f *.o *.i *.map *.out *.asm 

