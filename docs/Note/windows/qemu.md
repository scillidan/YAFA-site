---
tags: [reference#, cache#]
---

## CMD

```sh
qemu-system-x86_64 ^
	-M q35 ^
	-m 4096 -smp 4 -cpu qemu64 ^
	-drive file=D:\\_qemu\\Bliss14.qcow2,if=virtio ^
	-cdrom D:\\_qemu\\Bliss-v14.10-x86_64-OFFICIAL-foss-20230704.iso ^
	-usb ^
	-device virtio-tablet ^
	-device virtio-keyboard ^
	-device qemu-xhci,id=xhci ^
	-machine vmport=off ^
	-device virtio-vga-gl -display sdl,gl=on ^
	-net nic,model=virtio-net-pci -net user,hostfwd=tcp::4444-:5555
rem -enable-kvm ^
rem -bios /usr/share/ovmf/x64/OVMF.fd ^
```

## Reference

- [Install with Qemu Script](https://github.com/BlissRoms-x86/docs/blob/master/install-bliss-os/install-in-a-virtual-machine/install-in-qemu.md)