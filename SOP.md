# Standard Operating Procedure: NPI Lab Bring-Up

## Overview
This SOP documents how to bring up a multi-node Rocky Linux cluster from scratch using Vagrant and Ansible.

## Prerequisites
- Fedora with KVM/libvirt installed
- Vagrant and Ansible installed via dnf

## Step 1: Install Vagrant and Ansible
```bash
sudo dnf install vagrant ansible
```

## Step 2: Create Project Directory
```bash
mkdir VagrantProject
cd VagrantProject
```

## Step 3: Create and Configure Vagrantfile
Write your Vagrantfile with:
- 3 Rocky Linux nodes
- 2 CPUs and 1024MB RAM per node
- libvirt provider
- Looped `config.vm.define` for each node

Run `vagrant validate` to confirm syntax.

## Step 4: Boot the Cluster
```bash
vagrant up
```
Wait for all three nodes to boot. Output should show all nodes created successfully.

## Step 5: Capture Node IPs
```bash
vagrant ssh-config
```
Note the HostName (IP address) for each node.

## Step 6: Create inventory.yml
Write an inventory file with:
- All three nodes under a [cluster] group
- Each node's IP address
- SSH user: vagrant
- Private key path from .vagrant/machines/

## Step 7: Create Playbook Structure
```bash
mkdir -p roles/dev-tools/tasks
touch roles/dev-tools/tasks/main.yml
touch site.yml
```

## Step 8: Write Role Tasks
In `roles/dev-tools/tasks/main.yml`, add tasks to:
- Install git
- Install htop

## Step 9: Write Playbook
In `site.yml`, configure the playbook to:
- Target the cluster group
- Apply the dev-tools role
- Run with `become: yes` for sudo

## Step 10: Test Ansible Connectivity
```bash
ansible -i inventory.yml cluster -m ping
```
All nodes should respond with `SUCCESS => { "ping": "pong" }`.

## Step 11: Run the Playbook
```bash
ansible-playbook -i inventory.yml site.yml
```
First run should show `changed: 1` for package installs.

## Step 12: Verify Idempotency
Run the playbook again:
```bash
ansible-playbook -i inventory.yml site.yml
```
Second run should show `changed: 0` on all tasks — proof that the automation is idempotent.

## Verification
SSH into a node and confirm packages:
```bash
vagrant ssh node-1
which git
which htop
exit
```

## Troubleshooting
- **Nodes unreachable:** Run `vagrant reload` to restart them.
- **SSH key errors:** Confirm the private key path in inventory.yml matches `vagrant ssh-config` output.
- **Package install fails:** Confirm `become: yes` is in site.yml.

