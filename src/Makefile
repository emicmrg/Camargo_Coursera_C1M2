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
#PLATFORM = 

# Architectures Specific Flags
ifeq($(PLATFORM), MSP432)
	LINKER_FILE = msp432p401r.lds
	CPU = cortex-m4
	ARCH = thumb
	SPECS = nosys.specs
endif

# Compiler Flags and Defines
ifeq($(PLATFORM), HOST)
	CC = gcc
	LD = gcc-ld
	LDFLAGS = 
	CFLAGS = 
	CPPFLAGs = 
else
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	LDFLAGS = 
	CFLAGS = 
	CPPFLAGs = 
endif