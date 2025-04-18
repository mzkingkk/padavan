TOPDIR         = ${CURDIR}
CT_DIR        := $(TOPDIR)/toolchain
CT_PREFIX     := $(TOPDIR)/toolchain-mipsel
CT_TARGET     := mipsel-linux-musl
TOOLCHAIN_URL := https://github.com/TurBoTse/padavan/releases/download/toolchain/$(CT_TARGET).tar.xz

all: build

build:
	@echo "Building toolchain..."
	@(cd $(CT_DIR); \
		./bootstrap && \
		./configure --enable-local && \
		make && \
		./ct-ng $(CT_TARGET) && \
		./ct-ng build \
	)

clean:
	@if [ -d $(CT_DIR) ]; then \
		cd $(CT_DIR); \
		if [ -f ct-ng ]; then ./ct-ng distclean; fi; \
		if [ -f Makefile ]; then make distclean; fi; \
	fi
	@if [ -d $(CT_PREFIX) ]; then rm -rf $(CT_PREFIX); fi

download:
	@if [ ! -d $(CT_PREFIX) ]; then \
		echo "Downloading toolchain..."; \
		mkdir -p $(CT_PREFIX); \
		curl -fSsLo- $(TOOLCHAIN_URL) | tar Jx -C $(CT_PREFIX); \
	fi

ifeq ($(CT_TARGET),mipsel-linux-musl)
	@if [ ! -f $(CT_PREFIX)/$(CT_TARGET)/sysroot/usr/include/sys/queue.h ]; then \
		echo "Installing sys/queue.h..."; \
               cp -f $(TOPDIR)/queue.h $(TOPDIR)/toolchain-mipsel/mipsel-linux-musl/sysroot/usr/include/sys/; \
	fi
endif