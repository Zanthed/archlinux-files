- [grub](#grub)

# [grub](grub)
Lots of things here related to security.

- Added `extra_latent_entropy` \([patch by PaX Team](https://patchwork.kernel.org/project/linux-kbuild/patch/20160524001736.135ae3cdc101ecec3232a493@gmail.com/)\) to get early entropy from the first 4GB of RAM while the runtime memory allocator is being initialized.
- (might be redundant from using PREEMPT_RT patched kernel) Added `threadirqs` to force threading of all interrupt handlers.
- Added `slab_nomerge` to disable merging of slabs with a similar size. Remediates the possibility of obscure slabs being merged together in an attempt to save memory to assist in kernel exploits. Very slim increased kernel memory usage if used.
- Added `nowatchdog` to disable kernel lockup detectors. Slight performance gain and one less module loaded. Do *not* use this in a server production environment.

(TODO: finish this)