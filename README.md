# PYSTLINK

Python tool for manipulating with STM32 MCUs using **ST-Link** in-system programmer and debugger.

## Goal

Goal of this project is to bring more flexible support for different MCUs, very simple command line interface, easier integration into Makefile for direct flashing or uploading program into SRAM and many more, simplest way to add support for new MCUs. Also any suggestions are welcome.

## Features

- running on **Linux**, **Mac OS/X** (also on OS/X 10.11+) and **Windows**
- simple command line interface
- detect MCU
- dump registers and memory
- write registers
- download memory to binary file
- upload binary or SREC file into memory
- FLASH binary or SREC file to all **STM32F**
- basic runtime control: reset, halt, step, run
- support **ST-Link/V2** and **ST-Link/V2-1**

### Planed features

- support for new unlisted MCUs (STM32F46x, STM32F47x, ..)
- FLASH support for other MCU types (STM32L)
- FLASH information block (system memory, option bytes and OTP area)
- connecting under RESET
- stop Watchdog in debug mode to prevent device restart
- support for more ST-Link devices connected at once
- other file formats (SREC, HEX, ELF, ...)
- pip installer
- proxy to GDB
- and maybe GUI
- support for ST-Link/V1 is NOT planed, use ST-Link/V2 instead

## Install

### Requirements

- **Python v3.x** (tested with python-3.4)
- [**pyusbs**](https://github.com/walac/pyusb)
- [**libusb**](http://libusbx.org) or any other libusb driver
  - for Windows copy libusb-1.0.dll into Windows/System32 directory

### pystlink

- [Download](https://github.com/pavelrevak/pystlink/archive/master.zip) and unpack or `git clone https://github.com/pavelrevak/pystlink.git`
- Connect ST-LINK/V2, with [**latest firmware**](http://www.st.com/web/en/catalog/tools/PF258194)
- Run `./pystlink.py --help` (or `python3 pystlink.py ...` - depend on python installation)

## Help
```
usage: pystlink [-h] [-q | -i | -v | -d] [-V] [-c CPU] [-n]
                [action [action ...]]

pystlink v0.0.0 (ST-LinkV2)
(c)2015 by pavel.revak@gmail.com
https://github.com/pavelrevak/pystlink

optional arguments:
  -h, --help         show this help message and exit
  -V, --version      show program's version number and exit
  -c CPU, --cpu CPU  set expected CPU type [eg: STM32F051, STM32L4]
  -n, --norun        do not run core when program end (if core was halted)

set verbosity level:
  -q, --quiet
  -i, --info         default
  -v, --verbose
  -d, --debug

actions:
  action             actions will be processed sequentially

list of available actions:
  dump:core              print all core registers (halt core)
  dump:{reg}             print core register (halt core)
  dump:{addr}:{size}     print content of memory
  dump:sram[:{size}]     print content of SRAM memory
  dump:flash[:{size}]    print content of FLASH memory
  dump:{addr}            print content of 32 bit memory register
  dump16:{addr}          print content of 16 bit memory register
  dump8:{addr}           print content of 8 bit memory register

  write:{reg}:{data}     write register (halt core)
  write:{addr}:{data}    write 32 bit memory register

  read:{addr}:{size}:{file}      read memory with size into file
  read:sram[:{size}]:{file}      read SRAM into file
  read:flash[:{size}]:{file}     read FLASH into file

  fill:{addr}:{size}:{pattern}   fill memory with a pattern
  fill:sram[:{size}]:{pattern}   fill SRAM memory with a pattern

  upload:{file.srec}     upload SREC file into memory
  upload:{addr}:{file}   upload binary file into memory
  upload:sram:{file}     upload binary file into SRAM memory

  flash:erase            complete erase FLASH memory aka mass erase
  flash[:erase][:verify]:{file.srec}     erase + flash SREC file + verify
  flash[:erase][:verify][:{addr}]:{file} erase + flash binary file + verify

  reset                  reset core
  reset:halt             reset and halt core
  halt                   halt core
  step                   step core
  run                    run core

  sleep:{seconds}        sleep (float) - insert delay between commands

  (numerical values can be in different formats, like: 42, 0x2a, 0o52, 0b101010)

examples:
  pystlink.py --help
  pystlink.py -v --cpu STM32F051R8
  pystlink.py -q --cpu STM32F03 dump:flash dump:sram
  pystlink.py dump:0x08000000:256
  pystlink.py write:0x48000018:0x00000100 dump:0x48000014
  pystlink.py read:sram:256:aaa.bin read:flash:bbb.bin
  pystlink.py -n reset:halt write:pc:0x20000010 dump:pc core:step dump:all
  pystlink.py flash:erase:verify:app.bin
  pystlink.py flash:erase flash:verify:0x08010000:boot.bin```

## Supported MCUs

Currently almost all **ST32F** and **ST32L** [MCU](http://www.st.com/web/en/catalog/mmc/FM141/SC1169).

FLASHing support is for all **STM32F**.

**Not all MCUs are tested**. Please report any problems to [Issues tracker](https://github.com/pavelrevak/pystlink/issues).

In WiKi is some basic info about STM32 naming: [STM32 coding matrix](https://github.com/pavelrevak/pystlink/wiki/STM32-coding-matrix)

## Legal

Code is under [MIT license](https://github.com/pavelrevak/pystlink/blob/master/LICENSE).

In general, this program is allowed to use in commercial and without any limitations, but if you make some changes or updates then will be nice to share it.

Support is only by [Issues tracker](https://github.com/pavelrevak/pystlink/issues)

PYSTLINK is inspired by [OpenOCD](http://openocd.org/), [STLINK](https://github.com/texane/stlink) and lot of info is from sniffed USB communication with [original ST-LINK](http://www.st.com/web/en/catalog/tools/PF258168) program.

## TAGS
ST-Link/V2, stlink, SWD, Python, ARM, CortexM, STM32, debug, FLASH, USB
