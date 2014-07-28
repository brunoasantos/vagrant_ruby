# Vagrant box for Ruby development

## Prerequisites

* Download the latest [VirtualBox](https://www.virtualbox.org/wiki/Downloads) for your platform
* Download the latest [Vagrant](http://downloads.vagrantup.com/) for your platform

## Instructions

### 1. Install VirtualBox

See installation instructions [here](https://www.virtualbox.org/manual/ch02.html)

### 2. Install Vagrant

See installation instructions [here](http://vagrantup.com/v1/docs/getting-started/index.html)

### 3. Install VirtualBox Guest Additions Vagrant plugin

This plugin will automatically install the host's VirtualBox Guest Additions on the guest system.

Open your terminal and run:

`vagrant plugin install vagrant-vbguest`

### 4. Install Librarian-Chef Vagrant plugin

This plugin let's us automatically run chef when we fire up our machine.

Open your terminal and run:

`vagrant plugin install vagrant-librarian-chef`

### 5. Build the virtual machine

Open your terminal and run:

`vagrant up`

It should take quite some time to finish. Seriously, go grab a cup of coffee or something.

Here's a rundown of what it's doing:

* Booting the vm
* Upgrading VirtualBox Guest Additions
* Rebooting the vm
* Updating apt and the caches
* Installing:
	* mysql with dev libraries
	* build-essential
	* zsh
	* oh-my-zsh
	* rvm (user installation)
	* ruby (latest version of 2.1.2, 1.9.3 and 1.8.7)
	* imagemagick with dev libraries (libmagickwand-dev)
	* vim
* configuring the root mysql user with no password

### 6. Remove Chef configuration (optional)

Every time you start your VM, Chef will verify if everything is installed.

This will obviously add up on your VM start up time.

To avoid this, just open up your `Vagrantfile` and remove everything under `config.vm.provision :chef_solo do |chef|`

### 7. Package the VM (optional)

Once everything is setup, you could package the VM in a box, upload it somewhere (dropbox maybe), and use it as a base for other VMs.

First you need to stop the VM with `vagrant halt` if it's running

Then, run `vagrant package -o BOX_NAME.box` to generate the box.

In order to use your box, you need to add it to Vagrant like this:

`vagrant box add BOX_NAME BOX_NAME.box`

After that you can create a simple `Vagrantfile` like the one below:

```
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.box = 'BOX_NAME'
	config.vm.network :forwarded_port, guest: 3000, host: 3000
end
```