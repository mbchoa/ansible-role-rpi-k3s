Ansible Role: rpi-k3s
=========

An Ansible role that provisions a set of Raspberry Pis as a k3s cluster.

TODOs
------
This is still a work in progress. The following tasks still need to be implemented.

- [x] Install k3s master service on master node ([a050964](https://github.com/mbchoa/ansible-role-rpi-k3s/commit/a050964bb931d32c56c3c55ced21ab276342b85b#diff-2444ad0870f91f17ca6c2a5e96b26823R21-R28))
- [x] Install k3s agent service on child nodes ([a050964](https://github.com/mbchoa/ansible-role-rpi-k3s/commit/a050964bb931d32c56c3c55ced21ab276342b85b#diff-2444ad0870f91f17ca6c2a5e96b26823R41-R50))
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

Note: Keep in mind that once the role has been executed, all Raspberry Pis will effectively be locked down and inaccessible via SSH except for the machine that ran the playbook. This is done as a security measure per recommendation from the official raspberrypi.org [documentation](https://www.raspberrypi.org/documentation/configuration/security.md).

If this is not the desired behavior, I recommend reverting the changes made to the `/etc/ssh/sshd_config` file. In the future, I'm hoping this can be an opt-in feature to give users the freedom to choose the level of security of their Raspberry Pis.

#### playbook.yml
    - hosts: cluster
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

MIT

Author Information
------------------

This role was created in 2020 by [Michael-Bryant Choa](https://www.github.com/mbchoa).
