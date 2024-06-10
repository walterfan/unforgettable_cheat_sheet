# Ansible cheat sheet


## Glossary

|Ansible Keywords | Description |
| --- | --- |
| Control Node | a system where Ansible is installed and configured to connect and execute commands on nodes. |
| Node | a server controlled by Ansible.|
| Inventory File | a file that contains information about the servers Ansible controls, typically located at /etc/ansible/hosts.|
| Playbook| a file containing a series of tasks to be executed on a remote server.|
| Play| a full Ansible run. A play can have several playbooks and roles, included from a single playbook that acts as entry point |
| task | An Action performed by ansible on remote hosts. |
| role| it is an organized structure directory with files containing a predefined set of tasks, handlers, variables and files|
| Module | These are Reusable, Standalone ansible scripts sued to perform tasks on Worker nodes. |
| vars | It is used for defining the variables that can be used through out the playbook for dynamic configurations. |
| register | This keyword helps in capturing the ouptut of the tasks and stores in a variable for later use in playbook. |
| notify | This keyword used to trigger the handlers when specific conditions are matched such as service state changes. |


## ansible modules

| Ansible Modules | Usage |
| --- | --- |
| apt | This module manages the packages on Debian and Ubuntu Systems. |
| yum | This module manages the packages on Red Hat/ Cent [OS](https://www.geeksforgeeks.org/what-is-an-operating-system/) systems|
| copy | This module helps in copying the files from local or Remote system to Destination system. |
| file | This module manages the files and directories in local or Remote system. |
| service | This module manages the services in the ansible|
| shell | This module helps in executing shell commands on remote hosts. |
| template | This module help in using the Jinja2 templates allowing dynamic content usage. |
| cron | This module helps in managing the cron jobs, including the creation, modification and removal. |
| git | This module helps in managing the repositories allowing tasks such as cloning, pulling and pushing. |


## ansible cli

refer to https://docs.ansible.com/ansible/latest/command_guide/cheatsheet.html

### ansible-playbook
```
ansible-playbook -i /path/to/my_inventory_file -u my_connection_user -k -f 3 -T 30 -t my_tag -M /path/to/my_modules -b -K my_playbook.yml
```

Loads my_playbook.yml from the current working directory and:

```
-i - uses my_inventory_file in the path provided for inventory to match the pattern.

-u - connects over SSH as my_connection_user.

-k - asks for password which is then provided to SSH authentication.

-f - allocates 3 forks.

-T - sets a 30-second timeout.

-t - runs only tasks marked with the tag my_tag.

-M - loads local modules from /path/to/my/modules.

-b - executes with elevated privileges (uses become).

-K - prompts the user for the become password.
```


### ansible-galaxy
#### Installing collections

* Install a single collection:
```
ansible-galaxy collection install mynamespace.mycollection
```

Downloads mynamespace.mycollection from the configured Galaxy server (galaxy.ansible.com by default).

* Install a list of collections:

```
ansible-galaxy collection install -r requirements.yml
```
Downloads the list of collections specified in the requirements.yml file.

* List all installed collections:
```
ansible-galaxy collection list
```

#### Installing roles

* Install a role named example.role:

```
ansible-galaxy role install example.role

# SNIPPED_OUTPUT
- extracting example.role to /home/user/.ansible/roles/example.role
- example.role was installed successfully
```

* List all installed roles:

```
ansible-galaxy role list
```

See ansible-galaxy for detailed documentation.

### ansible

#### Running ad-hoc commands

* Install a package
```
ansible localhost -m ansible.builtin.apt -a "name=apache2 state=present" -b -K
```
Runs ansible localhost- on your local system. - name=apache2 state=present - installs the apache2 package on a Debian-based system. - -b - uses become to execute with elevated privileges. - -m - specifies a module name. - -K - prompts for the privilege escalation password.

```
localhost | SUCCESS => {
"cache_update_time": 1709959287,
"cache_updated": false,
"changed": false
#...
```

### ansible-doc

* Show plugin names and their source files:

```
ansible-doc -F
#...
```

* Show available plugins:

```
ansible-doc -t module -l
#...
```




### ansible command line

| Category | Command/Option | Explanation |
|---|---|---|
| General | `ansible --version` | Display the version of Ansible installed. |
| General | `ansible -m ping all` | Ping all hosts to check if they are reachable. |
| General | `ansible -m shell -a 'free -m' all` | Run the 'free -m' shell command on all hosts. |
|---|---|---|
| Playbooks | `ansible-playbook playbook.yml` | Run the playbook named `playbook.yml`. |
| Playbooks | `ansible-playbook -i inventory.ini playbook.yml` | Run a playbook with a specified inventory file. |
| Playbooks | `ansible-playbook playbook.yml --syntax-check` | Perform a syntax check on the playbook. |
| Playbooks | `ansible-playbook playbook.yml --start-at-task='taskname'` | Start the playbook at the specified task. |
| Playbooks | `ansible-playbook playbook.yml --list-hosts` | List all hosts the playbook will run against. |
| Playbooks | `ansible-playbook playbook.yml --list-tasks` | List all tasks the playbook will execute. |
| Playbooks | `ansible-playbook playbook.yml --step` | Execute the playbook interactively, asking for confirmation at each step. |
| Playbooks | `ansible-playbook playbook.yml --check` | Do a dry run of the playbook without making actual changes. |
| Playbooks | `ansible-playbook playbook.yml --diff` | Show differences in files when running the playbook. |
| Playbooks | `ansible-playbook playbook.yml --tags "tag1,tag2"` | Run only the tasks tagged with tag1 and tag2. |
| Playbooks | `ansible-playbook playbook.yml --skip-tags "tag3"` | Skip tasks tagged with tag3. |
| Playbooks | `ansible-playbook playbook.yml --limit servers` | Limit the playbook execution to the group 'servers'. |
| Playbooks | `ansible-playbook playbook.yml --extra-vars "version=1.2.3"` | Run the playbook with extra variables. |
| Playbooks | `ansible-playbook playbook.yml --forks=5` | Set the number of parallel processes to 5. |
| Playbooks | `ansible-playbook playbook.yml -v` | Run the playbook in verbose mode. `-vvv` and `-vvvv` can be used for more verbosity. |
| Playbooks | `ansible-playbook playbook.yml --check --diff` | Dry-run the playbook and show file differences. |
|---|---|---|
| Roles | `ansible-galaxy init role_name` | Initialize a new role structure with the specified name. |
| Roles | `ansible-galaxy install role_name` | Install an Ansible role. |
|---|---|---|
| Vault | `ansible-vault create vault.yml` | Create a new encrypted file. |
| Vault | `ansible-vault view vault.yml` | View an encrypted file. |
| Vault | `ansible-vault edit vault.yml` | Edit an encrypted file. |
| Vault | `ansible-vault decrypt vault.yml` | Decrypt an encrypted file. |
| Vault | `ansible-vault encrypt vault.yml` | Encrypt a file. |
| Vault | `ansible-vault rekey vault.yml` | Change the password on a vault file. |
| Vault | `ansible-vault encrypt_string --name 'var_name' 'string'` | Encrypt a string and output it in a format ready for use with Ansible. |
| Vault | `ansible-playbook playbook.yml --ask-vault-pass` | Run a playbook and prompt for the vault password. |
| Vault | `ansible-playbook playbook.yml --vault-password-file vault.pass` | Run a playbook using a file containing the vault password. |
| Vault | `ansible-vault encrypt_string --stdin-name 'var_name'` | Read the string from stdin and encrypt it. |
| Vault | `ansible-vault encrypt --vault-id dev@prompt vars.yml` | Encrypt a file using a prompt for the vault password. |
| Vault | `ansible-playbook playbook.yml --vault-id dev@prompt` | Run a playbook using a prompt for the vault password. |
|---|---|---|
| Configuration | `ansible-config list` | List all configuration options. |
| Configuration | `ansible-config dump` | Show the current configuration. |
| Configuration | `ansible-config view` | View the current Ansible configuration. |
| Configuration | `ansible-config dump --only-changed` | Dump configuration items that have changed from the default. |
| Configuration | `ansible-config set ANSIBLE_HOST_KEY_CHECKING=False` | Set a specific configuration item. |
| Configuration | `ansible-config unset ANSIBLE_HOST_KEY_CHECKING` | Unset a specific configuration item. |
|---|---|---|
| Console | `ansible-console` | Start an interactive console for executing Ansible tasks. |
| Console | `ansible-console -i inventory.ini` | Start an interactive console using a specific inventory. |
|---|---|---|
| Modules | `ansible-doc -l` | List available modules. |
| Modules | `ansible-doc module_name` | Get documentation for a specific module. |
|---|---|---|
| Inventory | `ansible-inventory --list -y` | List the inventory in YAML format. |
| Inventory | `ansible-inventory --graph` | Show a graph of the inventory. |
| Inventory | `ansible -i inventory.ini all -m ping` | Use a specific inventory file and ping all hosts. |
| Inventory | `ansible-inventory --host hostname` | Display all variables for a specific host. |
| Inventory | `ansible-inventory --playbook-dir . --graph` | Display a graph of the inventory from the current directory. |
| Inventory | `ansible -i 'localhost,' -c local -m ping` | Ping localhost using local connection and ad-hoc inventory. |
| Inventory | `ansible-inventory --list --yaml` | List inventory in YAML format. |
|---|---|---|
| Pull | `ansible-pull -U git_url` | Pull a Git repository of Ans
| Pull | `ansible-pull -U git_url` | Pull a Git repository of Ansible configurations on the target host. |
|---|---|---|
| Galaxy | `ansible-galaxy collection init my_namespace.my_collection` | Initialize a new collection with the given namespace and name. |
| Galaxy | `ansible-galaxy collection build` | Build an Ansible collection package ready for distribution. |
| Galaxy | `ansible-galaxy collection install my_namespace.my_collection` | Install an Ansible collection from Galaxy. |
| Galaxy | `ansible-galaxy role init my_role` | Initialize a new role with the given name. |
| Galaxy | `ansible-galaxy role install my_role` | Install an Ansible role from Galaxy. |
| Galaxy | `ansible-galaxy list` | List installed roles and collections. |
| Galaxy | `ansible-galaxy collection install -r requirements.yml` | Install collections from a requirements file. |
| Galaxy | `ansible-galaxy role install -r requirements.yml` | Install roles from a requirements file. |
| Galaxy | `ansible-galaxy collection publish ./namespace-collection-1.0.0.tar.gz --api-key=your_token` | Publish a collection to Galaxy using an API token. |
| Galaxy | `ansible-galaxy role init --role-skeleton skeleton my_role` | Initialize a new role with a specified role skeleton. |
|---|---|---|
| Modules | `ansible localhost -m file -a "path=/tmp/test state=touch"` | Create a new file on the local host using the file module. |
| Modules | `ansible localhost -m package -a "name=vim state=present"` | Install a package on the local host using the package module. |
| Modules | `ansible localhost -m command -a "uptime"` | Run a command on the local host using the command module. |
| Modules | `ansible localhost -m user -a "name=testuser state=absent"` | Remove user 'testuser' from the local host using the user module. |
| Modules | `ansible localhost -m service -a "name=httpd state=restarted"` | Restart the 'httpd' service on the local host using the service module. |
| Modules | `ansible localhost -m copy -a "src=/etc/hosts dest=/tmp/hosts"` | Copy '/etc/hosts' to '/tmp/hosts' on the local host using the copy module. |
| Modules | `ansible localhost -m file -a "path=/tmp/test state=absent"` | Remove file '/tmp/test' on the local host using the file module. |
| Modules | `ansible localhost -m apt -a "name=nginx state=latest"` | Install the latest version of 'nginx' on the local host using the apt module (Debian-based systems). |

## reference
* https://github.com/devops-cheat-sheets/ansible-cheat-sheet
* https://www.geeksforgeeks.org/ansible-cheat-sheet/