
Run Your First Playbook
==============================

This document explains how to run your first Fortinet Console Ansible playbook.

Prepare host inventory
~~~~~~~~~~~~~~~~~~~~~~

in our case we create a file named ``hosts``:

::

  [fortigates]
  fortigate01 term_server=192.168.190.130 term_ssh_port="2922" term_user="InReach" term_password="access" dev_user="admin" dev_password="password"

  In order to get Fortinet Console module to work, you need two sets of login credentials:
  1) Playbook needs to login to remote console (terminal) server first, with specified the port mapping to Fortinet device, become command is optional
  2) Then through the remote console server, playbook needs to login to Fortinet device in order to execute commands


Write the playbook
~~~~~~~~~~~~~~~~~~

in the example: ``test.yml`` we are going to modify the fortigate
device’s hostname:

::

  - name: Fortinet Console Example Playbook - User Remote Console Server to Factory Reset FortiGate Firewall
    hosts: fortigate01
    collections:
      - fortinet.console
    tasks:
    - name: With remote console access, factory reset FGT
      fortigate_remote_console:
        rcs_ip: "{{ term_server }}"
        rcs_username: "{{ term_user }}"
        rcs_password: "{{ term_password }}"
        rcs_fgt_port: "{{ term_ssh_port }}"
        rcs_fgt_username: "{{ dev_user }}"
        rcs_fgt_password: "{{ dev_password }}"
        rcs_fgt_become: "{{ term_become|default(omit) }}"
        rcs_fgt_action: "factoryreset"
    register: fortigate_remote_console_result
    - debug:
      msg: "{{ fortigate_remote_console_result }}"


there are several options which might need you special care:

-  **collections** : The namespace must be ``fortinet.console``

Run the playbook
~~~~~~~~~~~~~~~~

::

  ansible-playbook -i hosts test.yml

you can also observe the verbose output by adding option at the tail: ``-vvv``.
