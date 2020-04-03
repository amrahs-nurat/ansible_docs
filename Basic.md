Ansible Fundamentals: 

1. Introduction to Ansible
2. Setting Up ansible
3. Introduction to YAML
4. Ansible inventory files
5. Ansible Playbooks
6. Variables
7. Conditionals
8. Loops
9. Roles

#### best place to get help of ansible
https://docs.ansible.com

- if you want to communicate with the target machine using ansible, then we have to install ssh for linux machine, and Powershell Remoting for windows.
- Ansible is agentless.
- Information about the target machines are store in the Inventory file, for the inventory file ansible uses default file which is /etc/ansible/hosts.

> #Sample Inventory File
server1.company.com
server2.company.com
[mail]
server3.company.com
server4.company.com
[db]
server5.company.com
server6.company.com
[all_servers:children]
mail
db

# More on Inventory files
- Create alias for server:
```
  web ansible_host=server1.company.com  
  db ansible_host=server5.company.com
  localhost ansible_connection=localhost  `represent that we don't have any target machine use localhost to perform the actions`
```
- Inventory Parameters:
   ```
    ansible_connection - ssh/winrm/localhost
    ansible_port - 22/5989
    ansible_user - root/administrator
    ansible_ssh_pass - password
  ```  
- Security - `Ansible Vaul` `To store password`
- How to disable host-key fingerprint (not recommended way in real prod)
    in `/etc/ansible/ansible.cfg` and uncomment `host_key_checking=false` 


