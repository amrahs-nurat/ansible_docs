#### What is Yaml file
- yaml- yet another markup language
- it is written using Dictionary, List and Keys-vaules form.
> Notes:
- Dictionary is an `unordered` collection whereas lists are `ordered` collection. 

#### Playbook
- Playbook - A single Yaml file
- Play - Defines a set of activites (tasks) to be run on hosts
- Task - An action to be performed on the host
    1. Execute a command
    2. Run a script 
    3. Install a Package
    4. Shutdown/Restart
```
# Simple Playbook 
- 
  name: play1
  hosts: localhost  
  tasks:
    - name: Test the connectivity
      ping:                         ### Doesn't require any value for it.
      
    - name: Execute command 'date'
      command: date 

    - name: Execute script on server 
      script: test_script.sh

    - name: Install httpd service 
      apt:
        name: httpd
        state: present 

    - name: Start web server 
      service: 
        name: httpd
        state: started 
```
- Module: The different actions run by tasks are called modules.
    1. command
    2. script
    3. apt
    4. service
- To get modules information in cmd use `ansible-doc -l`.
- Execute Ansible Playbook
    `ansible-playbook playbook.yml`
- To get any help `ansible-playbook --help`
## Ansible command run on nodes:
- ansible <hosts> -a <command>
  `ansible all -a "/sbin/reboot"` All group is created by ansible by-default.
- ansible <hosts> -m <module>
  `ansible target1 -m ping`
## Using Ansible Playbook:
      ansible-playbook playbook.yml
> Note: if you don't define the inventory file explicitly, then ansible will take the entry information from the default inventory file (/etc/ansible/hosts). -i option is use to define custom inventory file along with command.
#### Modules
- Ansible Modules are categorized into various groups based on their functionality.
  1. System
  2. Commands
  3. files
  4. Database
  5. Cloud
  6. Windows
  7. More.... 
 ```
 # Simple Playbook 
- 
  name: play1
  hosts: localhost
  task:
    - name: Execute command 'date'
      command: date 

    - name: Display resolv.conf contents 
      command: cat /etc/resolv.conf        ## OR define using below command
    - name: Display resolv.conf contents
      command: cat resolv.conf chdir=/etc      ## First change the directory and then concatenate the file

    - name: Create a folder
      command: mkdir /folder creates=/folder ## first check the command existance then create it.

    - name: Copy the file 
      copy: src=/source_file dest=/destination_file ## this comes under the "free_form" Parameter

    - name: Run script 
      script: /some/local/script.sh -arg1 -arg2

    - name: service module use for to start,stop and restart
      service: name=postgresql state=started 
    
    - name: Add DNS Server to resolv.conf
      lineinfile: ## This module is idempotent
        path: /etc/resolv.conf
        line: 'nameserver 10.1.1.12'
    # lineinfile is to search for a line in a file and replace it or add it if it doesn't exist.
```
