TARGETIMG=disk.img
MOUNT_POINT=$(HOME)/tmp/mnt

$(TARGETIMG):
	qemu-img create -f raw $@ 200M
	mkfs.fat -n 'MIKAN OS' -s 2 -f 2 -R 32 -F 32 $@
	mkdir -p $(MOUNT_POINT)
	hdiutil attach -mountpoint $(MOUNT_POINT) $@
	mkdir -p $(MOUNT_POINT)/EFI/BOOT
	cp BOOTX64.EFI $(MOUNT_POINT)/EFI/BOOT/BOOTX64.EFI
	hdiutil detach $(MOUNT_POINT)

run:
	qemu-system-x86_64 \
		-drive if=pflash,file=$(HOME)/repo/os/qemu_images/mikan/mikanos-build/devenv/OVMF_CODE.fd \
		-drive if=pflash,file=$(HOME)/repo/os/qemu_images/mikan/mikanos-build/devenv/OVMF_VARS.fd \
		-hda $(TARGETIMG)

clean:
	rm -rf $(TARGETIMG)
