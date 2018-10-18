# VMware [Photon](https://github.com/vmware/photon) [Packer](http://packer.io) templates


## Requirements

The following software must be installed/present on your local machine before you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the VirtualBox box)
  - [Ansible](http://docs.ansible.com/intro_installation.html)
  - [OVFTOOL](https://code.vmware.com/tool/ovf/4.3.0) (v4+)
  - [VMware Fusion](https://my.vmware.com/web/vmware/info/slug/desktop_end_user_computing/vmware_fusion/10_0) (only tested with VMware Fusion 10)
  - [vagrant-vsphere](https://github.com/nsidc/vagrant-vsphere)
  - [vagrant-json](https://github.com/Bauer-Xcel-Media/vagrant-json-config)

## Build Photon artifacts

This packer template requires to variables to be set either via command line or separate JSON file:

1. `iso_file` - is the photon ISO for the build (can be local or remote).
1. `iso_sha1sum` - is the SHA1 sum of the ISO you want to build.
1. `product_version` - is the PhotonOS release version (will be added to the image filename)

Preset JSONs with the required parameters are provided in vars folder.

To kick off a full build using a preset:

```shell
packer build -var-file=vars/iso-2.0GA.json packer-photon.json
```

or manually:
```shell
packer build \
        -var 'iso_file=https://bintray.com/artifact/download/vmware/photon/iso/1.0TP2/x86_64/photon-minimal-1.0TP2.iso' \
        -var 'iso_sha1sum=a47567368458acd8c8ef5f1e3f793c5329063dac' \
        -var 'product_version=1.0TP2' \
        packer-photon.json
```

To build only a VMware workstation/fusion vagrant box:
```shell
packer build -only=vagrant-vmware_desktop -var-file=vars/iso-2.0GA.json packer-photon.json
```
or:
```shell
packer build \
       -only=vagrant-vmware_desktop \
       -var 'iso_file=https://bintray.com/artifact/download/vmware/photon/iso/1.0TP2/x86_64/photon-minimal-1.0TP2.iso' \
       -var 'iso_sha1sum=a47567368458acd8c8ef5f1e3f793c5329063dac' \
       -var 'product_version=1.0TP2' \
       packer-photon.json
```

### vSphere

Create a variables.json file with correct variables. See variables.json_example for needed variables. VMware Version is set to 13 = ESXi 6.5 and later

Make sure all the required software (listed above) is installed, then cd to the directory containing this README.md file, and run:

    $ packer build -var-file=vars/iso-2.0GA.json packer-photon.json -var-file=variables.json vsphere_photon.json

There are two build targets available:

1. `vagrant-vmware_desktop` - Generates the `vmware_desktop` compatible box found on Atlas as [`vmware/photon`](https://atlas.hashicorp.com/vmware/photon).
1. `vagrant-virtualbox` - Generates the `virtualbox` compatible box found on Atlas as [`vmware/photon`](https://atlas.hashicorp.com/vmware/photon).

`vsphere_photon.json` - creates a photon machine and uploads it to vCenter

## Legal

Copyright © 2015 VMware, Inc.  All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the “License”); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an “AS IS” BASIS, without warranties or
conditions of any kind, EITHER EXPRESS OR IMPLIED.  See the License for the
specific language governing permissions and limitations under the License.
