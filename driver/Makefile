
# Makefile for the kernel serial device drivers.
#
# Advantech eAutomation Division
#


UNAME := $(shell uname -r)
PWD := $(shell pwd)
ADVMOD := adv950


SERIAL_NAME= ttyAP
MAX_TTY= 64 
ADV_TTY_MAJOR= 30
 
SERIAL_FLAGS= -DSERIAL_NAME=\"$(SERIAL_NAME)\" \
              -DADV_TTY_MAJOR=$(ADV_TTY_MAJOR) \
              -DADV_TTY_CUMAJOR=$(ADV_TTY_CUMAJOR)       
	      

SERIAL_FLAGS:= $(SERIAL_FLAGS)

ifneq ($(KERNELRELEASE),)
EXTRA_CFLAGS  = $(SERIAL_FLAGS)
obj-m := adv950.o
adv950-objs := 8250.o 8250_pci.o
else
	KDIR := /lib/modules/$(shell uname -r)/build/
	PWD  := $(shell pwd)
all:	
	$(MAKE) -w -C $(KDIR) SUBDIRS=$(PWD) modules
endif


install:
	@echo "Intalling the advantech serial driver..."
	@echo "Please wait..."
	$(shell ! [ -d /lib/modules/$(shell uname -r)/kernel/drivers/adv950 ] && mkdir /lib/modules/$(shell uname -r)/kernel/drivers/adv950)
	$(shell [ -f adv950.ko ]  && cp ./adv950.ko /lib/modules/$(shell uname -r)/kernel/drivers/adv950/ && depmod && modprobe adv950)
	@echo "Done"

node:
	@echo "Creating serial device nodes $(SERIAL_NAME) ..."
	@chmod 777 ../advmknod
	@../advmknod $(SERIAL_NAME) \
	 $$(awk '$$2=="$(SERIAL_NAME)" {print $$1}' /proc/devices) $(MAX_TTY)
	@echo "Done"

uninstall:
	$(shell if grep $(ADVMOD) /proc/modules > /dev/null ; then \
	 rmmod $(ADVMOD) ; fi)

test:
	@cd ../getconfig; make; ./getconfig $(SERIAL_NAME) $(MAX_TTY)

clean:
	@rm -f *.o
	@rm -f *.ko
	@rm -f *.mod.c modules.order  Module.symvers Module.markers
	@rm -f .*.cmd
	@rm -rf .tmp_versions
	@rm -rf *~
	@cd ../getconfig; make clean
