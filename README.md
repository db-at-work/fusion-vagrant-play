# VMware Fusion Pro + Hasicorp Vagrant on Apple

## Why this repo

Notes: for starting multiple VMs using vagrant to launch vmware fusion on apple silicon.

## Why this solution

Because: Although virtualbox finally supports apple silicon with the [7.1.x series](https://www.virtualbox.org/wiki/Downloads) of releases in late 2024, Vagrant 2.4.1 has a virtualbox version check that does not support higher than 7.0.x series. So we suffer the version leap-frog of dysfunction, or use vmware.

# VMware Fusion Pro

This is now [available](https://blogs.vmware.com/teamfusion/2024/05/fusion-pro-now-available-free-for-personal-use.html) for non-commercial use w/o a license.

Go here for the download

* https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware+Fusion

You'll need a login with email and eventually you'll get to the version and "I agree to Terms and Conditions" checkbox to enable download of the universal binary. Presently this is the latest:

    VMware-Fusion-13.6.1-24319021_universal.dmg
    md5sum: 13304cbc30a0849a34cf1f7ea4065978

# Hasicorp Vagrant

## If using brew then

This might be easiest for arch and upgrade handling.

### install brew

Use this 

* https://brew.sh/

to check that this install command hasn't changed:

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

### install vagrant

    brew tap hashicorp/tap
    brew install hashicorp/tap/hashicorp-vagrant

The vagrant-vmware-utility install brew instructions look the same as those 
above.

## Else packages

Use these

* https://developer.hashicorp.com/vagrant/install
* https://developer.hashicorp.com/vagrant/install/vmware

to check the binary versions that 

### arch AMD64

    curl -kOLJ https://releases.hashicorp.com/vagrant/2.4.1/vagrant_2.4.1_darwin_amd64.dmg
    curl -kOLJ https://releases.hashicorp.com/vagrant-vmware-utility/1.0.23/vagrant-vmware-utility_1.0.23_darwin_amd64.dmg

### arch ARM

    curl -kOLJ https://releases.hashicorp.com/vagrant/2.4.1/vagrant_2.4.1_darwin_arm64.dmg
    curl -kOLJ https://releases.hashicorp.com/vagrant-vmware-utility/1.0.23/vagrant-vmware-utility_1.0.23_darwin_arm64.dmg

# Vagrant Boxes

Again, at the time of this note generation, ubuntu 24.04 was the latest well supported box, from the bento project builds.

If you can't find a box built by the distro, Chef project bento boxes are the next best thing (and sometimes even better for functional completeness).

## Ubuntu

arch: amd64 arm64

supports: vmware_desktop parallels qemu libvirt virtualbox

* https://portal.cloud.hashicorp.com/vagrant/discover/bento/ubuntu-24.04


## openSUSE

This is here only as reference, as it does not seem to work properly. The `vagrant up` command will complete, but `vagrant ssh control` fails to connect.

### arch amd64

arch: arm64 amd64 unknown

supports: vmware_desktop qemu virtualbox libvirt parallels

* https://portal.cloud.hashicorp.com/vagrant/discover/bento/opensuse-leap-15

## storage

vagrant boxes are stored in

    ~/.vagrant.d/boxes

Consider cleaning that space up from time to time to remove cruft.

# Vagrant Commands

## launch

Download the box if not present and launch the VMs

    vagrant up

to launch the 4 vms

## access

Specify the host to connect to

    vagrant ssh control

to ssh to the 'control' vm

## halt

To stop the VMs - like shutdown - use

    vagrant halt

## destroy

To remove the cow and configured VM for rebuilding from scratch after updating the underlying box version or any other reason

    vagrant destroy -f
