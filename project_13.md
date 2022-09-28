# Introducing Dynamic Assignment Into Our structure

Start a `new branch` in the `https://github.com/oayanda/ansible-config-mgt` GitHub repository  and call it `dynamic-assignments`.

![new branch](./images/1.png)

Create a new folder under the main directory `ansible-config-mgt`, name it `dynamic-assignments`. Then inside this folder, create a new file and name it `env-vars.yml`.

![new branch](./images/2.png)

Create a folder to keep each environment’s variables file. Let's called it `env-vars`, then create yaml files for each environment.

![new branch](./images/4.png)

Now paste the instruction below into the `env-vars.yml` file.

```bash
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always

```

![new branch](./images/5.png)

## Update site.yml with dynamic assignments

Update `site.yml` file to make use of the dynamic assignment with the code snippet below

```bash
---
- hosts: all
- name: Include dynamic variables 
  tasks:
  import_playbook: ../static-assignments/common.yml 
  include: ../dynamic-assignments/env-vars.yml
  tags:
    - always

-  hosts: webservers
- name: Webserver assignment
  import_playbook: ../static-assignments/webservers.yml

```

![new branch](./images/6.png)

## Community Roles

Create a role for MySQL database – it should install the MySQL package, create a database and configure users.
However, we can simplily do by using pre-develop, production ready ansible roles from   [Ansible galaxy](https://galaxy.ansible.com/home).

We will use a  [MySQL role developed by geerlingguy](https://galaxy.ansible.com/geerlingguy/mysql).

On Jenkins-Ansible server make sure that git is installed with git --version, then go to ‘`ansible-config-mgt`’ directory and run.

> Disable Your Github webhook - it is not needed to complete and also to maintain the code.

```bash
# To verify Git is installed
git --version
```

![new branch](./images/7.png)

In the ansible-config-mgt directory run the code in the snippet below.

```bash
git init
git pull https://github.com/<your-name>/ansible-config-mgt.git
git remote add origin https://github.com/<your-name>/ansible-config-mgt.git
git branch roles-feature
git switch roles-feature
```

![new branch](./images/8.png)