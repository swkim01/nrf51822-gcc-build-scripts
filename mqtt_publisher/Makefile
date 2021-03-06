PROJECT_NAME := iot_lwip_mqtt_publisher_pca10028

export OUTPUT_FILENAME
#MAKEFILE_NAME := $(CURDIR)/$(word $(words $(MAKEFILE_LIST)),$(MAKEFILE_LIST))
MAKEFILE_NAME := $(MAKEFILE_LIST)
MAKEFILE_DIR := $(dir $(MAKEFILE_NAME) ) 

TEMPLATE_PATH = ../../../../../../../components/toolchain/gcc
ifeq ($(OS),Windows_NT)
include $(TEMPLATE_PATH)/Makefile.windows
else
include $(TEMPLATE_PATH)/Makefile.posix
endif

OUTPUT_FILENAME := nrf51422_xxac_s1xx_iot
LINKER_SCRIPT=iot_lwip_mqtt_publisher_gcc_nrf51.ld
SDK_PATH = ../../../../../../..
SOFTDEVICE_HEX_PATH = $(SDK_PATH)/components/softdevice/s1xx_iot/s1xx-iot-prototype2_softdevice.hex

# if S110, start address for softdevice is 0x14000(v6), 0x16000(v7), 0x18000(v8)
# elseif S120, start address for softdevice is 0x1D000(v8)
# else 0x0
STARTADDRESS = 0x18000

#Flasher selection
#FLASHER := NRFJPROG
#FLASHER := JLink
FLASHER := OpenOCD

# Toolchain commands
NRFJPROG        := nrfjprog
JLINK           := JLinkExe -Device nrf51822 -speed 100 -if swd
#You need to change openOCD clause depending your dongle. I have stlink-v2 and it's working. Jlink will work if you fetch latest trunk from git.
OPENOCD         := /usr/local/bin/openocd -f interface/stlink-v2.cfg -c"transport select hla_swd" -f target/nrf51.cfg
#OPENOCD                := /usr/local/bin/openocd -f interface/jlink.cfg -c"transport select swd ; adapter_khz 1000 ; adapter_nsrst_delay 10" -f /home/death/de/nrf51.cfg

MK := mkdir
RM := rm -rf

#echo suspend
ifeq ("$(VERBOSE)","1")
NO_ECHO := 
else
NO_ECHO := @
endif

# Toolchain commands
CC       		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-gcc"
AS       		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-as"
AR       		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-ar" -r
LD       		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-ld"
NM       		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-nm"
OBJDUMP  		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-objdump"
OBJCOPY  		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-objcopy"
SIZE    		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-size"
GDB	    		:= "$(GNU_INSTALL_ROOT)/bin/$(GNU_PREFIX)-gdb"

#function for removing duplicates in a list
remduplicates = $(strip $(if $1,$(firstword $1) $(call remduplicates,$(filter-out $(firstword $1),$1))))

#source common to all targets
C_SOURCE_FILES += \
../../../../../../../components/libraries/button/app_button.c \
../../../../../../../components/libraries/util/app_error.c \
../../../../../../../components/libraries/fifo/app_fifo.c \
../../../../../../../components/libraries/scheduler/app_scheduler.c \
../../../../../../../components/libraries/timer/app_timer.c \
../../../../../../../components/libraries/timer/app_timer_appsh.c \
../../../../../../../components/libraries/trace/app_trace.c \
../../../../../../../components/libraries/mem_manager/mem_manager.c \
../../../../../../../components/libraries/util/nrf_assert.c \
../../../../../../../components/libraries/uart/retarget.c \
../../../../../../../external/lwip/src/app/mqtt/mqtt.c \
../../../../../../../external/lwip/src/port/nrf_platform_port.c \
../../../../../../../components/drivers_nrf/uart/app_uart_fifo.c \
../../../../../../../components/drivers_nrf/hal/nrf_delay.c \
../../../../../../../components/drivers_nrf/common/nrf_drv_common.c \
../../../../../../../components/drivers_nrf/gpiote/nrf_drv_gpiote.c \
../../../main.c \
../../../../../../../components/ble/common/ble_advdata.c \
../../../../../../../components/ble/common/ble_srv_common.c \
../../../../../../../components/iot/context_manager/iot_context_manager.c \
../../../../../../../components/toolchain/system_nrf51.c \
../../../../../../../components/softdevice/common/softdevice_handler/softdevice_handler.c \
../../../../../../../components/softdevice/common/softdevice_handler/softdevice_handler_appsh.c \

#assembly files common to all targets
ASM_SOURCE_FILES  = ../../../../../../../components/toolchain/gcc/gcc_startup_nrf51.s

#assembly files common to all targets
LIBS  = ../../../../../../../external/lwip/lib/liblwip.a
LIBS += ../../../../../../../components/iot/ble_6lowpan/lib/ble_6lowpan.a

#includes common to all targets
INC_PATHS  = -I../../../config
INC_PATHS += -I../../../../../../../external/lwip/src/app/mqtt
INC_PATHS += -I../../../../../../../components/softdevice/s1xx_iot/headers
INC_PATHS += -I../../../../../../../components/drivers_nrf/common
INC_PATHS += -I../../../../../../../components/softdevice/common/softdevice_handler
INC_PATHS += -I../../../../../../../external/lwip/src/include
INC_PATHS += -I../../../../../../../components/iot/ble_6lowpan
INC_PATHS += -I../../../../../../../components/drivers_nrf/hal
INC_PATHS += -I../../../../../../../components/ble/common
INC_PATHS += -I../../../../../../../external/lwip/src/port
INC_PATHS += -I../../../../../../../components/libraries/fifo
INC_PATHS += -I../../../../../../../components/libraries/trace
INC_PATHS += -I../../../../../../../components/device
INC_PATHS += -I../../../../../../../components/drivers_nrf/config
INC_PATHS += -I../../../../../../../components/libraries/mem_manager
INC_PATHS += -I../../../../../../../components/drivers_nrf/uart
INC_PATHS += -I../../../../../../../components/iot/ble_ipsp
INC_PATHS += -I../../../../../../../components/libraries/scheduler
INC_PATHS += -I../../../../../../../external/lwip/src/include/netif
INC_PATHS += -I../../../../../../../components/iot/include
INC_PATHS += -I../../..
INC_PATHS += -I../../../../../../../components/iot/context_manager
INC_PATHS += -I../../../../../../../components/toolchain/gcc
INC_PATHS += -I../../../../../../../components/drivers_nrf/gpiote
INC_PATHS += -I../../../../../../../components/libraries/timer
INC_PATHS += -I../../../../../../../external/lwip/src/port/arch
INC_PATHS += -I../../../../../../../components/libraries/button
INC_PATHS += -I../../../../../../../components/libraries/util
INC_PATHS += -I../../../../../../bsp
INC_PATHS += -I../../../../../../../components/toolchain

OBJECT_DIRECTORY = _build
LISTING_DIRECTORY = $(OBJECT_DIRECTORY)
OUTPUT_BINARY_DIRECTORY = $(OBJECT_DIRECTORY)

# Sorting removes duplicates
BUILD_DIRECTORIES := $(sort $(OBJECT_DIRECTORY) $(OUTPUT_BINARY_DIRECTORY) $(LISTING_DIRECTORY) )

#flags common to all targets
CFLAGS  = -D__HEAP_SIZE=512
CFLAGS += -DSWI_DISABLE0
CFLAGS += -DBOARD_PCA10001
CFLAGS += -DSOFTDEVICE_PRESENT
CFLAGS += -DNRF51
CFLAGS += -DS110
CFLAGS += -DBLE_STACK_SUPPORT_REQD
CFLAGS += -DBSP_DEFINES_ONLY
CFLAGS += -mcpu=cortex-m0
CFLAGS += -mthumb -mabi=aapcs --std=gnu99
CFLAGS += -Wall -Werror -O3
CFLAGS += -mfloat-abi=soft
# keep every function in separate section. This will allow linker to dump unused functions
CFLAGS += -ffunction-sections -fdata-sections -fno-strict-aliasing
CFLAGS += -fno-builtin --short-enums

# keep every function in separate section. This will allow linker to dump unused functions
LDFLAGS += -Xlinker -Map=$(LISTING_DIRECTORY)/$(OUTPUT_FILENAME).map
LDFLAGS += -mthumb -mabi=aapcs -L $(TEMPLATE_PATH) -T$(LINKER_SCRIPT)
LDFLAGS += -mcpu=cortex-m0
# let linker to dump unused sections
LDFLAGS += -Wl,--gc-sections
# use newlib in nano version
LDFLAGS += --specs=nano.specs -lc -lnosys

# Assembler flags
ASMFLAGS += -x assembler-with-cpp
ASMFLAGS += -D__HEAP_SIZE=512
ASMFLAGS += -DSWI_DISABLE0
ASMFLAGS += -DBOARD_PCA10028
ASMFLAGS += -DSOFTDEVICE_PRESENT
ASMFLAGS += -DNRF51
ASMFLAGS += -DS110
ASMFLAGS += -DBLE_STACK_SUPPORT_REQD
ASMFLAGS += -DBSP_DEFINES_ONLY
#default target - first one defined
default: clean nrf51422_xxac_s1xx_iot

#building all targets
all: clean
	$(NO_ECHO)$(MAKE) -f $(MAKEFILE_NAME) -C $(MAKEFILE_DIR) -e cleanobj
	$(NO_ECHO)$(MAKE) -f $(MAKEFILE_NAME) -C $(MAKEFILE_DIR) -e nrf51422_xxac_s1xx_iot

#target for printing all targets
help:
	@echo following targets are available:
	@echo 	nrf51422_xxac_s1xx_iot


C_SOURCE_FILE_NAMES = $(notdir $(C_SOURCE_FILES))
C_PATHS = $(call remduplicates, $(dir $(C_SOURCE_FILES) ) )
C_OBJECTS = $(addprefix $(OBJECT_DIRECTORY)/, $(C_SOURCE_FILE_NAMES:.c=.o) )

ASM_SOURCE_FILE_NAMES = $(notdir $(ASM_SOURCE_FILES))
ASM_PATHS = $(call remduplicates, $(dir $(ASM_SOURCE_FILES) ))
ASM_OBJECTS = $(addprefix $(OBJECT_DIRECTORY)/, $(ASM_SOURCE_FILE_NAMES:.s=.o) )

vpath %.c $(C_PATHS)
vpath %.s $(ASM_PATHS)

OBJECTS = $(C_OBJECTS) $(ASM_OBJECTS)

#nrf51422_xxac_s1xx_iot: OUTPUT_FILENAME := nrf51422_xxac_s1xx_iot
#nrf51422_xxac_s1xx_iot: LINKER_SCRIPT=iot_lwip_mqtt_publisher_gcc_nrf51.ld
nrf51422_xxac_s1xx_iot: $(BUILD_DIRECTORIES) $(OBJECTS)
	@echo Linking target: $(OUTPUT_FILENAME).out
	$(NO_ECHO)$(CC) $(LDFLAGS) $(OBJECTS) $(LIBS) -o $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out
	$(NO_ECHO)$(MAKE) -f $(MAKEFILE_NAME) -C $(MAKEFILE_DIR) -e finalize

## Create build directories
$(BUILD_DIRECTORIES):
	echo $(MAKEFILE_NAME)
	$(MK) $@

# Create objects from C SRC files
$(OBJECT_DIRECTORY)/%.o: %.c
	@echo Compiling file: $(notdir $<)
	$(NO_ECHO)$(CC) $(CFLAGS) $(INC_PATHS) -c -o $@ $<

# Assemble files
$(OBJECT_DIRECTORY)/%.o: %.s
	@echo Compiling file: $(notdir $<)
	$(NO_ECHO)$(CC) $(ASMFLAGS) $(INC_PATHS) -c -o $@ $<


# Link
$(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out: $(BUILD_DIRECTORIES) $(OBJECTS)
	@echo Linking target: $(OUTPUT_FILENAME).out
	$(NO_ECHO)$(CC) $(LDFLAGS) $(OBJECTS) $(LIBS) -o $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out


## Create binary .bin file from the .out file
$(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin: $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out
	@echo Preparing: $(OUTPUT_FILENAME).bin
	$(NO_ECHO)$(OBJCOPY) -O binary $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin

## Create binary .hex file from the .out file
$(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).hex: $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out
	@echo Preparing: $(OUTPUT_FILENAME).hex
	$(NO_ECHO)$(OBJCOPY) -O ihex $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).hex

finalize: genbin genhex echosize

genbin:
	@echo Preparing: $(OUTPUT_FILENAME).bin
	$(NO_ECHO)$(OBJCOPY) -O binary $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin

## Create binary .hex file from the .out file
genhex: 
	@echo Preparing: $(OUTPUT_FILENAME).hex
	$(NO_ECHO)$(OBJCOPY) -O ihex $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).hex

echosize:
	-@echo ""
	$(NO_ECHO)$(SIZE) $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out
	-@echo ""

clean:
	$(RM) $(BUILD_DIRECTORIES)

cleanobj:
	$(RM) $(BUILD_DIRECTORIES)/*.o

#flash: $(MAKECMDGOALS)
#	@echo Flashing: $(OUTPUT_BINARY_DIRECTORY)/$<.hex
#	nrfjprog --reset --program $(OUTPUT_BINARY_DIRECTORY)/$<.hex
#
## Flash softdevice
#flash_softdevice:
#	@echo Flashing: s110_nrf51_8.0.0_softdevice.hex
#	nrfjprog --program ../../../../../../components/softdevice/s110/hex/s110_nrf51_8.0.0_softdevice.hex -f nrf51 --chiperase
#	nrfjprog --reset

debug: $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin

# Create binary file from the softdevice 7 hex file
$(OUTPUT_BINARY_DIRECTORY)/softdevice.bin: $(BUILD_DIRECTORIES)
	$(OBJCOPY) -I ihex -O binary $(SOFTDEVICE_HEX_PATH) $(OUTPUT_BINARY_DIRECTORY)/softdevice.bin

# Create IUCR binary file from the softdevice 6 UICR hex file
$(OUTPUT_BINARY_DIRECTORY)/softdevice_uicr.bin: $(SOFTDEVICE_HEX_PATH)  $(BUILD_DIRECTORIES)
	$(OBJCOPY) -Iihex -Obinary --only-section .sec3 $< $@

# Create binary file from the softdevice 6 hex file
$(OUTPUT_BINARY_DIRECTORY)/softdevice_main.bin: $(SOFTDEVICE_HEX_PATH)  $(BUILD_DIRECTORIES)
	$(OBJCOPY) -Iihex -Obinary --remove-section .sec3 $< $@
#OpenOCD Flasher Scripts
ifeq ($(FLASHER), NRFJProg)
flash: $(MAKECMDGOALS)
	@echo Flashing: $(OUTPUT_BINARY_DIRECTORY)/$<.hex
	$(NRFJPROG) --program $(OUTPUT_BINARY_DIRECTORY)/$<.hex -f nrf51  --sectorerase
	$(NRFJPROG) --reset

# Flash softdevice
flash_softdevice: $(SOFTDEVICE_HEX_PATH)
	@echo Flashing: $(SOFTDEVICE_HEX_PATH)
	$(NRFJPROG) --program $(SOFTDEVICE_HEX_PATH) -f nrf51 --chiperase
	$(NRFJPROG) --reset

erase_all:
	$(NRFJPROG) --erase

pinreset:
	$(NRFJPROG) --reset
else
ifeq ($(FLASHER), OpenOCD)
flash: $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin
	$(OPENOCD) -c "program $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin $(STARTADDRESS) verify; shutdown"
	$(NO_ECHO)$(MAKE) -f $(MAKEFILE_NAME) -C $(MAKEFILE_DIR) -e pinreset

ifeq ($(USE_SOFTDEVICE), S110-v6)
flash_softdevice: $(OUTPUT_BINARY_DIRECTORY)/softdevice_main.bin $(OUTPUT_BINARY_DIRECTORY)/softdevice_uicr.bin
	$(OPENOCD) -c "init ; reset halt ; nrf51 mass_erase ; sleep 500 ; flash write_image $(OUTPUT_BINARY_DIRECTORY)/softdevice_uicr.bin 0x10001000 ; verify_image $(OUTPUT_BINARY_DIRECTORY)/softdevice_uicr.bin 0x10001000 ; flash write_image $(OUTPUT_BINARY_DIRECTORY)/softdevice_main.bin 0x0 ; verify_image $(OUTPUT_BINARY_DIRECTORY)/softdevice_main.bin 0; shutdown"
else
flash_softdevice: $(OUTPUT_BINARY_DIRECTORY)/softdevice.bin
	$(OPENOCD) -c "init ; reset halt ; nrf51 mass_erase ; sleep 500 ; flash write_image $(OUTPUT_BINARY_DIRECTORY)/softdevice.bin 0x0 ; verify_image $(OUTPUT_BINARY_DIRECTORY)/softdevice.bin 0x0 ; mww 0x4001e504 1 ; mww 0x10001000 $(STARTADDRESS) ; mww 0x4001e504 0 ; shutdown"
endif
erase_all:
	$(OPENOCD) -c "init ; reset halt ; nrf51 mass_erase ; shutdown"

pinreset:
	$(OPENOCD) -c "init ; reset halt ; mww 0x4001e504 2 ; mww 0x40000544 1 ; reset ; shutdown"

gdbserver:
	$(OPENOCD)

GDB_COMMAND_SCRIPT=$(OUTPUT_BINARY_DIRECTORY)/gdbinit
$(GDB_COMMAND_SCRIPT): debug
	echo -e -n "target remote localhost:3333    \n\
        monitor reset halt                \n\
        file $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out  \n\
        load                            \n\
        b main                          \n\
        b app_error_handler             \n\
        monitor reset                   \n\
        continue" > $(GDB_COMMAND_SCRIPT)

#JLink Flasher Scripts
else
$(OUTPUT_BINARY_DIRECTORY)/erase_all.jlink : $(BUILD_DIRECTORIES)
	echo " \n\
r \n\
h \n\
w4 0x4001e504,2 # enable erase all \n\
w4 0x4001e50c,1 # start erasing \n\
sleep 1000 \n\
w4 0x4001e514,1 # start erasing UICR \n\
sleep 500 \n\
r \n\
q \n\
  " \
    > $(OUTPUT_BINARY_DIRECTORY)/erase_all.jlink

erase_all: $(OUTPUT_BINARY_DIRECTORY)/erase_all.jlink
	$(JLINK) -CommanderScript $(OUTPUT_BINARY_DIRECTORY)/erase_all.jlink; [ "$$?" -eq 1 ]

$(OUTPUT_BINARY_DIRECTORY)/flash_softdevice.jlink: $(BUILD_DIRECTORIES)
	echo " \n\
r \n\
h \n\
w4 0x4001e504,2 # enable erase \n\
w4 0x4001e50c,1 # start erasing all \n\
sleep 1000 \n\
w4 0x4001e514,1 # start erasing UICR \n\
sleep 500 \n\
w4 0x10001000, $(STARTADDRESS) # write code region size \n\
sleep 1000 \n\
w4 0x4001e504,1 \n\
sleep 1000 \n\
loadbin \"$(OUTPUT_BINARY_DIRECTORY)/softdevice.bin\", 0x0 \n\
verifybin \"$(OUTPUT_BINARY_DIRECTORY)/softdevice.bin\", 0x0 \n\
r \n\
qc \n\
" > $(OUTPUT_BINARY_DIRECTORY)/flash_softdevice.jlink

flash_softdevice: $(OUTPUT_BINARY_DIRECTORY)/flash_softdevice.jlink $(OUTPUT_BINARY_DIRECTORY)/softdevice.bin
	$(JLINK) -CommanderScript $(OUTPUT_BINARY_DIRECTORY)/flash_softdevice.jlink; [ "$$?" -eq 1 ]

$(OUTPUT_BINARY_DIRECTORY)/flash.jlink: $(BUILD_DIRECTORIES) $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin
	echo " \n\
r \n\
h \n\
w4 0x4001e504,2 # enable erase \n\
" > $(OUTPUT_BINARY_DIRECTORY)/flash.jlink
	binsize=`stat -f %z $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin`;  \
    starthexaddr=$(STARTADDRESS); \
	startaddr=`printf "%d" $$starthexaddr`;  \
	page=0;  \
	while [ $$binsize -gt 0 ]; \
    do \
	echo "w4 0x4001e508, $$starthexaddr # start erasing code region 1 page $$page\n" >> $(OUTPUT_BINARY_DIRECTORY)/flash.jlink ; \
	binsize=`expr $$binsize - 1024`; \
	page=`expr $$page + 1`; \
	startaddr=`expr $$startaddr + 1024`; \
	starthexaddr=`printf "0x%x" $$startaddr`; \
    done
	echo "sleep 1000 \n\
r \n\
w4 0x4001e504,1  # enable write \n\
loadbin \"$(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin\", $(STARTADDRESS)\n\
verifybin \"$(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).bin\", $(STARTADDRESS)\n\
r \n\
g \n\
qc \n\
" >> $(OUTPUT_BINARY_DIRECTORY)/flash.jlink

flash: $(OUTPUT_BINARY_DIRECTORY)/flash.jlink
	$(JLINK) -CommanderScript $(OUTPUT_BINARY_DIRECTORY)/flash.jlink; [ "$$?" -eq 1 ]

$(OUTPUT_BINARY_DIRECTORY)/pinreset.jlink: $(BUILD_DIRECTORIES)
	echo "\n\
        r \n\
        h \n\
        w4 4001e504 2 \n\
        w4 40000544 1 \n\
        r \n\
        qc \n\
        " > $(OUTPUT_BINARY_DIRECTORY)/pinreset.jlink

pin-reset: $(OUTPUT_BINARY_DIRECTORY)/pinreset.jlink
	$(JLINK) -CommanderScript $(OUTPUT_BINARY_DIRECTORY)/pinreset.jlink; [ "$$?" -eq 1 ]

START_GDB_SERVER_SCRIPT=$(OUTPUT_BINARY_DIRECTORY)/start_gdbserver.sh
$(START_GDB_SERVER_SCRIPT): $(BUILD_DIRECTORIES)
	echo "JLinkGDBServer -if swd -device nrf51822 -LocalHostOnly" > $(START_GDB_SERVER_SCRIPT)
gdbserver: $(START_GDB_SERVER_SCRIPT)
	/bin/sh $(START_GDB_SERVER_SCRIPT)

GDB_COMMAND_SCRIPT=$(OUTPUT_BINARY_DIRECTORY)/gdbinit
$(GDB_COMMAND_SCRIPT): debug
	echo " \n\
        target remote localhost:2331    \n\
        mon flash breakpoints = 1       \n\
        monitor reset                   \n\
        file $(OUTPUT_BINARY_DIRECTORY)/$(OUTPUT_FILENAME).out  \n\
        load                            \n\
        b main                          \n\
        b app_error_handler             \n\
        monitor reset                   \n\
        continue" > $(GDB_COMMAND_SCRIPT)

#end of FLASHER selection
endif
endif
gdb: $(GDB_COMMAND_SCRIPT) debug
	$(GDB) -x $(GDB_COMMAND_SCRIPT)

serial:
	screen /dev/ttyUSB1cu.usbmodem1411 38400

