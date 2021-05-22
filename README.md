# alx-wol-dkms

Qualcomm Atheros Killer E220x Drivers had Wake-on-Lan (WoL) feature removed from the Linux Kernel a while back due to buggy unintended wakes.

See bug report:
[https://bugzilla.kernel.org/show_bug.cgi?id=61651](https://bugzilla.kernel.org/show_bug.cgi?id=61651)

There were patches
* AUR
[https://aur.archlinux.org/packages/alx-wol-dkms](https://aur.archlinux.org/packages/alx-wol-dkms)

* Git
[https://github.com/AMV007/alx_dkms_installer](https://github.com/AMV007/alx_dkms_installer)

but the driver patches above were outdated with latest Linux Kernels.

Hence here are updated driver patches with fixes that enables WoL back again. They have been tested and confirmed to be working with `5.12.5-arch1`.

---

To install simply clone this repository and run `makepkg -si` inside the directory.

You may also need to install `linux-headers`

For future kernels (5.12.5+) modify kernel versions on `PKGBUILD` and checksums using `makepkg -g`.
