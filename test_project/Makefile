# Base makefile for project.

# firmware name
NAME = test

# where to put temporary file during compilation
BUILD_PATH = ./build

# STM32Cube L4 path
STM32L4_CUBE_PATH = /home/dylan/Bureau/Cours/Robotique/_Temps_reel/STM32_2A/STM32Cube_FW_L4_V1.14.0/

SOURCE = main.c 

# path to our source code
SOURCE_PATH = ./

# include path macro: -I<include path>
INCLUDE = -I./ -I$(STM32L4_CUBE_PATH)/Drivers/BSP/STM32L4xx_Nucleo


# STM32 CUbe L4 HAL source code
SOURCE_PATH += 
SOURCE += 

# startup code
STARTUP_FILE = $(STM32L4_CUBE_PATH)/Drivers/CMSIS/Device/ST/STM32L4xx/Source/Templates/gcc/startup_stm32l476xx.s

# system startup code
SOURCE_PATH += $(STM32L4_CUBE_PATH)/Drivers/CMSIS/Device/ST/STM32L4xx/Source/Templates/
SOURCE += system_stm32l4xx.c

# include path for the system startup code
INCLUDE += -I./ -I$(STM32L4_CUBE_PATH)/Drivers/CMSIS/Device/ST/STM32L4xx/Include/

INCLUDE += -I./ -I$(STM32L4_CUBE_PATH)/Drivers/CMSIS/Include

INCLUDE += -I./ -I$(STM32L4_CUBE_PATH)/Drivers/STM32L4xx_HAL_Driver/Inc



# preprocessor definition -D<preprocessor>
DEFINE = -DSTM32L476xx -DUSE_HAL_DRIVER -DUSE_STM32L4XX_NUCLEO


## FreeRTOS
#FREE_RTOS_PATH = <todo-path-to-your-freertos>
#SOURCE_PATH += 
#SOURCE += 
#INCLUDE += -I$(FREE_RTOS_PATH)/

# linker file
CUSTOM_LINKER_FILE = stm32l476.ld

# external libraries, if you have it
LIBS =


# optimization level
OPTIMIZE = -Os

# cpu architecture
CPU = -mcpu=cortex-m4 -mfpu=fpv4-sp-d16 -mfloat-abi=hard -mthumb

# compiler options (CFLAGS):
COMPILER_OPTION	=$(OPTIMIZE)
COMPILER_OPTION += -Wall
COMPILER_OPTION += -Wextra
COMPILER_OPTION += $(CPU)
COMPILER_OPTION += -ffunction-sections
COMPILER_OPTION	+= -fdata-sections
COMPILER_OPTION += -Wno-strict-aliasing
COMPILER_OPTION += -std=c99
COMPILER_OPTION += --specs=nano.specs

# activate debug for jtag :
#COMPILER_OPTION += -ggdb 

# assembler options
ASSEMBLER_OPTION =$(CPU) -c -x assembler-with-cpp --specs=nano.specs


# linker options (LDFLAGS):
LINKER_OPTION =-T$(CUSTOM_LINKER_FILE)
LINKER_OPTION += --specs=nosys.specs
LINKER_OPTION += -Wl,-Map=$(BUILD_PATH)/main.map

# if we dont want default library into the firmware
#LINKER_OPTION += -Wl,--gc-sections
#LINKER_OPTION += -nostartfiles -nodefaultlibs

LINKER_OPTION += -static
LINKER_OPTION += -Wl,--cref --specs=nano.specs
LINKER_OPTION += $(CPU)
LINKER_OPTION += -Wl,--start-group -lc -lm -Wl,--end-group 


TOOLS_PATH= /home/dylan/Bureau/Cours/Robotique/_Temps_reel/STM32_2A/gcc-arm-none-eabi-9-2019-q4-major/bin/

EXE=$(BUILD_PATH)/$(NAME).hex
EXE_MAIN=$(BUILD_PATH)/$(NAME).elf
EXE_BIN=$(BUILD_PATH)/$(NAME).bin

HEX=$(TOOLS_PATH)arm-none-eabi-objcopy
GCC=$(TOOLS_PATH)arm-none-eabi-gcc
ASM=$(TOOLS_PATH)arm-none-eabi-as
SIZE=$(TOOLS_PATH)arm-none-eabi-size
DISASM=$(TOOLS_PATH)arm-none-eabi-objdump

COMPILER=$(GCC) $(COMPILER_OPTION) $(DEFINE) $(INCLUDE) -c
LINKER=$(GCC) $(LINKER_OPTION) -o $(EXE_MAIN)

# Object search path:
OBJ_FILES =$(patsubst %.c,$(BUILD_PATH)/%.o,$(SOURCE))


## compiler & linker macro :

build: $(OBJ_FILES)
	@echo $(STARTUP_FILE)
	@$(GCC) $(ASSEMBLER_OPTION) -o build/stm32l476_startup.o $(STARTUP_FILE)
	@echo $<
	@$(LINKER) $(OBJ_FILES) build/stm32l476_startup.o $(LIBS)
	@$(HEX) --strip-unneeded -O binary $(EXE_MAIN) $(EXE_BIN)
	# output size of firmware
	@$(SIZE) --target=binary $(EXE_BIN)
	
vpath %.c $(SOURCE_PATH)

$(BUILD_PATH)/%.o : %.c
	@echo $<
	@$(COMPILER) -o $@ $<

# get size of various firmware segment
mem:
	$(SIZE) -A $(EXE_MAIN)
	
# dissassemble firmware
dump:
	$(DISASM) -d $(EXE_MAIN)
	
clean:
	rm $(BUILD_PATH)/*

cleanbuild:
	rm $(BUILD_PATH)/*

