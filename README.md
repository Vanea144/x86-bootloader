# x86-bootloader-vga

A 16-bit real-mode bootloader and VGA driver written in NASM Assembly.
Fits strictly within the 512-byte MBR limit (`0xAA55` signature).

## Technical Overview

### Memory & Execution
* **Origin**: Loaded at `0x7c00` by BIOS.
* **Mode**: Real Mode (16-bit). No OS, no GDT/IDT tables.

### VGA Driver
* **Video Mode**: Switches to Mode 13h (320x200, 256 colors) using BIOS interrupt `int 0x10`.
* **Rendering**: Direct Memory Access (DMA) to Video RAM at segment `0xA000`. Writes pixels directly to memory addresses rather than using BIOS plotting calls for performance.

### Input Handling
* **Interrupts**: Polling `int 0x16` (AH=0x00) for blocking keyboard input.
* **Logic**: Maps scancodes to memory offsets to update the pixel pointer `DI` in real-time.

## Build & Run

Requires `nasm` and `qemu`.

```bash
# Assemble raw binary
nasm -f bin boot.asm -o boot.bin

# Run in QEMU
qemu-system-i386 -drive format=raw,file=boot.bin
