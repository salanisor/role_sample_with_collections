role_cisco_nxos
=========

This is an example role with its project folder named exactly after its purpose.

A more appropriate example would be given that we're targeting Cisco's NXOS modules.

    role_cisco_nxos
    
In this directory you will create the following structure and will allow you to turn it into a git repository to use as-is within Ansible Tower or through the cli.

#### Example that you will end with.
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
├── pb_cisco_nxos.yaml
├── README.md
└── roles
    └── role_cisco_nxos
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        ├── templates
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml
```

#### Role creation.

* Step 1) Create the required sub-directories inside your new project directory `role_cisco_nxos`

      mkdir -p role_cisco_nxos/{collections,roles}
      
* Step 2) cd into the `role_cisco_nxos` directory and add the following text into the `ansible.cfg` file that you need to also create. This will allow you run the `ansible-galaxy` command to appropriately install Cisco's NXOS collection into your project directory. 

      [defaults]
      # Installs collections into [collections]/ansible_collections/namespace/collection_name
      collections_paths = ./collections

      # Installs roles into [role_cisco_nxos]/roles/namespace.rolename
      roles_path = ./roles
   
* Step 3) Run the following to create the project directory with the necessary files.

      ansible-galaxy init --init-path role_cisco_nxos/roles/ role_cisco_nxos

* Step 4) On a system with Internet access, run the following `ansible-galaxy` command to install the nxos and commons collections into your collections directory.           First, change directories to the project's top level directory `role_cisco_nxos`
      
      ansible-galaxy collection install cisco.nxos

* Step 5) Verify that the following directory structure with the contents is created upon the successful result of step 4.

      ├── collections
      │   └── ansible_collections
      │       ├── ansible
      │       │   └── netcommon
      │       └── cisco
      │           └── nxos
      
* Step 6) Create the playbook to run your tasks. I use the prefix `pb` for playbook proceeded by the role name `pb_cisco_nxos.yaml`
          See the [example-playbook](https://github.com/salanisor/role_sample_with_collections/blob/master/README.md#example-playbook) below for contents.

* Step 7) Add the neccessary variables to file `defaults/main.yml` required for your `httpapi` connection.
          See the [role-defaults](https://github.com/salanisor/role_sample_with_collections/blob/master/README.md#role-defaults) below for contents.
          Note that we add them to the `defaults` section since you can simply override these at runtime, especially when passing the username and password.

* Step 8) Finally, add the neccesary tasks in the `tasks/main.yml` file. **Important to note** that now when calling the NXOS modules you have to prefix using the namespace (cisco) then the collection name (nxos) followed by the module name (nxos_config).

#### Example:
      ---
      # tasks file for role_cisco_nxos
      - name: "Configurable Cisco NXOS Backup Path"
        cisco.nxos.nxos_config:
          backup: yes
          backup_options:
            dir_path: "/etc/ansible/backups/nxos/{{ inventory_hostname }}"
            filename: backup.cfg

Requirements
------------

Completion of the steps above.

Role Defaults
-------------

      ansible_httpapi_use_ssl: yes
      ansible_httpapi_validate_certs: no
      ansible_network_os: nxos
      httpapi_user: ''
      httpapi_password: ''

Dependencies
------------

Cisco's NXOS and Ansible's netcommon collections.

Run on a computer with Internet access to be able to download the required collections.

    ansible-galaxy collection install cisco.nxos

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

Command line deploy.

```
ansible-playbook -vvvv pb_cisco_nxos.yaml -e "ansible_user='httpapi-user',ansible_httpapi_password='httpapi-password'"
```

    ---
    - hosts: nxos
      gather_facts: false
      connection: httpapi
      roles:
        - { role: role_cisco_nxos }

License
-------

BSD

Author Information
------------------

Copyright: (c) 2021, salanisor <[email protected]>
