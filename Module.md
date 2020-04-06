#### Install ansible on ec2 instance:
Add epel using yum command: 
- yum-config-manager --enable epel
- yum repolist
- yum install ansible -y
#### Setup Module:
```
ansible web -m setup | less
ansible web -m copy -a src='/opt/txt.file dest=/opt/' ( Copy module is also used to create a file along with contents in playbook. {dest,content} )
```
#### Shell and Command module:
`ansible web -m shell -a 'ls -l /opt/'`

`ansible all -m command -a 'pwd'`  ( Only to run single command, it's not useful for special expression like |, >>, Then you should choose shell module instead)
```
ansible web -m command -a 'cat /opt/txt.file | grep -i module'
172.31.0.89 | FAILED | rc=1 >>
cat: invalid option -- 'i'
Try 'cat --help' for more information.non-zero return code

ansible web -m shell -a 'cat /opt/txt.file | grep -i module'
172.31.0.89 | SUCCESS | rc=0 >>
This is for test for Copy module
```
> Note: we can use `shell` command to provide number of command simultaneously, along with regular expressions, but this not possible using `command` module.
#### file module:
```
ansible web -m file -a 'dest=/opt/txt.file mode=777' ( To change the file permission on web server )
ansible db -m file -a 'dest=/opt/txt.file state=touch mode=777' ( To create file with given permission )
ansible db -m file -a 'dest=/opt/txt.file state=absent' ( To delete the file from the given server 'db' )
ansible db -m file -a 'dest=/opt/dir1 state=directory' ( To create a directory )
```
#### Some Regular Playbooks
```
---
- name: Creating a file along with content
  hosts: all
  tasks:
    - name: Creating file with content
      copy:
        dest: "/opt/filewithcontent.txt"
        content: |
          This is file to test Creating module
          Hello Check this
```
```
---
- name: Creating file and directory
  hosts: all
  tasks:
    - name: Ansible create file if it doesn't exist example
      file:
        path: "/opt/file1.txt"
        state: touch

    - name: create a directory
      file:
        path: "/opt/dir2"
        state: directory
```
```
---
- name: Creating multiple file using file module
  hosts: all
  tasks:
    - name: Creating multiple file 
      file:
        path: "/opt/{{ item }}"
        state: absent
      with_items:
        - access.log
        - system.log
        - error.log
        - config.log
```
```
---
- name: Creating multiple file along with different permission
  hosts: all
  tasks:
    - name: Creating multiple file 
      file:
        path: "/opt/{{ item.location }}"
        state: touch
        mode: "{{ item.mode }}"
      with_items:
        - { location: 'lcl1.txt', mode: '0566'}
        - { location: 'lcl2.txt', mode: '0666'}
```

