# Ansible
How to install ansible ?

 Google ---> Pypi  ( Python package index ) and search for ansible 
    https://pypi.org/project/ansible/
On the vm

    $ type pip3.11    ( Pip is the python package manager )
Install Ansible

    $ pip3.11 install ansible  

    Ansible 10 is available on 2.17 of ansible core ( FYI )

# To Do

Create a t3.micro instance ( WS ) : This we would be asing as our base machine and we would be using it till the end of our training.
Ensure this is not spot.

### Ansible Basic Terminology :

1) Ansible Inventory  ( Is nothing but a file that has all the details related to hosts that it's going to do configuration management ) 
2) Ansible can be worked both 
        i) Manual     ( One command at a time : Not really used in the areas of automation )
        ii) Automatic ( Playbook : yaml )

## Ansible Modules vs Collections:

1) Till ansible 2.9, they are reffed as modules 
2) From 2.9, we are referring them as collections

# Why ping is not working ?

I want to check whether my Workstation node can SSH to the nodes 1,2,3 or not.

## telnet destinationAddress portNumber

## timeout 3 telnet destinationAddress portNumber

$ ansible -i inv all  -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ansible.builtin.shell -a uptime

all: is a group that included everything

# Manual way of dealing ansible ( Not recommended, as with this approach, we can only run one command at a time )

In our case, list of actions :

Install nginx,
ansible -i inv frontend  -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ansible.builtin.package -a nginx
Enable and start start nginx
ansible -i inv frontend  -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ansible.builtin.service -a nginx 

Download ,

**If I want to run multiple commands at a time, what can we do ?**

1.We go with playbook approach ( Ansible scripts are referred as playbooks )
2. Playbooks are scripted by using YAML ( yet another markup language : presentation language )
YAML : kubernets, helmCharts, argoCD, GitHubActions

What is a playbook ?

1) A playbook is bothing byt list of plays 
2) A play is nothing but a list of tasks.
3) A task is nothing but a list of actions.
YAML Basics

1) Dictionary    ( A key with a value or a keyVale pair is referred as dictionary )
2) List 
Dictionary :

    name: Martin D'vloper
    job: Developer
    skill: Elite
( Use either one or 2 spaces across the playbook )

martin: name: Martin D'vloper job: Developer skills: - python - perl - pascal
Another way of representing list: fruits: ['Apple', 'Orange', 'Strawberry', 'Mango']

IMP Points

One play cannot have 2 tasks with the same name
If you attempt to use a variable that's not declared, that particular task accessing the variable will be a failure.
Gathering facts:

A fact is nothing but a system property that would gathered by ansible during every-run.

    ansible -i inv frontend  -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ansible.builtin.gather_facts
Ansible is very rich with collections.

Problem Statement :
If we use playbooks directly, we never have any idea on which file is been used by which playbook.
You never know which variable file is used by which playbook.
You cannot reuse the code
ANSIBLE ROLES ( Usage of this is close to what use see prod approach )
    roles/
        common/               # this hierarchy represents a "role"
            tasks/            #
                main.yml      #  <-- tasks file can include smaller files if warranted
            handlers/         #
                main.yml      #  <-- handlers file
            templates/        #  <-- files for use with the template resource
                ntp.conf.j2   #  <------- templates end in .j2
            files/            #
                bar.txt       #  <-- files for use with the copy resource
                foo.sh        #  <-- script files for use with the script resource
            vars/             #
                main.yml      #  <-- variables associated with this role
            defaults/         #
                main.yml      #  <-- default lower priority variables for this role
            meta/             #
                main.yml      #  <-- role dependencies

copy vs template module

Copy just copy - paste from local to remove machine ( You cannot parameterize the files )
If you template collection, you can use it for both copy - paste, along with parameterized files
IMP Regarding Roles and their calling

When you call a specific role, tasks mentioned in the main.yml will be executed.
We can also define tasks in another file tasks/anything.yml and can imports the tasks that are available in anything.yml
What is Role Dependency In Ansible ?

This helps in making one particular task as pre-requisite.
For example, running backend first without making mysql operation doesn't work.

So, we can define role dependency for BACKEND as MySQL, that means, whenever you wish to run backend, mysql will be execute.

Running Playbook using push mechanism :

    ansible-playbook -i inv-dev -e ansible_user=ec2-user -e ansible_password=DevOps321 -e COMPONENT=frontend -e ENV=dev   expense.yml

    ansible-playbook -i frontend-dev.expense.internal,  -e ansible_user=ec2-user -e ansible_password=DevOps321 -e COMPONENT=frontend -e ENV=dev   expense.yml

Ansible-Pull

Ansible also works using pull based mechanism, in this case we don't have to maintain the inventory. But ensure the node that runs this ansible-pull should have ansible installed ?

When to use push vs pull ?

Typically it's a choice. But generally if the inventory is STATIC, then we designate one of the node as Ansible Controller where we make the deployments from here using push. If the inventory is dynamic ( where infra scales up and down dynamically ) in that case we endup using ansible-pull, but the pre-requisite is that the node running ansible-pull should have ansible installed.

And for that, we maintain AMI's as per the application and will ensure we bake "ansible" in to it. So that we don't have install again and again.

Ansible Vault is to encrypt the sensitive strings so that the code will be out of secrets that are plain text. If we use ansible vault, only ansible secrets can be encrypted and decrypted.

But typically, we need to use a tool, that fits with all the technologies.


