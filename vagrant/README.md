<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

# Installing Vagrant/Packer on Ubuntu/Debian

- [Installing Vagrant/Packer on Ubuntu/Debian](#installing-vagrantpacker-on-ubuntudebian)
  - [Add the HashiCorp GPG key](#add-the-hashicorp-gpg-key)
  - [Add the official HashiCorp Linux repository](#add-the-official-hashicorp-linux-repository)
  - [Update & Install](#update--install)
  - [Add public box in vagrant](#add-public-box-in-vagrant)
    - [Using Public Boxes](#using-public-boxes)
      - [Adding a bento box to Vagrant](#adding-a-bento-box-to-vagrant)
    - [Using a bento box in a Vagrantfile](#using-a-bento-box-in-a-vagrantfile)
  - [Building Boxes](#building-boxes)
    - [Clone bento Project](#clone-bento-project)
    - [To build an Ubuntu 18.04 box for only the VirtualBox provider](#to-build-an-ubuntu-1804-box-for-only-the-virtualbox-provider)

<!-- /code_chunk_output -->

## Add the HashiCorp GPG key

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
```

## Add the official HashiCorp Linux repository

```bash
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
```

## Update & Install

```bash
sudo apt-get update
sudo apt-get install vagrant
sudo apt-get install packer
```

## Add public box in vagrant

### Using Public Boxes

#### Adding a bento box to Vagrant

```bash
vagrant box add --provider virtualbox bento/ubuntu-22.04
vagrant box add --provider virtualbox bento/debian-12
```

### Using a bento box in a Vagrantfile

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
end
```

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "bento/debian-12"
end
```

## Building Boxes

**Requirements:** install packer, vagrant and virtualbox

### Clone bento Project

```bash
git clone https://github.com/chef/bento.git
```

### To build an Ubuntu 18.04 box for only the VirtualBox provider

```bash
cd packer_templates/ubuntu
packer build -only=virtualbox-iso ubuntu-22.04-amd64.json
```
