# buki
Simple POSIX shell UKI builder

This was created primarily to avoid systemd-ukify on my own systems, but also to be more portable.

This script supports both llvm-objcopy and binutils' objcopy to build the UKI.

# Dependencies
- llvm-objcopy or binutils objcopy
- any POSIX-capable shell
- A handful of basic utilities such as `id`, `uname`, `stat`, and `cat` if building with microcode.

# Usage

- Build a UKI:
``buki build -i </path/to/initramfs> -k </path/to/kernel> -o </path/to/esp/bootx64.efi> [-c 'loglevel=4 init_on_free=1'] [-s /usr/lib/systemd/boot/efi/linuxx64.efi.stub] [...]``

- Request help output:
``buki``, ``buki help``, any non ``buki build`` command.

# Options
  - `-c` - Specify kernel cmdline or path to file containing kernel parameters if begins with `.` or `/`
           If nothing is specified, buki will default to `/proc/cmdline`
           If passed a set of parameters directly, `/tmp/bukitmp/cmdline` will be created and
           will then be passed into objcopy.

  - `-i` - Specify path to initramfs image

  - `-k` - Specify path to kernel

  - `-m` - Specify path to microcode image
         If nothing is specified, UKI will be built without microcode
         If specified, `/tmp/bukitmp/buki-combined_initrd.img` will be created, and will contain
         the concatenated microcode and the initrd, and will then be passed into objcopy.

  - `-o` - Specify path to output the built UKI

  - `-r` - Specify path to os-release information
         If nothing is specified, buki will default to `/etc/os-release`

  - `-s` - Specify path to EFI stub file
         If nothing is specified, buki will default to `/usr/lib/systemd/boot/efi/march.efi.stub`

# Miscellaneous options
  - `-b` - Forces UKI to be built with binutils objcopy rather than llvm-objcopy
  - `-l` - Forces UKI to be built with llvm-objcopy rather than binutils
         If an objcopy variant is not explicitly set, the first one found on the system will be used.

  - `-v` - Verbose output when building a UKI

Miscellaneous options do not take an argument.

The order these options are set in does not matter.
