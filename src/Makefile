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
# Use: make [TAR] [PLATFORM-OVERRIDES]
#
# Build TARs:
#      <Put a description of the supported TARs here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
# Platform Overrides

include sources.mk

TAR = c1m2
OBJS = $(SOURCES:.c=.o)

# Architectures Specific Flags
ifeq ($(PLATFORM),MSP432)
	LINKER_FILE = ../msp432p401r.lds
	CPU = cortex-m4
	SPECS =	nosys.specs
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	CFLAGS = -Wall -Werror -g -O0 -std=c99 -mcpu=$(CPU) -mthumb --specs=$(SPECS) -mfpu=fpv4-sp-d16 -mfloat-abi=hard -march=armv7e-m 
	LDFLAGS = -Wl,-Map=$(TAR).map -T $(LINKER_FILE)
	CPPFLAGs = -D$(PLATFORM) 
else
	CC = gcc
	LD = ld
	CFLAGS = -Wall -Werror -g -O0 -std=c99
	CPPFLAGs = -DHOST
	LDFLAGS = -Wl,-Map=$(TAR).map
endif

%.i : %.c
	$(CC) -E  $(INCLUDES) $(CFLAGS) $(CPPFLAGs) $< -o $@

%.asm : %.o
	$(CC) -S  $(INCLUDES) $(CFLAGS) $(CPPFLAGs) $< -o $@

%.o : %.c
	$(CC) -c $(INCLUDES) $(CFLAGS) $(CPPFLAGs) $<

$(TAR).out: $(OBJS)
	$(CC) $(OBJS) $(INCLUDES) $(CFLAGS) $(CPPFLAGs) $(LDFLAGS) -o $@

# make build PLATFORM = HOST / MSP432
.PHONY: build
build: $(TAR).out

.PHONY: compile-all
compile-all: $(OBJS)

.PHONY: clean
clean:
	rm -rf *.o *.i *.asm $(TAR).out $(TAR).map