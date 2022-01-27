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
#PLATFORM = $(shell)
TARGET = c1m2

# Architectures Specific Flags
ifeq ($(PLATFORM), MSP432)
	LINKER_FILE = msp432p401r.lds
	CPU = cortex-m4
	ARCH = thumb
	SPECS = nosys.specs
endif

# Compiler Flags and Defines
ifeq ($(PLATFORM), MSP432)
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	LDFLAGS = -O0 -Map=$(TARGET).map -T $(LINKER_FILE)
	CFLAGS = -mcpu=$(CPU) -m$(ARCH) -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16 --specs=$(SPECS) -std=c99 -Wall -Werror -g
	SOURCES = $(MSP432_S)
	INCLUDES = $(MSP432_I)
else
	CC = gcc
	LD = ld
	LDFLAGS = -O0
	CFLAGS = -std=c99 -Wall -Werror -g
	CPPFLAGs =
	SOURCES = $(HOST_S)
	INCLUDES = $(HOST_I)
endif

OBJS = $(SOURCES:.c=.o)

%.i : %.c
	$(CC) -c -D$(PLATFORM) $(INCLUDES) $< $(CFLAGS) -o $@

%.o : %.c
	$(CC) -c -D$(PLATFORM) $(INCLUDES) $< $(CFLAGS) -o $@

.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map