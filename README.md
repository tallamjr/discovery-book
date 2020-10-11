# Discovery Book: Rust

Discovering the world of microcontrollers through Rust! 🦀

This repository is to hold code used and in conjunction with working through material in the
_Discovery-Book_ for learning embedded rust.

#### Getting Started: Testing the Hardware

```bash
$ openocd -f interface/stlink-v2-1.cfg -f target/stm32f3x.cfg
Open On-Chip Debugger 0.10.0
Licensed under GNU GPL v2
For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>'.
adapter speed: 1000 kHz
adapter_nsrst_delay: 100
Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
none separate
Info : Unable to match requested speed 1000 kHz, using 950 kHz
Info : Unable to match requested speed 1000 kHz, using 950 kHz
Info : clock speed 950 kHz
Info : STLINK v2 JTAG v37 API v2 SWIM v26 VID 0x0483 PID 0x374B
Info : using stlink api v2
Info : Target voltage: 2.896991
Info : stm32f3x.cpu: hardware has 6 breakpoints, 4 watchpoints

```

> The microcontroller in the F3 has a Cortex-M4F processor in it. rustc knows how to cross compile to
> the Cortex-M architecture and provides 4 different targets that cover the different processor
> families within that architecture:

> - `thumbv6m-none-eabi`, for the Cortex-M0 and Cortex-M1 processors
> - `thumbv7m-none-eabi`, for the Cortex-M3 processor
> - `thumbv7em-none-eabi`, for the Cortex-M4 and Cortex-M7 processors
> - `thumbv7em-none-eabihf`, for the Cortex-M4F and Cortex-M7F processors

> Binary size is something we should always keep an eye on! How big is your solution? You can check
> that using the size command on the release binary:

```bash
$ # equivalent to size target/thumbv7em-none-eabihf/debug/led-roulette
$ cargo size --target thumbv7em-none-eabihf --bin led-roulette -- -A
led-roulette  :
section               size        addr
.vector_table          392   0x8000000
.text                16404   0x8000188
.rodata               2924   0x80041a0
.data                    0  0x20000000
.bss                     4  0x20000000
.debug_str          602185         0x0
.debug_abbrev        24134         0x0
.debug_info         553143         0x0
.debug_ranges       112744         0x0
.debug_macinfo          86         0x0
.debug_pubnames      56467         0x0
.debug_pubtypes      94866         0x0
.ARM.attributes         58         0x0
.debug_frame        174812         0x0
.debug_line         354866         0x0
.debug_loc             534         0x0
.comment                75         0x0
Total              1993694

$ cargo size --target thumbv7em-none-eabihf --bin led-roulette --release -- -A
led-roulette  :
section              size        addr
.vector_table         392   0x8000000
.text                1826   0x8000188
.rodata                84   0x80008ac
.data                   0  0x20000000
.bss                    4  0x20000000
.debug_str          23334         0x0
.debug_loc           6964         0x0
.debug_abbrev        1337         0x0
.debug_info         40582         0x0
.debug_ranges        2936         0x0
.debug_macinfo          1         0x0
.debug_pubnames      5470         0x0
.debug_pubtypes     10016         0x0
.ARM.attributes        58         0x0
.debug_frame          164         0x0
.debug_line          9081         0x0
.comment               18         0x0
Total              102267
```
> Know how to read this output? The text section contains the program instructions. It's around 2KB
> in my case. On the other hand, the data and bss sections contain variables statically allocated in
> RAM (static variables). A static variable is being used in aux5::init; that's why it shows 4 bytes
> of bss.

**Chapter 6: Hello World!**
