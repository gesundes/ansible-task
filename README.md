# How to use it?
You can apply configuration to hosts like this:
```bash
ansible-playbook -u ubuntu -i ./inventory --key-file ~/aws-test-user ./playbook.yaml
```
Let's describe CLI options:

- -u — username
- -i — path to the directory with the inventory
- --key-file — path to the private SSH key (400 permissions must be set)
- last option is the path to the playbook to run

Other useful options:

- --diff — to get a small file diffs in the execution log
- --check — to simulate a play execution, works great along with --diff

## How to provide a list of hosts to apply configuration?
Inventory file is in ./inventory directory. It is written in YAML.\
Some variables can be set right there. For example, configuration for drives/partitions encryption.

## What will happen to these hosts?
It described in the [./playbook.yaml](playbook.yaml) file. There one can find a list of hosts as well as list of roles that will be applied to that hosts.

In current case the *basic* role will be applied. Comments regarding the role one can find in the [roles/basic/README.md](roles/basic/README.md) file.

## Where configuration options can be found?
All configuration options for the group of hosts that are different for the role defaults must be set in the *group_vars* directory. In our particular case in the [group_vars/basic.yaml](group_vars/basic.yaml) file.

## Useful links
- [The basic role README](roles/basic/README.md)
- [Current task notes](task_notes.md)
