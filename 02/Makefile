SRCROOT=$(HOME)/repo/os/qemu_images/mikan/02/MikanLoaderPkg
SRCS=$(SRCROOT)/Main.c
MIKANDEV=$(HOME)/repo/os/qemu_images/mikan/mikanos-build
TARGETDIR=$(WORKSPACE)/Build/MikanLoaderX64
TARGETEFI=$(TARGETDIR)/DEBUG_CLANGPDB/X64/MikanLoaderPkg/Loader/DEBUG/Loader.efi
TARGETIMG=disk.img

$(TARGETIMG): $(TARGETEFI)
	$(MIKANDEV)/devenv/make_image.sh $(TARGETIMG) mnt $(TARGETEFI)

$(TARGETEFI): $(SRCS)
	if test -d $(WORKSPACE)/MikanLoaderPkg; then \
		rm -rf $(WORKSPACE)/MikanLoaderPkg; \
	fi
	ln -s $(SRCROOT) $(WORKSPACE)/MikanLoaderPkg
	build


run: $(TARGET)
	$(MIKANDEV)/devenv/run_image.sh $(TARGETIMG)

mount: $(TARGETIMG)
	$(MIKANDEV)/devenv/mount_image.sh $(TARGETIMG) mnt

umount:
	hdiutil eject mnt

clean:
	rm -rf $(TARGETDIR) $(TARGETIMG)
