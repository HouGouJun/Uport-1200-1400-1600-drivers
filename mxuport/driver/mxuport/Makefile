#-------------------------------------------------------------------------------
#   Makefile for MOXA UPort series drvier
#-------------------------------------------------------------------------------
KVER	 := $(shell uname -r)
KDISVER  := $(shell uname -a)
KDIR	 := /lib/modules/$(KVER)/build
PWD	 := $(shell pwd)
TARGET	 := mxuport


ifeq "$(findstring Fedora, $(shell grep Fedora /etc/issue ))" "Fedora"
MXPARM := -DFC6
endif

all:	module

install: $(TARGET).o
	@mkdir -p /lib/modules/$(KVER)/kernel/drivers/usb/serial
	@cp -f ./$(TARGET).ko /lib/modules/$(KVER)/kernel/drivers/usb/serial
	@mkdir -p /lib/modules/$(KVER)/misc/
	@cp -f ./$(TARGET).ko /lib/modules/$(KVER)/misc/
	@depmod -a

ifneq ($(KERNELRELEASE),)
obj-m := $(TARGET).o
$(TARGET)-objs := mx-uport.o
else
module:
	$(MAKE) EXTRA_CFLAGS=$(MXPARM) -C $(KDIR) SUBDIRS=$(PWD) modules
endif

remove:
	@rm -f /lib/modules/$(KVER)/kernel/drivers/usb/serial/$(TARGET).ko
	@rm -f /lib/modules/$(KVER)/misc/$(TARGET).ko
	@depmod -a

clean: 
	@rm -f compile-msg 
	@rm -f *.o
	@rm -f .*.o.cmd
	@rm -f *.symvers
	@rm -f *.order
	@rm -rf ./.tmp_versions
	@rm -f $(TARGET).mod*
	@rm -f .$(TARGET)*
	@rm -f $(TARGET).ko

