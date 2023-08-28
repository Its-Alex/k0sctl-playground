# k0sctl playground

This project aim to be a playground for [`k0sctl`](https://github.com/k0sproject/k0sctl),
some VMs are provisionned with [vagrant](https://www.vagrantup.com/) to be able
to be used with [`k0sctl`](https://github.com/k0sproject/k0sctl).

## Requirement

- [`rtx`](https://github.com/jdx/rtx)
- [`vagrant`](https://www.vagrantup.com/)

## Getting started

First you must boot VMs:

```bash
$ vagrant up
Bringing machine 'cluster-001' up with 'virtualbox' provider...
Bringing machine 'cluster-002' up with 'virtualbox' provider...
==> cluster-001: Importing base box 'bento/ubuntu-22.04'...
==> cluster-001: Matching MAC address for NAT networking...
==> cluster-001: Checking if box 'bento/ubuntu-22.04' version '202303.13.0' is up to date...
==> cluster-001: Setting the name of the VM: cluster-001
==> cluster-001: Clearing any previously set network interfaces...
==> cluster-001: Preparing network interfaces based on configuration...
    cluster-001: Adapter 1: nat
    cluster-001: Adapter 2: hostonly
==> cluster-001: Forwarding ports...
    cluster-001: 22 (guest) => 2222 (host) (adapter 1)
==> cluster-001: Running 'pre-boot' VM customizations...
==> cluster-001: Booting VM...
...
```

Then you can now apply [`k0sctl configuration`](./multiple_controller_worker_nodes.yaml):

```bash
$ k0sctl apply --config ./k0sctl/multiple_controller_worker_nodes.yaml

⠀⣿⣿⡇⠀⠀⢀⣴⣾⣿⠟⠁⢸⣿⣿⣿⣿⣿⣿⣿⡿⠛⠁⠀⢸⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠀█████████ █████████ ███
⠀⣿⣿⡇⣠⣶⣿⡿⠋⠀⠀⠀⢸⣿⡇⠀⠀⠀⣠⠀⠀⢀⣠⡆⢸⣿⣿⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀███          ███    ███
⠀⣿⣿⣿⣿⣟⠋⠀⠀⠀⠀⠀⢸⣿⡇⠀⢰⣾⣿⠀⠀⣿⣿⡇⢸⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠀███          ███    ███
⠀⣿⣿⡏⠻⣿⣷⣤⡀⠀⠀⠀⠸⠛⠁⠀⠸⠋⠁⠀⠀⣿⣿⡇⠈⠉⠉⠉⠉⠉⠉⠉⠉⢹⣿⣿⠀███          ███    ███
⠀⣿⣿⡇⠀⠀⠙⢿⣿⣦⣀⠀⠀⠀⣠⣶⣶⣶⣶⣶⣶⣿⣿⡇⢰⣶⣶⣶⣶⣶⣶⣶⣶⣾⣿⣿⠀█████████    ███    ██████████
k0sctl v0.15.5 Copyright 2023, k0sctl authors.
Anonymized telemetry of usage will be sent to the authors.
By continuing to use k0sctl you agree to these terms:
https://k0sproject.io/licenses/eula
INFO ==> Running phase: Connect to hosts
WARN 192.168.56.10:22: Ignored a SSH host key mismatch because StrictHostkeyChecking is set to 'no' in ssh config
INFO [ssh] 192.168.56.10:22: connected
INFO [ssh] 192.168.56.11:22: connected
...
INFO k0s cluster version v1.27.4+k0s.0 is now installed
INFO Tip: To access the cluster you can now fetch the admin kubeconfig using:
INFO      k0sctl kubeconfig
```

You now have two clusters on two differents nodes. You can go on VMs to check:

```bash
$ vagrant ssh cluster-001
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-67-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Aug 28 10:41:29 AM UTC 2023

  System load:  1.0556640625       Users logged in:              0
  Usage of /:   17.1% of 30.34GB   IPv4 address for eth0:        10.2.0.15
  Memory usage: 37%                IPv4 address for eth1:        192.168.56.10
  Swap usage:   0%                 IPv4 address for kube-bridge: 10.244.0.1
  Processes:    189

 * Introducing Expanded Security Maintenance for Applications.
   Receive updates to over 25,000 software packages with your
   Ubuntu Pro subscription. Free for personal use.

     https://ubuntu.com/pro


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
vagrant@cluster-001:~$ sudo su
root@cluster-001:/home/vagrant# k0s status
Version: v1.27.4+k0s.0
Process ID: 2946
Role: controller
Workloads: true
SingleNode: true
Kube-api probing successful: true
Kube-api probing last error:
```

You can check, add or edit `k0sctl configurations` in [`k0sctl`](./k0sctl/).

## How to change VMs

You can edit the [`Vagrantfile`](./Vagrantfile) to update number of VMs or their
configurations.

## Licence

[MIT](./LICENSE)
