# Active Directory Lab

This lab is made of two virtual machines:
- **Domain controller** running on Windows Server 2019
- **Member server** with an Octopus Deploy server and a Microsoft SQL server

The lab setup is automated using vagrant and ansible automation tools, and the OctopusDSC configuration library.

After setup, you should have an AD-connected Octopus Deploy instance with the latest public version, configured to only be authenticated against the Active Directory server. `admin@octopusdeploy.local` will be the administrator of the Octopus Server, and you should be able to add `alice` and `bob` from the same domain as users, if you like. Their passwords are set in `/group_vars/all`.

If you're using Virtualbox, you should be able to reach the Server instance on `localhost:8080` from your *host* as the ports are forwarded from the guest out to your local host. You will need to use form-based auth when accessing the instance from outside the VMs.

### Requirements

So far the lab has only been tested on a macOS machine, but it should work as well on linux. It has not been tested on a Windows host.

For the setup to work properly you need to install:
- **vagrant** from their official site [vagrant](https://www.vagrantup.com/). The version you can install through your favourite package manager (apt, yum, ...) is probably not the latest one.
- **ansible** following the extensive guide on their website [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).
- [**VirtualBox**](https://www.virtualbox.org/wiki/Downloads) to host the VMs. 

Vagrant will be needed to provision the virtual machines and ansible to automate their configuration.

> NOTE: From a 'cold start', you're going to download over 6GB of images here, mostly the Windows Server VM image. This might take some time.

### Setup

The default domain will be octopusadlab.local, on the subnet 192.168.56.1/24 and each machine has only been allocated with 2048MB of memory. If you want to change some of these settings some small modifications are required inside the configuration files.

> NOTE: On macOS Big Sur, you will need to `export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES` before running `ansible-playbook` to work around a [bug in Python on macOS](https://github.com/ansible/ansible/issues/49207).

To have the lab up and running the two commands you need to run are:
- `vagrant up`
- `ansible-playbook -i hosts labsetup.yml`

NOTE: There's a chance you'll see something like this during `vagrant up` doing initial setup of the boxes:
```
win_server: (10,8):UserId:
    win_server: At line:1 char:1
    win_server: + $username = 'vagrant'
    win_server: + ~~~~~~~~~~~~~~~~~~~~~
    win_server:     + CategoryInfo          : OperationStopped: (:) [Write-Error], ArgumentException
    win_server:     + FullyQualifiedErrorId : System.ArgumentException
```
This doesn't seem to impact ansible's ability to connect to the VMs later on.

If you run into some problems while running the main playbook, you can also the independent playbooks:
- `ansible-playbook -i hosts domain_controller.yml`
- `ansible-playbook -i hosts member_server.yml`

### Thanks to

This repo is based on the work of [jckhmr](https://github.com/jckhmr/adlab), [kkolk](https://github.com/kkolk/mssql), and [alebov](https://github.com/alebov/AD-lab).