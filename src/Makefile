#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM = HOST

# Architectures Specific Flags
LINKER_FILE = -T../msp432p401r.lds
CPU = cortex-m4
ARCH = armv7e-m
SPECS = nosys.specs

# Compiler Flags and Defines
TARGET= c1m2
LDFLAGS = -Wl, -Map=$(TARGET).map -T $(LINKER_FILE) 
CPPFLAGS = -Wall -Werror -g -O0 -std=c99 $(INCLUDES) 

ifeq ($(PLATFORM),HOST)
	CC=gcc
	CFLAGS = -DHOST
	LD = ld 
else
	CC=arm-none-eabi-gcc
	CFLAGS =  -DMSP432 -mcpu=$(CPU) -march=$(ARCH) --specs=$(SPECS) -mfloat-abi=hard -mfpu=fpv4-sp-d16
	LD = arm-none-eabi-ld
endif

OBJS = $(SOURCES:.c=.o)

%.o : %.c 
	$(CC) -c $< $(CFLAGS) $(CPPFLAGS) -o $@
%.i : %.c
	$(CC) -E $< $(CFLAGS) $(CPPFLAGS) -o $@
%.asm : %.c
	$(CC) -c $< $(CFLAGS) $(CPPFLAGS) -o $@
	objdump -S $@
%.d : %.c
	$(CC) -M $< $(CFLAGS) $(CPPFLAGS) -o $@

.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS) 
	$(CC) $(OBJS) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map *asm *i


