# K64 Firmware
This repository serves as the working master of the real time flight computer
firmware for the entire project.

## Contribution Style
In order to contribute to this repository:
If you are making major changes that you will own, ie: new feature, driver:
 - Fork this repository.
 - Make your changes there, in any branching strategy that you like
 - Create a pull request from the specific branch in your repository with only
   the desired changes for a single pull request in it onto the branch in this
   repository to which those features should be added.

If you are making minor changes or are collaborating with others in this org:
 - Clone this repository.
 - Create a new branch starting from the branch you will work off of
   (usually, master) with a descriptive name to put your changes on.
 - Make your changes, test them, and commit everything to your branch.
 - Push the branch to this repository.
 - Create a pull request from that new branch to the branch that you started
   work from (again, usually master)

## Building the Firmware
1. Run `python init.py`.
2. Run `meson <build dir> --cross-file k64_files/k64_cross_properties.cfg`.
3. Run `ninja -C <build dir>`.

## Flashing the Target
1. Run `scripts/flash.sh`.

## Debugging the Target
1. From your terminal, run `scripts/gdb.sh build/main.elf`.
2. From another pty, run `telnet localhost 4444`.
3. In the telnet shell, type `reset init` before doing anything in GDB.

When debugging and flashing, an OpenOCD server is started that runs in the
background.  The scripts here create a PID file in case you need to kill it
manually, which is located at the project root with the name `openocd.pid`.
Normally, this should not be necessary.  Running
`scripts/openocd_server.sh stop` or typing `shutdown` in the telnet console
should be enough to end your OpenOCD session gracefully.

When debugging the target, a new ROM may be flashed to the device, but GDB will
not be aware of the change, nor the reset.  By default, the flash script causes
the target to be running after a flash, meaning that `reset init` must be issued
in the telnet console in order to reset and halt the target.  Note that GDB will
still not be aware of the new debugging symbols, so it needs to be restarted or
the ELF file reloaded for correct source-level debugging.
