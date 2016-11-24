# nrf51822-gcc-build-scripts
nrf51822 firmware build script using cross arm gcc

Our recipe is almost same with nordic semi's tutorial([Development with GCC and Eclipse](https://devzone.nordicsemi.com/tutorials/7/)) except OpenOCD support

0. install GCC ARM Embedded toolchain:

    from: https://launchpad.net/gcc-arm-embedded

    If your host is MS Window, install [core utilities](http://gnuwin32.sourceforge.net/packages/coreutils.htm) and [GNU Make](http://gnuwin32.sourceforge.net/packages/make.htm) by GnuWin

1. download and extract nRF51-SDK 10.0.0 (with s110 softdevice) or nRF5-SDK 11.0.0

2. download firmware uploader/debugging program (nrfjprog/j-link or openocd) as to your debugger (J-Link or STLink-v2)

3. create your project

4. copy .cproject, .project, Makefile and linker script file (.ld) to your project directory

6. modify Makefile variables (OUTPUT_FILENAME, LINKER_SCRIPT, SOFTDEVICE_HEX_PATH, FLASHER, C_SOURCE_FILES, INC_PATHS, etc.) in order to suit your project

7. make

8. make erase_all

9. make flash_softdevice

10. make flash

11. (optional) install eclipse IDE with CDT and GCC ARM plug-ins.

12. (optional) import your project onto eclipse and build/upload/debug your application
