Role Name
=========

opsta.cassandra

Requirements
------------

none

Role Variables
--------------

```yml
cassandra_cluster_name: MyCluster
cassandra_rpc_address: 0.0.0.0
cassandra_num_token: 256
cassandra_endpoint_snitch: SimpleSnitch
```
Example of inventory file
```ini
cassandra1 ansible_host=xx.xx.xx.xx ansible_user=ubuntu
cassandra2 ansible_host=yy.yy.yy.yy ansible_user=ubuntu
cassandra3 ansible_host=zz.zz.zz.zz ansible_user=ubuntu

[cassandra-seeds]
cassandra1

[cassandra-clients]
cassandra2
cassandra3

[cassandra-cluster:children]
cassandra-seeds
cassandra-clients
```

Dependencies
------------

none

Example Playbook
----------------

    If you had more than one seed. You need to run gather_facts all hosts
before start the following playbook. Example run install a new version of java
for cassandra and gather facts for all hosts.
```yml
---
- hosts: all
  become: true
  gather_facts: true
  roles:
    - opsta.java
  environment: "{{ proxy_env | default({}) }}"

- hosts: cassandra-seeds
  become: true
  gather_facts: true
  serial: 1
  roles:
    - opsta.cassandra
  environment: "{{ proxy_env | default({}) }}"

- hosts: cassandra-clients
  become: true
  gather_facts: true
  serial: 1
  roles:
    - opsta.cassandra
  environment: "{{ proxy_env | default({}) }}"
```

License
-------

MIT

Author Information
------------------

Anurak Jannawan (Opsta Thailand)
