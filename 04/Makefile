SRCROOT=$(HOME)/repo/os/qemu_images/mikan/04
SRCS=$(SRCROOT)/MikanLoaderPkg/Main.c
KERNELSRC=$(SRCROOT)/kernel/main.cpp
KERNELO=main.o
KERNEL=kernel.elf
MIKANDEV=$(HOME)/repo/os/qemu_images/mikan/mikanos-build
TARGETDIR=$(WORKSPACE)/Build/MikanLoaderX64
TARGETEFI=$(TARGETDIR)/DEBUG_CLANGPDB/X64/MikanLoaderPkg/Loader/DEBUG/Loader.efi
TARGETIMG=disk.img

BASEDIR=/opt/homebrew
BASEVER=11
EDK2DIR=$(WORKSPACE)

CXX=clang++
CXXFLAGS=-O2 -Wall -g --target=x86_64-elf -ffreestanding -mno-red-zone -fno-exceptions -fno-rtti -std=c++17 \
-I$(WORKSPACE)/MikanLoaderPkg \
-I$(BASEDIR)/include/c++/v1 -I$(BASEDIR)/include/c++/$(BASEVER) -I$(BASEDIR)/include/c++/$(BASEVER)/aarch64-apple-darwin21 \
-I$(BASEDIR)/include -I$(BASEDIR)/include/freetype2 \
-I$(EDK2DIR)/MdePkg/Include -I$(EDK2DIR)/MdePkg/Include/X64 \
-nostdlibinc -D__ELF__ -D_LDBL_EQ_DBL -D_GNU_SOURCE -D_POSIX_TIMERS \
-DEFIAPI='__attribute__((ms_abi))'
LD=ld.lld
LDFLAGS=-L$(BASEDIR)/lib --entry KernelMain -z norelro --image-base 0x100000 --static -z separate-code

$(TARGETIMG): $(TARGETEFI) $(KERNEL)
	$(MIKANDEV)/devenv/make_image.sh $(TARGETIMG) mnt $(TARGETEFI) $(KERNEL)

$(TARGETEFI): $(SRCS)
	if test -d $(WORKSPACE)/MikanLoaderPkg; then \
		rm -rf $(WORKSPACE)/MikanLoaderPkg; \
		fi
	ln -s $(SRCROOT)/MikanLoaderPkg $(WORKSPACE)/MikanLoaderPkg
	build

$(KERNEL): $(KERNELSRC)
	$(CXX) $(CXXFLAGS) -c $<
	$(LD) $(LDFLAGS) -o $@ $(KERNELO)

run: $(TARGETIMG)
	$(MIKANDEV)/devenv/run_image.sh $(TARGETIMG)

mount: $(TARGETIMG)
	$(MIKANDEV)/devenv/mount_image.sh $(TARGETIMG) mnt

umount:
	hdiutil eject mnt

clean:
	rm -rf $(TARGETDIR) $(TARGETIMG) $(KERNEL) *.o
