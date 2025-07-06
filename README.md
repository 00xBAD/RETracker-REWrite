# [ RETracker :: REWrite ]

> 🚀 **Disclaimer A:** This project is an independent initiative. It is not affiliated with, endorsed by, or derived from Polyend or any of its official firmware, software, or documentation. This firmware is written from scratch and does **not** contain or use any copyrighted materials or intellectual property from Polyend.

> ⚖️ **Disclaimer B:** Actual reverse engineering findings, technical documentation, and any potentially sensitive or legally ambiguous materials are **NEVER** published online or in this repository. All such information is kept strictly on local storage and is not distributed, shared, or made public through this project.

---

## [ RE:VERSE ]

### [ HARDWARE :: TEARDOWN ]

- Disassemble the device to inspect internal components and board layout.
- Perform a full Bill of Materials (BOM) reverse-engineering to identify all chips, passives, and connectors.
- Map the PCB layout, tracing signal paths, power domains, and key interfaces.
- Analyze serial communication lines (UART, SPI, I2C) for debugging and firmware upload.
- Locate and probe JTAG/SWD or other debug entry points for hardware access.
- Document all findings for firmware development, hardware interfacing, initial hardware / software hacking.

### [ DATA :: EXTRACTION ]

- Acquire official firmware file (.ptf)
  - .ptf = Intel HEX format file containing firmware data as ASCII lines with address and checksum.
- Convert Intel HEX to raw binary (.bin)
  - Tools: srec_cat or Python IntelHex library.
  - Verify data integrity using CRC or SHA-256 checksum.

### [ BINARY :: ANALYSIS ]
- Load binary into disassembler
  - Disassembler = tool that converts machine code to assembly instructions.
  - Set architecture to ARMv7-M, little-endian.
  - Define base load address according to vector table offset.
- Define memory segments
  - .text = code segment where executable instructions reside.
  - .data = initialized data segment for global variables with preset values.
  - .bss = uninitialized data segment for global variables zeroed at startup.
  - .rodata = read-only data segment for constants and strings.
- Identify vector table
  - Vector table = table of pointers to interrupt and reset handlers at fixed low memory addresses.
  - Extract reset handler address for entry point reference.
- Extract reset handler and startup routines
  - Reset handler = first code executed after reset or power-on.
  - Startup code = routines that initialize stack pointer, copy .data, zero .bss, then call main().

### [ DATA :: OVERRIDE ]
- Identify standard library functions
  - Locate memcpy, memset, strcpy patterns by signature or string references.
- Map interrupt service routines (ISRs)
  - ISR = function executed in response to hardware event.
  - Match vector entries to disassembled functions.
- Identify memory-mapped I/O access
  - Peripheral registers = fixed addresses controlling hardware modules.
  - Search load/store instructions targeting known peripheral base addresses (GPIO, I2C, SPI, USB, DMA).
- Document DMA and buffer usage
  - DMA = Direct Memory Access, hardware engine moving data without CPU.
  - Identify DMA channel configuration and buffer descriptors for audio and USB.
- Map audio engine routines
  - PCM playback = Pulse-Code Modulation audio output.
  - Locate buffer refill and DAC/I2S transfer routines.

### [ DATA FORMATS :: DUMP ]
- Extract .pti instrument format structure
  - Header length and field offsets for envelope, loop points, sample length.
  - CRC32 = cyclic redundancy check for data integrity.
- Extract .mtp pattern format
  - Fixed-size record containing note events, effect commands, and timing data.
- Extract .mt project format
  - Sequence of pattern indices representing song arrangement.

### [ MAIN :: REWRITE :: PHASE ]
- Set up development framework
  - Choose language: Rust no_std or C with CMSIS and vendor HAL.
  - Initialize project with correct linker script matching memory map.
- Implement startup code
  - Copy .data, zero .bss, configure stack pointer.
  - Set up vector table and NVIC.
- Implement peripheral drivers
  - GPIO driver for pad matrix and encoders.
  - SPI driver for display interface.
  - SD card driver with FAT or custom FS parser.
  - USB driver for mass storage and debug interface.
- Implement core modules
  - Sequencer module: step counter, pattern pointer, tempo generator.
  - Audio module: sample buffer management, mixing, DAC output.
  - UI module: draw primitives on display, handle encoder input, menu navigation.
  - File module: parse .pti/.mtp/.mt formats into memory structures.
- Integrate modules and test
  - Unit test functions in simulator or dev board.
  - End-to-end test: load instrument, play pattern, display output, respond to input.

### [ BINARY :: PATCHING ]
- Add logging interface via UART or USB CDC
- Measure timing jitter and buffer underruns
- Optimize critical loops with assembly or DMA chaining
- Verify memory usage and adjust stack/heap sizes

### [ END USERS :: QOL ]
- Prepare build scripts and CI pipeline
- Write developer documentation for each module and data format
- Release source code under open-source license
- Provide flashable binaries and usage guide

---

## [ RE:WRITE ]

- Make an initial MVP firmware that can run on the Tracker OG, released as open-source firmware.
    - ⚠️ **Flash at your own risk!**

- Build a **fully independent, open-source firmware** for the Tracker OG
- Restore and enhance the user experience
- Prioritize **modularity**, **transparency**, and **user control**
- Respect the hardware by unleashing its true capabilities

This is not a clone of Polyend’s firmware. This is a fresh start.

---

## [ RE:SHIELD ]

This firmware is:
- Developed from scratch using clean-room practices
- Free from Polyend code, documentation, or proprietary internals
- Openly licensed for community contribution and use

I do not:
- Publish or reverse-engineer Polyend's firmware code or binaries
- Reproduce or document proprietary system internals
- Use Polyend trademarks in the firmware name or binaries

This project operates under fair-use and user rights to modify or replace software on legally owned hardware.

---

## [ RE:BIRTH ]

Contributors, testers, hardware explorers, and curious minds are welcome. If you love the Tracker OG but feel abandoned, this is your new home.

I'll do what Polyend didn’t: **support the device properly**.

Stay tuned for roadmap, architecture, and contribution guides.

---

## [ RE:VENGE ]

**Let’s bring the Tracker OG back to life. One byte at a time.**
