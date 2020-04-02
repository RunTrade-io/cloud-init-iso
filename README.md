Cloud Init ISO
==============

Many premade cloud images (e.g. Fedora Cloud, Ubuntu Cloud) use some form of 
cloud-init to set instance/user metadata, such as hostnames and SSH keys. This 
works well when used in a cloud infrastructure such as EC2 or OpenStack that can 
seed this data, but not so well when used for local VMs, or an out-of-the-box 
XenServer installation.

However, [cloud-init supports an CD/ISO datasource][1], which can be loaded 
whether running the machine locally in KVM, VirtualBox, or on a XenServer host. 

This repository and guide aims to make this task a lot easier by pre-seeding a 
template and script. It draws heavily on existing resources ([2][2], [3][3]).

## Usage

1. Clone this repository
2. Modify the `user-data` file to specify an auth token.  The file should be in the format:

    token <token>

Other options that can be added to the user-data include:

    ip4 <ipv4 address>
    netmask4 <subnet mask>
    gateway4 <default gateway>
    vlan <vlan>
    dns4_1 <dns server 1>
    dns4_2 <dns server 2>
    proxy <proxy>:<port>
    proxy_auth <username>:<password>

3. Verify that you have the dependency `genisoimage`. In RHEL/CentOS/Fedora, 
   `sudo yum install genisoimage`; in Ubuntu/Debian, `sudo apt-get install genisoimage`.
4. Build the ISO using `./build.sh`. You can either specify an output filename as the 
   first parameter (e.g. `./build.sh output-file.iso`), or you can let the script decide 
   on the filename. If you are working inside a git repository, the build script should 
   name your file after the branch and commit hash, such as 
   `frost-init-20141228.4d48bab28b6d8f9c43f7b6e36238ef6863b41e90.iso`.
5. If everything went well, attach the ISO file to your VM by methods 
   conventional to your virtualization hypervisor.
6. Boot the VM!
