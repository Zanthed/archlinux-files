- [/etc/](#etc)
  - [makepkg.conf](#makepkgconf)
    - [CFLAGS](#cflags)
    - [CXXFLAGS](#cxxflags)
    - [RUSTFLAGS](#rustflags)
    - [MAKEFLAGS](#makeflags)
    - [DEBUG_CFLAGS](#debug_cflags)
    - [DEBUG_CXXFLAGS](#debug_cxxflags)
    - [BUILDENV](#buildenv)
    - [BUILDDIR](#builddir)
    - [INTEGRITY_CHECK](#integrity_check)
    - [PACKAGER](#packager)
    - [GPGKEY](#gpgkey)
    - [COMPRESSLZ4](#compresslz4)
  - [/X11](#x11)
    - [Xwrapper.conf](#xwrapperconf)
    - [xorg.conf](#xorgconf)
  - [fstab](#fstab)
  - [pacman.conf](#pacmanconf)

# /etc/

## [makepkg.conf](makepkg.conf)

Important changes made:

### CFLAGS
- Added `-flto` for link-time optimization (heavy memory usage!)
- Set `-march` and `-mtune` to `native` to allow GCC to determine your architecture and its recommended optimizations. Avoid setting this to something other than `native` or `generic` because GCC might have better results with what it finds rather than what you specify. `generic` is for multi-architectures.
- Added `-ftree-loop-vectorize` to speed up loops. A feature of -O3.
- Added `-fstack-clash-protection` to identity and\/or correct stack overflows better.
- Added `-pie -fPIE` to obtain full [ASLR](https://wikipedia.org/wiki/Address_space_layout_randomization).
- Added `-ftrapv` to add traps (kill) on signed integer overflow.
- Added `-Wformat-security -Wall -Werror=format-security` to warn *and* error on insecure code as well as increase warning verbosity.
- Added `-mshstk` to enable built-in shadow stack functions from x86 Control-flow Enforcement Technology (CET).
- Added `-fstack-protector-all -Wstack-protector --param ssp-buffer-size=4` to enable enforced function buffer overflow checking on ALL functions rather than some. Warns if any functions won't be protected too.
- Added `-D_GLIBCXX_ASSERTIONS` to implement extra precondition error checking such as bounds checking in strings and null pointer checks.
- Added `-fdiagnostics-color=always` to enable colored output always :)


Note: I do not use `-O3` due to the possible inconsistencies it may have on certain code such as excessive loop unrolling causing slower code or compiler errors. Issues with -O3 have been exhibited on the Linux kernel. You're guaranteed better results with LTO and loop vectorization.

### CXXFLAGS
- Removed everything and added the `$CFLAGS` variable since everything there works for G++.

### RUSTFLAGS
- Added `opt-level=2` to do -O2 optimization.
- Added `target-cpu=native` to compile and optimize for your architecture.
- Added `lto=fat` to do full link-time optimization. If you experience excessively long linking times, try using `thin` which achives similar performance gains to fat while being quicker. See http://blog.llvm.org/2016/06/thinlto-scalable-and-incremental-lto.html

### MAKEFLAGS
- Added `-j$(getconf _NPROCESSORS_ONLN)` to use all threads available.
- Added `--quiet` to only print information, warnings, or errors.

### DEBUG_CFLAGS
- Added `-grecord-gcc-switches` to insert GCC compiler arguments in debugging information.
- Added `-fasynchronous-unwind-tables` to make backtraces more reliable.

### DEBUG_CXXFLAGS
- Removed everything and added the `$DEBUG_CFLAGS` variable.

### BUILDENV
- Uncommented `ccache` to utilize [ccache](https://wiki.archlinux.org/title/Ccache) to improve subsequent compiling times.
- Uncommented `sign` to sign all locally built AUR packages with my own GPG key.

### BUILDDIR
- Uncommented and set BUILDDIR to `/tmp/makepkg` to build in RAM. See my fstab file for increasing the size of it.

### INTEGRITY_CHECK
- Changed the disgusting, awful md5 to sha512 for clearly obvious reasons. (Why the hell is this default? Not even sha1???)

Update: As of pacman 6.0.0, Arch now switched to a very better hashing algorithm: CRC. While SHA512 is still superior to CRC, it's a lot better than MD5 and very good on embedded hardware which is where Arch strives in.

### PACKAGER
- Set packager to me of course.

### GPGKEY
- Set the GPG key to sign packages with my key.

### COMPRESSLZ4
- Added `--best` to lz4 compression.

## /X11

### [Xwrapper.conf](X11/Xwrapper.conf)
- Disable the requirement of root permissions on the Xorg server to allow rootless Xorg.

### [xorg.conf](X11/xorg.conf)
- Changes made to it to allow hybrid multihead\/dual monitors on Intel UHD 630 and EVGA GTX 550 Ti using nvidia-390xx-dkms.

## [fstab](fstab)
- Crazy BTRFS full disk single partition encryption setup. Bootloader on separate drive.
- Swap subvolume isn't actually a swap partition. It's just a subvolume that holds a swap file so the swap file isn't included in creating snapshots. LOL.
- Modified mount points such as `/dev/shm` and `/boot` with more secure mount options.

## [pacman.conf](pacman.conf)
- Set `ParallelDownloads` to 50 because I have gigabit internet.
- Uncommented `CheckSpace` to check for space before doing any operations.
- Uncommented `Color` to color output.
- Added easter egg `ILoveCandy` to increase the amount of colors in pacman output.
- Uncommented `UseSyslog` to log to syslog about any pacman operations.
- Set all signtature levels to `Required DatabaseOptional` to require package signing from any source.
