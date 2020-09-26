# mkinitcpio-remote-cryptroot

> `systemd-networkd` + `tinysshd` + `systemd-tty-ask-password-agent`

<p align="center">
  <img src="https://www.archlinux.org/static/logos/archlinux-logo-dark-90dpi.ebdee92a15b3.png" style="max-width: 100%;" alt="SRB2Kart">
</p>

\[**Arch Linux**\] If you want to be able to [boot a fully LUKS-encrypted system remotely](https://wiki.archlinux.org/index.php/Dm-crypt/Specialties#Remote_unlocking_of_the_root_(or_other)_partition), you will need a way to enter a passphrase for the root partition/volume at startup. This can be achieved by running a set of `mkinitcpio` hooks that configure a network interface, ssh access, and a password agent in initramfs.

```
$ ssh root@<remote host>
🔐 Please enter passphrase for disk cryptroot: (press TAB for no echo)
```

Works as of `systemd 245 (245.6-7-arch)`

## Example

Example `/etc/mkinitcpio.conf`:

```ini
...
HOOKS=(base autodetect modconf block filesystems keyboard fsck systemd sd-encrypt network tinyssh password-agent)
...
```

## Hooks

### systemd (owned by the `systemd` package)

See: [https://wiki.archlinux.org/index.php/Mkinitcpio#Common_hooks](https://wiki.archlinux.org/index.php/Mkinitcpio#Common_hooks)

This hook triggers a systemd based init, which does not run any runtime hooks but uses systemd units instead.

### sd-encrypt (owned by the `cryptsetup` package)

See: [https://wiki.archlinux.org/index.php/Mkinitcpio#Common_hooks](https://wiki.archlinux.org/index.php/Mkinitcpio#Common_hooks)

This hook allows for an encrypted root device with systemd initramfs.

**Recommend:** That you set a root [device timeout kernel parameter](https://wiki.archlinux.org/index.php/dm-crypt/System_configuration#Timeout):

```
rootflags=x-systemd.device-timeout=0
```

### network

This hook sets up the `systemd-networkd.service` systemd service and copies over any `*.initramfs` files in `/etc/systemd/network`.

Example:

```ini
# /etc/systemd/network/wired.network.initramfs

[Match]
Name=*

[Network]
DHCP=yes
```

### tinyssh

This hook sets up the `initrd-tinyssh.service` systemd service, converts the openssh ed25519 host key to binary format for TinySSH, and copies over the authorized keys file at `/etc/tinyssh/root_key`.

Example:

```ini
# /etc/tinyssh/root_key

ssh-ed25519 <base64-encoded key> <comment>
```

### password-agent

This hook adds the command `systemd-tty-ask-password-agent --query` to `/root/.profile` (for `sh` invocation).

## License

This project is licensed under the [MIT License](./LICENSE).
