<!-- vim-markdown-toc GFM -->

* [Nmstate Installation Guide](#nmstate-installation-guide)
    * [RPM based](#rpm-based)
        * [Stable release](#stable-release)
        * [Developer Branch](#developer-branch)
    * [PyPI/pip](#pypipip)
        * [Stable Release](#stable-release-1)
        * [Developer Branch](#developer-branch-1)
    * [setup.py](#setuppy)
        * [Stable Release](#stable-release-2)
        * [Developer Branch](#developer-branch-2)

<!-- vim-markdown-toc -->

# Nmstate Installation Guide

## RPM based

### Stable release

Nmstate is in Fedora and EPEL 7 testing, you may install it using below
commands.

 * Fedora 31+

```bash
sudo dnf install nmstate
```

 * RHEL 8 using copr repo

```bash
sudo dnf copr enable nmstate/nmstate-stable
sudo dnf install nmstate
```

### Developer Branch
We have copr repos which automatically build whenever a patch goes into
git master branch. Only for develop use.

 * Fedora 31+, CentOS/RHEL 8

```bash
sudo dnf copr enable nmstate/nmstate-git
sudo dnf install nmstate
```

## PyPI/pip

### Stable Release

```bash
pip --user --upgrade install nmstate
```

### Developer Branch

```bash
git clone https://github.com/nmstate/nmstate.git
cd nmstate
pip install --user --upgrade .
```

## setup.py

### Stable Release

```bash
# Download tarball and signature from:
# https://github.com/nmstate/nmstate/releases/
gpg2 --recv-keys F7910D93CA83D77348595C0E899014C0463C12BB
gpg2 --verify ./nmstate-*.tar.gz.asc nmstate-*.tar.gz
tar xf nmstate-*.tar.gz
cd nmstate-*
python setup.py build
python setup.py install
```

### Developer Branch

```bash
git clone https://github.com/nmstate/nmstate.git
cd nmstate
python setup.py build
python setup.py install
```
