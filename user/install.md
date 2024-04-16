<!-- vim-markdown-toc GFM -->

* [Nmstate Installation Guide](#nmstate-installation-guide)
    * [RPM based](#rpm-based)
        * [Stable release](#stable-release)
        * [Developer Branch](#developer-branch)
    * [Install from source](#install-from-source)

<!-- vim-markdown-toc -->

# Nmstate Installation Guide

## RPM based

### Stable release

Nmstate is in Fedora 31+ and RHEL 8+, you may install it using below
commands.

```bash
sudo dnf install nmstate
```

### Developer Branch
We have copr repos which automatically build whenever a patch goes into
git base branch. Only for develop use.

 * Fedora 31+, CentOS/RHEL 8

```bash
sudo dnf copr enable nmstate/nmstate-git
sudo dnf install nmstate
```

## Install from source

```bash
# Use git or download tarball from https://github.com/nmstate/nmstate/releases
cd nmstate
PREFIX=/usr make install
```
