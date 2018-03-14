# $ZEL: Makefile,v 1.12 2016/03/16 20:29:53 wuestner Exp $
#
# We're just setting the stage here and
# let the Kernel Makefile do the real work.
#

KERNVER := $(shell uname -r)
#KERNVER := 4.0.0-rc3
XVER    := X${KERNVER}

# accepted kernel versions
V24 := X2.4
V26 := X2.6
V3 := X3.
V4 := X4.

.PHONY: driver

driver:
	make -C /lib/modules/$(KERNVER)/build SUBDIRS=$(shell pwd) modules

install:
	make -C /lib/modules/$(KERNVER)/build SUBDIRS=$(shell pwd) modules_install

clean:
	-rm *.o *.ko .*.o.cmd .*.ko.cmd *.mod.c Module.symvers modules.order
	-rm Module.markers
	-rm -r .tmp_versions

MODULENAME := sis1100

ifndef src
    src:=$(shell pwd)
endif

SOURCES:=$(notdir $(wildcard $(src)/*.c))
sis1100-objs := $(patsubst %.c, %.o, $(filter-out %netbsd.c, $(SOURCES)))

ifneq (,$(or $(findstring ${V26},${XVER}),$(findstring ${V3},${XVER}),$(findstring ${V4},${XVER})))

    # kernel version 2.6 or later
    $(info kernel version ${KERNVER})
    EXTRA_CFLAGS += -DMODULENAME=$(MODULENAME)
    obj-m += sis1100.o

else ifneq (,$(findstring ${V24},${XVER}))

    # kernel version 2.4.*; does probably not work anymore
    $(warning old kernel version ${KERNVER})
modules: sis1100.o
sis1100.o: $(sis1100-objs)
	$(LD) -r -o $@ $^

else 
    $(error unsupported kernel version ${KERNVER})
endif
