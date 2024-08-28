====================================
OpenStack Live Migration by Ansible
====================================

- The purpose of this playbook is to automate the live-migration of VMs on OpenStack Cloud.

- Prerequisites:-

  As in normal situation before running any openstack command you need to source the keystone.rc file of the desired tenant. The same here before running the playbook you need to edit in the inventory file "./hosts" and feed it with the proper valid values taken from the keystone.rc file of the desired tenant. Most probably you'll used the admin role to accomplish this task, however choosing the tenant is up to you.

- Roles architecture is shown below with a brief description of each file.
.
|-- hosts                               ---> Inventory file contains remote host and the environment vars
|-- live-migrate.yml                    ---> The main playbook which icludes the main Role
`-- roles
    |-- compute_nodes                   ---> The role which includes all tasks related to hypervisors
    |   `-- tasks
    |       |-- dsthost.yml             ---> A task to collect destination compute node
    |       |-- list_hypervisors.yml    ---> A task to list all hypervisors
    |       `-- srchost.yml             ---> A task to collect source compute node
    |-- perform_live_migration          ---> The main Role which includes all sub roles
    |   |-- tasks
    |       `-- main.yml                ---> The main task which includes all sub tasks                  
    `-- vm-migration                    ---> The role which includes all tasks related to VM migration operation
        `-- tasks
            |-- list-vm.yml             ---> A task to list all VMs on source compute node
            |-- migrate-vm.yml          ---> A task to migrate VM from source to destination compute node
            `-- validate-migration.yml  ---> A task to validate migration 


- How to run it ?
  
  1- from inside ansible container run command ---> "ansible-playbook -i /root/ansible/openstack-live-migration/hosts /root/ansible/openstack-live-migration/live-migrate.yml"

  2- from OpenStack-client-fra run command ---> "docker exec -it <ansible-container-id> ansible-playbook -i /root/ansible/openstack-live-migration/hosts /root/ansible/openstack-live-migration/live-migrate.yml"
