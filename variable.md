## Variable in playbook
- Variable: To store the information which varies to each host.
- How to define variables in playbook: `vars`
```
# Simple Playbook 
- 
  name: play1
  hosts: localhost
  vars:    ## Define variable 
    dns_server: 10.1.1.12
  task:
    - name: Add DNS Server to resolv.conf
      lineinfile: 
        path: /etc/resolv.conf
        line: 'nameserver {{dns_server}}'  ## Define variable '' quotes is important while define the variables
```
### Sample Ansible Playbook for variables
```
# Simple Playbook 
- 
  name: Set Firewall Configuration 
  hosts: web 
  tasks: 
    - firewalld: 
        service: httpd
        permanent: true 
        state: enabled 
    - firewalld: 
        port: '{{ httpd_port }}'/tcp
        permanent: true 
        state: disabled
    -  firewalld: 
        port: '{{ snmp_port }}'/udp
        permanent: true 
        state: disabled  
    -  firewalld: 
        port: '{{ inter_ip_range }}'/24
        permanent: true 
        state: enabled
```
# sample Inventory file
`web http_port=8081 snmp_port=161-162 inter_ip_range=192.0.2.0`
# OR 
- Using Jinja Template, recommended way to define variables
```
# sample variable file - web.yml   
http_port=8081
snmp_port=161-162
inter_ip_range=192.0.2.0
```
#### Conditional 
- To instruct when need to execute the tasks. 
- How to apply condition if we have two web servers (linux) and one db server (windows). To start the services on based on their functionality.
```
# Simple Inventory File 

web1 ansible_host=web1.company.com ansible_connection=ssh ansible_ssh_pass=P@ssword
db ansible_host=db.company.com ansible_connection=winrm ansible_ssh_pass=P@ssword
web2 ansible_host=web2.company.com ansible_connection=ssh ansible_ssh_pass=P@ssword

[all_servers] # Group 
web1
db
web2

# sample Ansible Playbook.yml
- 
  name: Start service 
  hosts: all_servers
  tasks: 
    - service: name=mysql state=started
      when: ansible_host == "db.company.com"

    - service: name=httpd state=started 
      when: ansible_host == "web1.company.com" or 
            ansible_host == "web2.company.com"
```
- Recommended way in such cases
```
# Simple Inventory File 

web1 ansible_host=web1.company.com ansible_connection=ssh ansible_ssh_pass=P@ssword
db ansible_host=db.company.com ansible_connection=winrm ansible_ssh_pass=P@ssword
web2 ansible_host=web2.company.com ansible_connection=ssh ansible_ssh_pass=P@ssword

[db_servers]
db 

[web_servers]
web1
web2

# sample Ansible Playbook.yml
- 
  name: Start service 
  hosts: db_servers
  tasks: 
    - service: name=mysql state=started

- 
  name: Start service 
  hosts: web_servers
  tasks: 
    - service: name=httpd state=started 
```
### Another example of Conditional 
```
# sample Ansible Playbook.yml
- 
  name: Check status of service and email if its down 
  hosts: localhost
  tasks: 
    - command: service httpd status 
      register: command_output

    - mail:
        to: Admin <system.admins@company.com>
        subject: Service Alert 
        body: 'Service {{ ansible_hostname }} is down'
      when: command_output.stdout.find('down') != -1
```
