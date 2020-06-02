Ansible Role: rpi-k3s
=========

An Ansible role that provisions a set of Raspberry Pis as a k3s cluster. The k3s cluster, by default, comes with the [Traefik Ingress Controller](https://rancher.com/docs/k3s/latest/en/networking/#traefik-ingress-controller) as well as a basic [service load balancer](https://rancher.com/docs/k3s/latest/en/networking/#service-load-balancer).

This role disables installation of the default ingress controller and load balancer in favor of using the [kubernetes/ingress-nginx](https://kubernetes.github.io/ingress-nginx/) ingress controller along with the [MetalLB](https://metallb.universe.tf/) load balancer.

Requirements
------------

* Clone repo to your `~/.ansible/roles` folder (the folder will need to be renamed `mbchoa.rpi-k3s`)
* Raspberry Pis should have freshly installed Raspbian OS
* Ideally, the RPis should have static IP addresses assigned to them

Dependencies
------------

None.

Example Playbook
----------------

First time setup, you'll need to run the playbook with the `-k` flag to enter the Raspberry Pi passsword (raspberry). You can utilize the playbooks contained in the [example](example) directory to setup your Raspberry Pi cluster as a starting point. The repository should be located in the `~/.ansible/roles` folder.

#### setup.yml
    - hosts: cluster
      become: true

      vars:
        # Update to use your own IP range
        - lb_address_pool: 192.168.0.240-192.168.0.250

      roles:
        - mbchoa.rpi-k3s

#### inventory
    [master]
    192.168.0.1

    [workers]
    192.168.0.2
    192.168.0.3
    192.168.0.4

    [cluster:children]
    master
    workers

    [cluster:vars]
    ansible_user=pi

Accessing Cluster
-----------------

To access the cluster from a remote machine, you will need to copy the kubeconfig file from the k3s master to your remote machine by running the following command:

```bash
scp pi@master-ip:~/.kube/config ~/.kube/config
```

This will configure `kubectl` to direct commands to the k3s master on the Raspberry Pi.

Securing Raspberry Pi
---------------------

The role includes a `secure.yml` task which is an opt-in task that can be included in the setup of the Raspberry Pis to disable password login access once your machine's public SSH key has been copied over to each Raspberry Pi.

This is taken from the official Raspberry Pi ["Securing your Raspberry Pi"](https://www.raspberrypi.org/documentation/configuration/security.md) article.

#### playbook.yml
    - hosts: cluster
      become: true

      vars:
        secure_raspberry_pis: true

      roles:
        - mbchoa.rpi-k3s

License
-------

MIT

Author Information
------------------

This role was created in 2020 by [Michael-Bryant Choa](https://www.github.com/mbchoa).
