# Ansible Task

## Description
This Ansible playbook suite is designed to set up the following servers:

1. Gateway
2. DHCP
3. DNS
4. App A
5. App B

When App A playbook is run it will start App A and wait for App B to call. It will then return data from URL specified in TARGET_URL environment variable.



## Step 1 - Gateway VM
1. Create a VM with additional network card - Rich's net.
2. Setup ssh from local machine to 172.16.0.55 - ssh-copy-id -i id_rsa.pub 'root@172.16.6.55'
3. Run ansible playbook - ansible-playbook gateway


## Step 2 - DHCP VM
1. Create a VM and connect - Rich's net.
2. Change IP manually to: `192.168.10.2`.
3. Copy ssh key from local machine to DHCP - sh-copy-id -o 'ProxyJump=root@172.16.6.55' 'root@192.168.10.2'
4. Run ansible playbook - ansible-playbook dhcp.yml

## Step 3 - DNS VM
1. Create a VM and connect - Rich's net.
2. Change IP manually to: `192.168.10.3`.
3. Copy ssh key from local machine to DHCP - sh-copy-id -o 'ProxyJump=root@172.16.6.55' 'root@192.168.10.3'
4. Run ansible playbook - ansible-playbook dhcp.yml

## Step 4 - App A VM
1. Create a VM and connect - Rich's net.
2. Change IP manually to: `192.168.10.5`.
3. Copy ssh key from local machine to DHCP - sh-copy-id -o 'ProxyJump=root@172.16.6.55' 'root@192.168.10.5'
4. Store git personal key in ansible [vault](https://www.digitalocean.com/community/tutorials/how-to-use-vault-to-protect-sensitive-ansible-data)

5. Run ansible playbook - ansible-playbook appa.yml
6. Playbook will sit at and wait for call from App B before completing.

## Step 5 - App B VM
1. Create a VM and connect - Rich's net.
2. Change IP manually to: `192.168.10.6`.
3. Copy ssh key from local machine to DHCP - sh-copy-id -o 'ProxyJump=root@172.16.6.55' 'root@192.168.10.6'
4. Run ansible playbook - ansible-playbook appb.yml
5. From cli run - curl http://appa.richie.local:3000
6. Data will be displayed in App B.
7. Playbook running in App A will complete.

## Step 6 - Restart App A
1. Run start_appa to run again - ansible-playbook start_appa.yml
2. From cli (App B) run - curl http://appa.richie.local:3000

# Technologies
## Ansible Vault
Ansible vault used to encrypt github personal key. See step 4 - item 4.

## Ansible roles
Ansible roles for repeated tasks and to improve code structure.
Ansible role for [node installation](https://galaxy.ansible.com/ui/standalone/roles/geerlingguy/nodejs/documentation/) created by Jeff Geerling