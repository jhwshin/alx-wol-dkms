# alx-wol-dkms

## About

This repo is a patch to the  [__Qualcomm Atheros ALX__ (ethernet network) driver](https://github.com/archlinux/linux/tree/master/drivers/net/ethernet/atheros/alx) in the Linux Kernel that re-enables the wake-on-lan (WoL) feature.

Written to work with __Arch Linux__ have yet to test in other distros.

Last tested and confirmed to be working on:

- __Arch Linux__ - `6.11.7-arch1-1`
- __Qualcomm Atheros Killer E220x Gigabit Ethernet Controller__


<details>
<summary>BEFORE</summary>

```yaml
$ ethtool enp6s0

Settings for enp6s0:
    Supported ports: [ TP ]
    Supported link modes:   10baseT/Half 10baseT/Full
                            100baseT/Half 100baseT/Full
                            1000baseT/Full
    Supported pause frame use: Symmetric Receive-only
    Supports auto-negotiation: Yes
    Supported FEC modes: Not reported
    Advertised link modes:  10baseT/Half 10baseT/Full
                            100baseT/Half 100baseT/Full
                            1000baseT/Full
    Advertised pause frame use: Symmetric
    Advertised auto-negotiation: Yes
    Advertised FEC modes: Not reported
    Speed: 1000Mb/s
    Duplex: Full
    Auto-negotiation: on
    Port: Twisted Pair
    PHYAD: 0
    Transceiver: internal
    MDI-X: Unknown
        Current message level: 0x000060e4 (24804)
                               link ifup rx_err tx_err hw wol
    Link detected: yes
```
</details>


<details>
<summary>AFTER</summary>

```yaml
$ ethtool enp6s0

Settings for enp6s0:
    Supported ports: [ TP ]
    Supported link modes:   10baseT/Half 10baseT/Full
                            100baseT/Half 100baseT/Full
                            1000baseT/Full
    Supported pause frame use: Symmetric Receive-only
    Supports auto-negotiation: Yes
    Supported FEC modes: Not reported
    Advertised link modes:  10baseT/Half 10baseT/Full
                            100baseT/Half 100baseT/Full
                            1000baseT/Full
    Advertised pause frame use: Symmetric
    Advertised auto-negotiation: Yes
    Advertised FEC modes: Not reported
    Speed: 1000Mb/s
    Duplex: Full
    Auto-negotiation: on
    Port: Twisted Pair
    PHYAD: 0
    Transceiver: internal
    MDI-X: Unknown
    Supports Wake-on: pg
    Wake-on: pg
        Current message level: 0x000060e4 (24804)
                               link ifup rx_err tx_err hw wol
    Link detected: yes

# p = physical activity
# g = magic packet
```
</details>

## Details

Logged in the kernel bug report tracker - [[BUG 61651]](https://bugzilla.kernel.org/show_bug.cgi?id=61651)

Investigation shows WoL support for ALX was removed back in [[4/7/2013]](https://github.com/archlinux/linux/commit/bc2bebe8de8ed4ba6482c9cc370b0dd72ffe8cd2) due to a bug that caused immediate resume after suspending. The issue was never solved but the problem only occured in some machines.

Forked from repo [[AndiWeiss/alx-wol]](https://github.com/AndiWeiss/alx-wol) which only patched up to kernel version `6.3`.

Using their latest commit from [[11/1/2024]](https://github.com/AndiWeiss/alx-wol/commit/6f4408bebc18a47a6fee9479eee3cc9c99b2d0e1) patched against the latest alx kernel driver from [[22/5/2024]](https://github.com/archlinux/linux/commit/f0bae243b2bcf2b160ae547463bf542762beef8f)

`PKGBUILD` from [alx-wol-dkms (AUR)](https://aur.archlinux.org/packages/alx-wol-dkms)


## Installation

1. Clone the repo
```bash
git clone https://github.com/jhwshin/alx-wol-dkms
```

2. Make and install
```bash
makepkg -si
```

You may also need to install `linux-headers`, `linux-zen-headers` or `linux-lts-headers` depending on the kernel your running.

## Enabling WoL

1. Install package:

```bash
pacman -S ethtool
```

2. Enable WoL - however this is transient and will not make it persistent on reboot:

```bash
ethtool -s enp6s0 wol g
```

3. To make it persistent using `Network Manager`:

```bash
# find your connection
$ nmcli c

# check wol capabilities
$ nmcli c show "Wired connection 1" | grep '802-3-ethernet.wake-on-lan'

# enable wol using magic packets
$ nmcli c modify "Wired connection 1" 802-3-ethernet.wake-on-lan magic

# verify
$ nmcli c show "Wired connection 1" | grep '802-3-ethernet.wake-on-lan'
```

If you are using `TLP` it may disable WoL:

`/etc/tlp.conf`
```
WOL_DISABLE=N
```

## DIY

There is a possiblity that future patches to the kernel alx drivers could break this patch.

If that happens and you would like to patch it yourself:

1. Clone Linux Kernel repo:

```bash
git clone https://github.com/archlinux/linux
```

2. Make changes to files in `drivers/net/ethernet/atheros/alx` by cross checking with `alx-wol.patch`

3. diff changes in git

```bash
git diff > alx-wol.patch
```

4. Edit `PKGBUILD`

5. Update checksum

```bash
updpkgsums
```

6. Create `.SRCINFO` (for AUR)

```bash
makepkg --printsrcinfo > .SRCINFO
```

## Disclaimer

I simply added the patch from `AndiWeiss` to alx kernel drivers.
I am still new to patching drivers.

__!!  USE AT YOUR OWN RISK  !!__

The author doesn't take any responsibility for any kind of malfunction or data loss on your system.
