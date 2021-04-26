
CC = riscv64-unknown-elf-gcc

INCLUDES = -I . \
           -I /home/chaws/src/FreeRTOS/FreeRTOS/Source/include \
           -I /home/chaws/src/FreeRTOS/FreeRTOS/Source/portable/GCC/RISC-V \
           -I /home/chaws/src/FreeRTOS/FreeRTOS/Source/portable/GCC/RISC-V/chip_specific_extensions/RV32I_CLINT_no_extensions \
           -I freedom-e-sdk/env/freedom-e300-hifive1/ \
           -I freedom-e-sdk/env/ \
           -I freedom-e-sdk/include/

C_SOURCES = $(wildcard blinky_demo/*.c) \
          $(wildcard freedom-e-sdk/env/freedom-e300-hifive1/*.c) \
          $(wildcard freedom-e-sdk/libwrap/misc/*.c) \
          $(wildcard freedom-e-sdk/libwrap/stdlib/*.c) \
          $(wildcard freedom-e-sdk/libwrap/sys/*.c) \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/portable/MemMang/heap_4.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/portable/GCC/RISC-V/port.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/tasks.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/list.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/queue.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/timers.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/event_groups.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/stream_buffer.c \
          main.c

S_SOURCES = /home/chaws/src/FreeRTOS/FreeRTOS/Source/portable/GCC/RISC-V/portASM.S \
            freedom-e-sdk/env/start.S \
            freedom-e-sdk/env/entry.S

C_OBJECTS = $(C_SOURCES:.c=.o)

S_OBJECTS = $(S_SOURCES:.S=.o)

OBJECTS = $(C_OBJECTS) $(S_OBJECTS)

LDSCRIPT = freedom-e-sdk/env/freedom-e300-hifive1/flash.lds

ARCH_FLAGS = -march=rv32imac -mabi=ilp32 -mcmodel=medlow

CFLAGS = -DDONT_USE_PLIC \
         -DportasmHANDLE_INTERRUPT=handle_trap \
         -DDONT_USE_M_TIME \
         -O0 -g3 \
         -Wall -Wextra -Wno-unused-parameter \
         -ffunction-sections -fdata-sections -fmessage-length=0 -fno-builtin-printf \
         $(ARCH_FLAGS) \
         $(INCLUDES)

LINKER_FLAGS = -T $(LDSCRIPT) $(ARCH_FLAGS) --specs=nano.specs \
               -Xlinker --gc-sections -Wl,-Map,RTOSDemo.map \
               -Xlinker --defsym=__stack_size=350 \
               -Wl,--start-group -Wl,--end-group \
               -Wl,--wrap=__exit -Wl,--wrap=_exit  -Wl,--wrap=_close  -Wl,--wrap=_execve -Wl,--wrap=_fork \
               -Wl,--wrap=_free  -Wl,--wrap=_fstat -Wl,--wrap=_getpid -Wl,--wrap=_isatty -Wl,--wrap=_kill \
               -Wl,--wrap=_link  -Wl,--wrap=_lseek -Wl,--wrap=_malloc -Wl,--wrap=_open   -Wl,--wrap=_puts \
               -Wl,--wrap=_read  -Wl,--wrap=_sbrk  -Wl,--wrap=_stat   -Wl,--wrap=_times  -Wl,--wrap=_unlink \
               -Wl,--wrap=_wait  -Wl,--wrap=_write -Wl,--wrap=close   -Wl,--wrap=execve  -Wl,--wrap=fork \
               -Wl,--wrap=free   -Wl,--wrap=fstat  -Wl,--wrap=getpid  -Wl,--wrap=isatty  -Wl,--wrap=kill \
               -Wl,--wrap=link   -Wl,--wrap=lseek  -Wl,--wrap=malloc  -Wl,--wrap=open    -Wl,--wrap=puts \
               -Wl,--wrap=read   -Wl,--wrap=sbrk   -Wl,--wrap=stat    -Wl,--wrap=times   -Wl,--wrap=unlink \
               -Wl,--wrap=wait   -Wl,--wrap=write

RTOSDemo.elf : $(OBJECTS)
	$(CC) -nostartfiles $(LINKER_FLAGS) $(OBJECTS) -o RTOSDemo.elf

$(C_OBJECTS) : %.o : %.c
	$(CC) -c $(CFLAGS) $< -o $@

$(S_OBJECTS) : %.o : %.S
	$(CC) -c $(CFLAGS) $< -o $@

clean:
	rm -rf $(OBJECTS) RTOSDemo.elf

run: RTOSDemo.elf
	qemu-system-riscv32 RTOSDemo.elf -S -s -machine sifive_e
