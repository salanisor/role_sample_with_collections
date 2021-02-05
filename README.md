role_cisco_nxos
=========

This is an example role with its project folder named exactly after its purpose.

A more appropriate example would be given that we're targeting Cisco's NXOS modules.

    role_cisco_nxos
    
In this directory you will create the following structure and will allow you to turn it into a git repository to use as-is inside of Ansible Tower or through the cli.

#### Example what you will end with.
```
tree -L 4
.
├── ansible.cfg
├── collections
│   └── ansible_collections
│       ├── ansible
│       │   └── netcommon
│       └── cisco
│           └── nxos
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── pb_sample_with_collections.yaml
├── README.md
├── tasks
│   └── main.yml
├── templates
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml

14 directories,
```

#### Role creation.

* Step 1) On a system with Internet access run the following to create the project directory with most necessary files. 

      ansible-galaxy init role_cisco_nxos
      
* Step 2) cd into the role directory and add the following text into the `ansible.cfg` file. This will allow you run the ansible-galaxy command to appropriately install the Cisco's NXOS collection. 

      [defaults]
      # Installs collections into [collections]/ansible_collections/namespace/collection_name
      collections_paths = ./collections

      # Installs roles into [current dir]/roles/namespace.rolename
      roles_path = ../
      
* Step 3) Create the collections directory inside your current directory `role_cisco_nxos`

      mkdir collections

* Step 4) Now run the following `ansible-galaxy` command to install the nxos and commons collections.

      ansible-galaxy collection install cisco.nxos

* Step 5) Verify that the following directory structure with the contents is created upon the successful result of step 4.

      ├── collections
      │   └── ansible_collections
      │       ├── ansible
      │       │   └── netcommon
      │       └── cisco
      │           └── nxos
      
* Step 6) Create the playbook to run your tasks. I use the prefix `pb` for playbook proceeded by the role name `pb_cisco_nxos.yaml`
          See [Requirements](https://github.com/salanisor/role_sample_with_collections/blob/master/README.md#requirements) below for contents.



Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

Cisco's NXOS and Ansible's netcommon collections.

Run on a computer with Internet access to access and download the required collections.

    ansible-galaxy collection install cisco.nxos

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ---
    - hosts: nxos
      gather_facts: true
      connection: httpapi
      roles:
          - { role: role_cisco_nxos }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
