
CC = arm-none-eabi-gcc

INCLUDES = -I . \
           -I /home/chaws/src/FreeRTOS/FreeRTOS/Source/include \
           -I /home/chaws/src/FreeRTOS/FreeRTOS/Source/portable/GCC/ARM_CM3 \
           -I Utilities/STM32_EVAL/STM32_Discovery \
           -I Libraries/CMSIS/CM3/CoreSupport \
           -I Libraries/CMSIS/CM3/DeviceSupport/ST/STM32F10x \
		   -I Libraries/STM32F10x_StdPeriph_Driver/inc

SOURCES = /home/chaws/src/FreeRTOS/FreeRTOS/Source/portable/MemMang/heap_1.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/portable/GCC/ARM_CM3/port.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/tasks.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/list.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/queue.c \
          /home/chaws/src/FreeRTOS/FreeRTOS/Source/timers.c \
		  $(wildcard Libraries/CMSIS/CM3/CoreSupport/*.c) \
		  $(wildcard Libraries/STM32F10x_StdPeriph_Driver/src/*.c) \
          $(wildcard Utilities/STM32_EVAL/STM32_Discovery/*.c) \
          stm32f10x_it.c system_stm32f10x.c main.c

OBJECTS = $(SOURCES:.c=.o)

LDSCRIPT = stm32_flash.ld

CRT0 = Libraries/CMSIS/CM3/DeviceSupport/ST/STM32F10x/startup/TrueSTUDIO/startup_stm32f10x_md_vl.s
# CRT0 = startup_stm32f10x_md_vl.s

DEVICE = STM32F10X_LD

CFLAGS = -Wall -mcpu=cortex-m3 -nostartfiles \
         $(INCLUDES) \
		 -D STM32F10X_LD \
         -D USE_STDPERIPH_DRIVER \
         -T $(LDSCRIPT)


LINKER_FLAGS=-Xlinker -odemo.elf

demo.elf : $(OBJECTS)
	$(CC) $(CFLAGS) $(OBJECTS) $(CRT0) $(LINKER_FLAGS)

$(OBJECTS) : %.o : %.c
	$(CC) -c $(CFLAGS) $< -o $@

clean:
	rm -rf $(OBJECTS)
