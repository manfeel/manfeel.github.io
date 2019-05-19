---
layout: post
title: HC6361 (ar9331) jtag config
---

# 重要说明
reset_config trst_and_srst `combined`
必须设置为combined模式！否则reset后，无法halt！

# ar9331.cfg
```
# Atheros AR9331 MIPS 24Kc SoC.
# tested on AP121 reference board (TP-LINK TL-MR3020)
#
# this settings are taken from source of u-boot for this board
# (for PLL) file: u-boot/board/ar7240/common/lowlevel_init.S
# (for DDR2) file: u-boot/cpu/mips/ar7240/hornet_ddr_init.S 
# with file: u-boot/include/configs/ap121.h
#
# source: https://wikidevi.com/wiki/TP-LINK_TL-MR3020
#
#daemon configuration
telnet_port 4444
gdb_port 3333
#interface
interface jlink
#jtag_speed 0
adapter_khz 500
jtag_nsrst_delay 100
jtag_ntrst_delay 100
gdb_breakpoint_override hard
reset_config trst_and_srst combined ;# or use only "reset_config none"
#reset_config trst_only separate
set CHIPNAME ar9331
jtag newtap $CHIPNAME cpu -irlen 5 -ircapture 0x1 -irmask 0x1f -expected-id 1
set TARGETNAME $CHIPNAME.cpu
target create $TARGETNAME mips_m4k -endian big -chain-position $TARGETNAME
#$TARGETNAME configure -event reset-init {}
proc init_soc { } {
    #pll initialization
    mww 0xb8050008 0x00018004
    mww 0xb8050004 0x00000352
    mww 0xb8050000 0x40818000
    mww 0xb8050010 0x001003e8
    mww 0xb8050000 0x00818000
    mww 0xb8050008 0x00008000
    sleep 10
    # Setup DDR1 config and flash mapping
    mww 0xb8000000 0x7fbc8cd0
    mww 0xb8000004 0x9dd0e6a8
    mww 0xb8000010 0x8
    mww 0xb800000c 0x2
    mww 0xb8000010 0x2
    mww 0xb8000008 0x133
    mww 0xb8000010 0x1
    mww 0xb8000010 0x8
    mww 0xb8000010 0x4
    mww 0xb8000010 0x4
    mww 0xb8000008 0x33
    mww 0xb8000010 0x1
    mww 0xb8000014 0x4186
    mww 0xb800001c 0x8
    mww 0xb8000020 0x9
    mww 0xb8000018 0xff
    #UART
    #mww 0xb8020004 0x4388
    #mww 0xb8020008 0xc2000
    #GPIO
    #mww 0xb8040028 0x48002
}
proc run_flash { } {
    reset
    halt
    resume 0xbfc00400
}
proc run_uboot { } {
    reset
    load_uboot
    resume 0x80100000    
}
proc load_uboot { } {
    halt
    init_soc
    sleep 1
    load_image /Volumes/MacData/works/u-boot_mod/bin/u-boot_mod__alfa-network_ap121f__20190518__git_master-7a540a78__RAM-LOAD-ONLY.bin 0x80100000
    #load_image /Users/manfeel/Downloads/u-boot.img 0x800FFFC0
    #resume 0x81000000
    reg pc 0x80100000
    step
} 
# setup working area somewhere in RAM
$TARGETNAME configure -work-area-phys 0xa0600000 -work-area-size 0x20000
# serial SPI capable flash
# flash bank <driver> <base> <size> <chip_width> <bus_width>
set FLASHNAME $CHIPNAME.flash
flash bank $FLASHNAME ath79 0xbf000000 0x01000000 0 0 $TARGETNAME
```
