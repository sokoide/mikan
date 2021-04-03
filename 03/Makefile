SRCROOT=$(HOME)/repo/qemu_images/mikan/03
SRCS=$(SRCROOT)/MikanLoaderPkg/Main.c
KERNELSRC=$(SRCROOT)/kernel/main.cpp
KERNELO=main.o
KERNEL=kernel.elf
MIKANDEV=$(HOME)/repo/qemu_images/mikan/mikanos-build
TARGETDIR=$(WORKSPACE)/Build/MikanLoaderX64
TARGETEFI=$(TARGETDIR)/DEBUG_CLANGPDB/X64/MikanLoaderPkg/Loader/DEBUG/Loader.efi
TARGETIMG=disk.img

$(TARGETIMG): $(TARGETEFI) $(KERNEL)
	$(MIKANDEV)/devenv/make_image.sh $(TARGETIMG) mnt $(TARGETEFI) $(KERNEL)

$(TARGETEFI): $(SRCS) $(KERNEL)
	if test -d $(WORKSPACE)/MikanLoaderPkg; then \
		rm -rf $(WORKSPACE)/MikanLoaderPkg; \
		fi
	ln -s $(SRCROOT)/MikanLoaderPkg $(WORKSPACE)/MikanLoaderPkg
	build

$(KERNEL): $(KERNELSRC)
	clang++ -O2 -Wall -g --target=x86_64-elf -ffreestanding -mno-red-zone -fno-exceptions -fno-rtti -std=c++17 -c $<
	ld.lld --entry KernelMain -z norelro --image-base 0x100000 --static -o $(KERNEL) main.o

run: $(TARGET)
	$(MIKANDEV)/devenv/run_image.sh $(TARGETIMG)

mount: $(TARGETIMG)
	$(MIKANDEV)/devenv/mount_image.sh $(TARGETIMG) mnt

umount:
	hdiutil eject mnt

clean:
	rm -rf $(TARGETDIR) $(TARGETIMG) $(KERNEL) *.o
