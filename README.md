## fake-proxmox-subscription
A Debian package that disables the "No valid subscription" dialog on all
Proxmox products (in theory), regardless of their version.

Based on [Jamesits/fake-pve-subscription][1]'s work. There are a few key
differences between his repository and this repository:

1. The name `pve-fake-subscription` has been replaced with
`fake-proxmox-subscription`. This should have been done in the first place
since the package covers all 3 Proxmox products.
2. Added proper Debian packaging so that `dpkg-buildpackage` could be used
instead of `nFPM` nonsense.
3. Added GitHub Actions workflow to build and then publish `.deb` packages.

The 3rd point above is the most critical change here. This is because you
wouldn't download and install a .deb that you can't verify has not been
altered with, would you?

### Features
This package should work and patch:
- Proxmox VE (5.x or later - 3.x and 4.x requires some manual actions)
- Proxmox Mail Gateway (5.x or later)
- Proxmox Backup Server (1.x or later)

What this package does is:
1. **Non-Intrusive**: Performs 0 modifications to the system files.
2. **Future-Proof**: Requires no adjustments between system updates & major
upgrades.
3. **Hassle-Free**: Install (or uninstall) with ease.
4. **Debian-ized**: `fake-proxmox-license` is delivered as a proper Debian
package, fresh from GitHub CI/CD to provide transparency and automation.
5. **Fuck JavaScript**: No JavaScript is involved in the whole process, because
fuck JavaScript.

### Usage
#### Installation
Download the [latest `.deb`][2] file from the [releases][3] page and install it
with `apt` or `apt-get`:

```
# curl -s https://api.github.com/repos/all-solutions/fake-proxmox-subscription/releases/latest | grep "browser_download_url.*deb" | cut -d : -f 2,3 | tr -d \" | wget -i - -O fake-proxmox-subscription.deb
# apt install ./fake-proxmox-subscription.deb
```

### Uninstallation
To uninstall, run `apt` or `apt-get` with the `remove` flag and the package
name:

```
# apt remove fake-proxmox-subscription
```

### Build It Yourself
You can easily build the package yourself, assuming you have a *Debian*-based
system:

```
$ sudo apt-get update
$ sudo apt-get install -y --no-install-recommends build-essential debhelper dpkg-dev
$ git clone https://github.com/all-solutions/fake-proxmox-subscription
$ cd fake-proxmox-subscription/
$ dpkg-buildpackage -us -uc -b
$ ls -al ../fake-proxmox-subscription_*
```

While at it, you can view and validate the contents of your newly built `.deb`:

```
$ dpkg-deb --info ../fake-proxmox-subscription_*.deb
$ dpkg-deb --contents ./fake-proxmox-subscription_*.deb
```

Alternatively, if you want to use `podman` or `docker` to build the `.deb`, a
`Dockerfile` is available:

```
$ podman build -t fake-proxmox-subscription .
$ podman run -it fake-proxmox-subscription:latest /bin/bash -c "ls -al /opt/fake-proxmox-subscription/debian/artifacts/"
```

Afterwards, transfer the generated `.deb` from
`/opt/fake-proxmox-subscription/debian/artifacts/`.

The `Dockerfile` is basic and was primarily used for build testing,
validating packaging, etc. I've only kept it in case someone might be
interested in using it.

[1]: https://github.com/Jamesits/pve-fake-subscription
[2]: https://github.com/all-solutions/fake-proxmox-subscription/releases/latest
[3]: https://github.com/all-solutions/fake-proxmox-subscription/releases
[4]: https://github.com/Jamesits
