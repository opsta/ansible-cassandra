Role Name
=========

opsta.cassandra

Requirements
------------

none

Role Variables
--------------

```yml
cassandra_search_config_path: "{{ playbook_dir }}/files/groups/{{ item }}/cassandra"
cassandra_search_config_paths: []

cassandra_host_config_file_path: "{{ cassandra_host_config_path | default(inventory_hostname) }}/cassandra.yaml.j2"
cassandra_config_file_path: /etc/cassandra/cassandra.yaml
cassandra_config_path: /etc/cassandra

```
example of inventory file
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

```yml
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
