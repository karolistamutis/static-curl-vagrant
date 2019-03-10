static-curl-vagrant
===

I recently had a need to compile 32bit and 64bit static curl binaries with TLS support and the process was error-prone and "hairy" to say the least so I've decided to automate the build as I'm sure someone else will benefit from this.

![curl binary screenshot](https://s3.eu-central-1.amazonaws.com/ktpublic/static-curl-vagrant/static-curl.png)

The resulting binaries are 3.7M in size and have no dependencies making them perfect for use in Docker or other environment with a relatively recent Linux kernel.

This is a [Vagrant](https://www.vagrantup.com/) and [Ansible](https://www.ansible.com/) configuration that

* Pulls a fresh ArchLinux box definition from Vagrant Cloud, depending on the architecture you need.
* Installs dev tools and clones git repos of OpenSSL, Zlib, curl based on tags/branches you specify in provisioning/playbook.yml.
* Builds using [musl](https://www.musl-libc.org/) libc as glibc and static linking don't go well together.
* Copies out curl binaries to `build` directory.

## Dependencies

Download and install [VirtualBox](https://www.virtualbox.org/), [Vagrant](https://www.vagrantup.com/downloads.html) and [Ansible](https://www.ansible.com/) for your platform.

First two are probably best to download from the sites directly, Ansible is usually available in your package management, i.e.

Linux 

```
apt-get install ansible
```

Mac

```
brew install ansible
```

## How to use


```
git clone https://github.com/karolistamutis/static-curl-vagrant

cd static-curl-vagrant

ARCH="32" vagrant up
```

or for 64bit

```
ARCH="64" vagrant up
```

or simply ```vagrant up```

After the build is complete binaries will be copied to your local build folder and you can ```vagrant destroy```

## Why Vagrant and not Docker

Vagrant is a VM management tool and spinning up a full isolated VM for builds allows flexibility in terms of using a different kernel or non host matching architecture.

The static binaries are perfect to be used within Docker though.

## Why ArchLinux as build base

Arch provides `musl` in the form of kernel-headers-musl in addition to full musl package, this allows to build OpenSSL without workarounds that would take away some security features, it's not the case in Ubuntu/Debian based distros.

## I don't trust the provided ArchLinux images

Great, neither did I so I made my own with [Packer](https://www.packer.io/) and uploaded to Vagrant Cloud.

I do provide the [Packer template](https://github.com/karolistamutis/packer-archlinux) in a separate repo so you can inspect and build your own. It's just a fresh base Arch tailored for Vagrant and python2 installed for Ansible provisioning later.

