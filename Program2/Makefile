all: boot1

boot1: boot1.asm boot2
	nasm -l boot1.list -DENTRY=`./getaddr.sh main` boot1.asm

boot2: boot2.exe
	objcopy -S -O binary boot2.exe boot2

boot2.exe: boot2_c.o boot2_S.o
	ld -g -Ttext 0x10000 -e main -o boot2.exe boot2_S.o boot2_c.o

boot2_c.o: boot2.c
	gcc -g -c -o boot2_c.o boot2.c

boot2_S.o: boot2.S
	gcc -g -c -masm=intel -o boot2_S.o boot2.S

install:
	bximage -fd -size=1.44 -q a.img
	mformat a:
	dd if=boot1 of=a.img bs=1 count=512 conv=notrunc
	mcopy -o boot2 a:BOOT2

run:
	qemu -S -s -boot a -fda a.img

runall:
	qemu -boot a -fda a.img

clean:
	rm *.o *.exe boot2 boot1.list boot1 a.img

