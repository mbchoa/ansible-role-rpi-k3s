Ansible Role: rpi-k3s
=========

An Ansible role that provisions a set of Raspberry Pis as a k3s cluster.

TODOs
------
This is still a work in progress. The following tasks still need to be implemented.

- [ ] Install k3s master service on master node
- [ ] Install k3s agent service on child nodes
- [ ] Cleanup task to remove k3s services from master and child nodes

Requirements
------------

* Raspberry Pis should have freshly installed Raspbian OS
* Ideally, the RPis should have static IP addresses assigned to them

Dependencies
------------

None.

Example Playbook
----------------

First time setup, you'll need to run the playbook with the `-k` flag to enter the Raspberry Pi passsword (raspberry).

#### playbook.yml
    - hosts: k3s_cluster
      roles:
         - { role: mbchoa.rpi-k3s }

#### inventory
    [master]
    192.168.0.1

    [nodes]
    192.168.0.2
    192.168.0.3
    192.168.0.4

    [cluster:children]
    master
    nodes

    [cluster:vars]
    ansible_user=pi

License
-------

MIT / BSD

Author Information
------------------

This role was created in 2020 by [Michael-Bryant Choa](https://www.github.com/mbchoa).
