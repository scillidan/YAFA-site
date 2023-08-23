## QEMU

### CMD

```
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

### Reference

1. https://github.com/BlissRoms-x86/docs/blob/master/install-bliss-os/install-in-a-virtual-machine/install-in-qemu.mdho

## shawl

```
gsudo shawl add --name shawl-%1 -- %2 ^
	&& gsudo sc config shawl-%1 start= auto ^
	&& gsudo sc start shawl-%1
rem && gsudo sc create %1 binPath= "shawl.exe run -- %2"
```

