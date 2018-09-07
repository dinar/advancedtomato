include ../common.mak
include $(LINUXDIR)/.config

UTILS := wl

WLOBJS = wlu.o wlu_cmd.o wlu_iov.o wlu_linux.o bcmutils.o bcmwifi.o
ifneq ($(wildcard $(SRCBASE)/shared/miniopt.c),)
WLOBJS += miniopt.o
endif
ifneq ($(wildcard $(SRCBASE)/shared/bcm_app_utils.c),)
WLOBJS += bcm_app_utils.o
endif
ifneq ($(wildcard $(SRCBASE)/wl/exe/wlu_client_shared.c),)
WLOBJS += wlu_client_shared.o wlu_pipe.o wlu_pipe_linux.o
endif

all: $(UTILS)

clean:
	@rm -f *~ $(UTILS)
	@rm -f $(WLOBJS) $(EAPDOBJS) $(NASOBJS)

ifneq ($(wildcard $(SRCBASE)/wl/exe/*.c),)
vpath %.c $(SRCBASE)/wl/exe $(SRCBASE)/shared
CFLAGS += -I$(SRCBASE)/include -I$(SRCBASE)/wl/exe -Os $(EXTRACFLAGS)
ifeq ($(CONFIG_BCM_CTF),y)
CFLAGS += -DHNDCTF -DCTFPOOL
else
ifeq ($(CONFIG_BCM_CTF),m)
CFLAGS += -DHNDCTF -DCTFPOOL
endif
endif
else
ifneq ($(wildcard $(SRCBASE)/wl/$(PLATFORM)/wl),)
wl:
	@cp -f $(SRCBASE)/wl/$(PLATFORM)/wl wl
else
vpath %.o $(SRCBASE)/wl/$(PLATFORM)
wl: wl_exe.o
	$(CC) $(CCFLAGS) -DBCMWPA2 -o $@ $<

endif
endif

install: all
	install -d $(INSTALLDIR)/usr/sbin

	install -m 0755 wl $(INSTALLDIR)/usr/sbin/wl
	$(STRIP) $(INSTALLDIR)/usr/sbin/wl

