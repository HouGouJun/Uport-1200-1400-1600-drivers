#-------------------------------------------------------------------------------
#   Makefile for MOXA UPort series drvier
#-------------------------------------------------------------------------------
DRV_VER  := 1.2
REL_DATE := 07/13/2012
KVER	 := $(shell uname -r)
KDISVER  := $(shell uname -a)
KDIR	 := /lib/modules/$(KVER)/build
PWD	 := $(shell pwd)
TARGET	 := mxuport


all:	modules

install: modules
	@make -s -C mxuport install
	@make -s -C mxusbserial install
	@depmod -a
	@echo " "
	@echo "************************************************************************"
	@echo " MOXA UPort 1200/1400/1600 series driver ver $(DRV_VER) installed successfully."
	@echo "************************************************************************"

ifeq "$(findstring Fedora, $(shell grep Fedora /etc/issue ))" "Fedora"
MXPARM := -DFC6
endif

ifneq ($(KERNELRELEASE),)
obj-y := mxuport/ mxusbserial/
else

modules: envchk

	@$(MAKE) EXTRA_CFLAGS=$(MXPARM) -C $(KDIR) SUBDIRS=$(PWD) modules 2>> build.log \
	&& cp -f ./build.log ../build.log \
	&& exit 0 \
	|| echo "Build Fail! Please chech build.log for further information." \
	&& cp -f ./build.log ../build.log \
	&& exit 1

endif

envchk:
	@./mxconf | tee build.log

remove:
	@make -s -C mxuport remove
	@make -s -C mxusbserial remove
	@depmod -a
	@echo ""
	@echo "************************************************************************"
	@echo " MOXA UPort 1200/1400/1600 series driver ver $(DRV_VER) removed successfully."
	@echo "************************************************************************"


clean: clean_local clean_lib

clean_local:
	@echo "Removing driver files..."
	@make -s -C mxuport clean
	@make -s -C mxusbserial clean
	@rm -rf ./.tmp_versions
	@rm -f *.symvers
	@rm -f *.order
	@rm -f *.markers
	@rm -rf ../.tmp*

clean_lib:
	@rm -f /lib/modules/$(KVER)/kernel/drivers/usb/serial/$(TARGET_DRIVER).ko
	@rm -f ../*.log
	@rm -f ./*.log
	@echo ""
	@echo "************************************************************************"
	@echo " MOXA UPort 1200/1400/1600 series driver ver $(DRV_VER) removed successfully."
	@echo "************************************************************************"
