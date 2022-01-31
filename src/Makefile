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
#
# Use: make [File].extension PLATFORM=HOST/MSP432
#
# This command will create the file type you wish for the selected platform
#
#------------------------------------------------------------------------------
#
# Use: make build PLATFORM=HOST/MSP432
#
# This command will compile and link into a final executable
#
#------------------------------------------------------------------------------
#
# Use: make compile-all PLATFORM=HOST/MSP432
#
# This command will compile but do not link
#
#------------------------------------------------------------------------------
#
# Use: make clean PLATFORM=HOST/MSP432
#
# This command will remove all compiled objects, preprocessed output, assembly outputs,
# executable and build files
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM = $(shell)
TARGET = c1m2

# Architectures Specific Flags
ifeq ($(PLATFORM), MSP432)
	LINKER_FILE = ../msp432p401r.lds
	CPU = cortex-m4
	ARCH = thumb
	SPECS = nosys.specs
endif

# Compiler Flags and Defines
ifeq ($(PLATFORM), MSP432)
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	LDFLAGS = -O0 -Wl,-Map=$(TARGET).map -Wl,-T $(LINKER_FILE)
	CFLAGS = -mcpu=$(CPU) \
	         -m$(ARCH) \
			 -march=armv7e-m \
			 -mfloat-abi=hard \
			 -mfpu=fpv4-sp-d16 \
			 --specs=$(SPECS) \
			 -std=c99 \
			 -Wall \
			 -Werror
	CPPFLAGs = -mcpu=$(CPU) \
	           -m$(ARCH) \
			   -march=armv7e-m \
			   -mfloat-abi=hard \
			   -mfpu=fpv4-sp-d16 \
			   --specs=$(SPECS) \
			   -std=c99 \
			   -Wall \
			   -Werror \
			   -E
	SOURCES = $(MSP432_S)
	INCLUDES = $(MSP432_I)
endif
ifeq ($(PLATFORM), HOST)
	CC = gcc
	LD = ld
	LDFLAGS = -O0 -Wl,-Map=$(TARGET).map
	CFLAGS = -std=c99 -Wall -Werror
	CPPFLAGs = -std=c99 -Wall -Werror -E
	SOURCES = $(HOST_S)
	INCLUDES = $(HOST_I)
else
	SOURCES = $(MSP432_S)
	INCLUDES = $(MSP432_I)
endif

OBJS = $(SOURCES:.c=.o)
ASMF = $(SOURCES:.c=.asm)
LNKF = $(SOURCES:.c=.i)

%.asm : %.c
	$(CC) -D$(PLATFORM) $(INCLUDES) -g $(CFLAGS) -S $< -o $@
%.i : %.c
	$(CC) -D$(PLATFORM) $(INCLUDES) $(CPPFLAGs) $< -o $@

%.o : %.c
	$(CC) -c -D$(PLATFORM) $(INCLUDES) $(CFLAGS) $< -o $@

.PHONY: compile-all
compile-all: $(OBJS)

.PHONY: build
build: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(ASMF) $(LNKF) $(TARGET).out $(TARGET).map 