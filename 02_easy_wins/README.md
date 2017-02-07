# LAB #
## Gear ##
The gear is composed by a debian vm and four vEOS switchs running on an esxi.
The vEOS switchs are connected in a Clos network.

The VM and the switchs shares a management LAN.

![Lab](/01_build_the_lan/lab.png)

## Directory layout ##
+ /
 \
  | + inventory
  |  \
  |   | + group_vars
  |   |  \ 
  |   |   | all
  |   |   | leaf
  |   |   | spine
  |   |   
  |   | + host_vars
  |   |  \
  |   |   | leaf1
  |   |   | leaf2
  |   |   | spine1
  |   |   | spine2
  |   |
  |   | hosts
  |
  | + reports *
  |  \
  |   |
  |
  | + validators
  |  \
  |   | leaf
  |   | management
  |   | spine
  |   
  | + vault *
  |  \ 
  |   | foobar.yml
  |
  | credential.yml
  | playbook.yml

### Reports ###
reports directory is in gitignore file, thus not in the repository.
The main playbook creates it when needed.

### Vaults ###
vault store the password for the user.
This password is then used inside credentials.yml.
The vault filename must matches the username.
For instance, the vault for adminlocal should be:
adminlocal.yml
```password: foobar```

### Validators ###
validators contains the tasks which retrives and validates the data.
The must append to the array errors the issues as dictionnaries with the keys
info and message.
info is the short name of the issue and message is the detail.

### credential.yml ###
This file contains the transport informations for connecting to the hosts.
It defines a default user if none is passed as argument.

### invocation ###
ansible-playbook -i inventory/hosts playbook.yml
ansible-playbook -i inventory/hosts playbook.yml -e "username=foobar" --ask-vault
ansible-playbook -i inventory/hosts playbook.yml -e "transport=cli"
