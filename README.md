
INSTALL UBUNTU 16.04 SERVER EDITION IN UNATTENDED MODE
=======================================================================

1.a Extract the CD iso file to local directory "/cdimg-path"

1.b Modify isolinux.cfg, prevent language selection before the installation.

	vi /cdimg-path/isolinux/isolinux.cfg
	timeout 0 ==> timeout 10

1.c Modify txt.cfg, add "auto=true" to append option.
    
	vi /cdimg-path/isolinux/txt.cfg

	label install
	menu label ^Install Ubuntu Server
	kernel /install/vmlinuz
	append  file=/cdrom/preseed/ubuntu-server.seed auto=true vga=788 initrd=/install/initrd.gz quiet ---
    
1.d Using my preseed file substitude the one in cd for ubuntu server installation.

	cp 16.04.3-server-unattended.seed /cdimg-path/preseed/ubuntu-server.seed

1.e Remake your cd with genisoimage or mkisofs for new CD iso file.

	IMAGE=ubuntu-zhouyoujoin.iso
	BUILD=/cdimg-path
	mkisofs -r -V "Custom Ubuntu CD" \
		-cache-inodes \
		-J -l -b isolinux/isolinux.bin \
		-c isolinux/boot.cat -no-emul-boot \
		-boot-load-size 4 -boot-info-table \
		-o $IMAGE $BUILD
