- [/etc/](#etc)
  - [makepkg.conf](#makepkgconf)
    - [CFLAGS](#cflags)
    - [CXXFLAGS](#cxxflags)
    - [RUSTFLAGS](#rustflags)
    - [MAKEFLAGS](#makeflags)
    - [DEBUG_CFLAGS](#debug_cflags)
    - [DEBUG_CXXFLAGS](#debug_cxxflags)
    - [BUILDENV](#buildenv)
    - [INTEGRITY_CHECK](#integrity_check)
    - [PACKAGER](#packager)
    - [GPGKEY](#gpgkey)
  - [/X11](#x11)
    - [Xwrapper.conf](#xwrapperconf)
    - [xorg.conf](#xorgconf)
  - [fstab](#fstab)

# /etc/

## [makepkg.conf](makepkg.conf)

Important changes made:

### CFLAGS
- Added `-flto` for link-time optimization (heavy memory usage!)
- Set `-march` to `native` to allow GCC to determine your architecture and its recommended optimizations. Avoid setting this to something other than `native` or `generic` because GCC might have better results with what it finds rather than what you specify. `generic` is for multi-architectures.
- Added `-ftree-loop-vectorize` to speed up loops. A feature of -O3.
- Added `-fstack-clash-protection` to identity and\/or correct stack overflows better.

Note: I do not use `-O3` due to the possible inconsistencies it may have on certain code such as excessive loop unrolling causing slower code or compiler errors. Issues with -O3 have been exhibited on the Linux kernel. You're guaranteed better results with LTO 

### CXXFLAGS
- Removed everything and added the `$CFLAGS` variable since everything there works for G++.

### RUSTFLAGS
- Added `opt-level=2` to do -O2 optimization.
- Added `target-cpu=native` to compile and optimize for your architecture.
- Added `lto=thin` to do link-time optimization **on the crate you're compiling** instead of `fat` where it does LTO on all dependencies and the crate. This is to avoid possible issues other crates may have with LTO while still going for a performance boost.

### MAKEFLAGS
- Added `-j$(nproc)` to use all threads available.

### DEBUG_CFLAGS
- Added `-grecord-gcc-switches` to insert GCC compiler arguments in debugging information.
- Added `-fasynchronous-unwind-tables` to make backtrackes more reliable.

### DEBUG_CXXFLAGS
- Removed everything and added the `$DEBUG_CFLAGS` variable.

### BUILDENV
- Uncommented `ccache` to utilize [ccache](https://wiki.archlinux.org/title/Ccache) to improve subsequent compiling times.
- Uncommented `sign` to sign all locally built AUR packages with my own GPG key.

### INTEGRITY_CHECK
- Changed the disgusting, awful md5 to sha512 for clearly obvious reasons. (Why the hell is this default? Not even sha1???)

### PACKAGER
- Set packager to me of course.

### GPGKEY
- Set the GPG key to sign packages with to mine.

## /X11

### [Xwrapper.conf](X11/Xwrapper.conf)
- Disable the requirement of root permissions on the Xorg server to allow rootless Xorg.

### [xorg.conf](X11/xorg.conf)
- Changes made to it to allow hybrid multihead\/dual monitors on Intel UHD 630 and EVGA GTX 550 Ti using nvidia-390xx-dkms.

## [fstab](fstab)
- Crazy BTRFS full disk single partition encryption setup. Bootloader on separate drive.
- Swap partition isn't actually a swap partition. It's a partition that holds a swap file so the swap file isn't included in creating snapshots. LOL.
- Modified mount points such as `/dev/shm` and `/boot` with more secure mount options.