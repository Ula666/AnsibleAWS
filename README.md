# AnsibleAWS

### Task: ansible to creating ec2 instances with playbooks using Ansible-vault to secure the aws keys
1. First we have to create an EC2 instance on AWS called Controller.
2. SSH to this instance using SSH command and .pem file
3. Install the required dependecies:
- `sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
sudo apt install python3-pip -y
pip3 install awscli
pip3 install boto boto3`
- then update and upgrade

4. Create a new ssh key, or securly copy the one you used to ssh to this instance
- `scp -i ~/.ssh/eng84devops.pem -r eng84devops.pem ubuntu@public_ip_controller:~/.ssh/` to copy a file

5. Create Ansible directory structure:
- `mkdir -p AWS_Ansible/group_vars/all/`
- `cd AWS_Ansible`
- `touch playbook.yml`

6. Create Ansible Vault file to store the AWS Access and Secret keys.
- `ansible-vault create group_vars/all/pass.yml`
- `New Vault password:`
- `Confirm New Vault password:`
7. Edit the pass.yml
- `ansible-vault edit group_vars/all/pass.yml` 
- enter Vault password:
- add the access key and secret key:
- `ec2_access_key: AAAAAAAAAAAAAABBBBBBBBBBBB`                                      
- `ec2_secret_key: afjdfadgf$fgajk5ragesfjgjsfdbtirhf`

8. Run `tree` to see the tree structure
9. Go back to the playbook you created and edit it
- `sudo nano playbook.yml`

- example of creating a ec2 instance from playbook:
- `- name: APP instance
      ec2:
        aws_access_key: "{{ aws_access_key }}"
        aws_secret_key: "{{ aws_secret_key }}"
        key_name: "{{ key_name }}"
        id: eng84_ula_ansible_app
        vpc_subnet_id: "{{ public_subnet }}"
        group_id: "{{ public_sg }}"
        image: "{{ image }}"
        instance_type: t2.micro
        region: "{{ region }}"
        wait: true
        count: 1
        assign_public_ip: yes
        instance_tags:
          Name: eng84_ula_ansible_app`

10. Run the playbook
- ` sudo ansible-playbook playbook.yml --ask-vault-pass` and enter the password
11. Run the playbook again and create instances:
- `sudo ansible-playbook playbook.yml --ask-vault-pass --tags create_ec2 `
12. Add information about our new instances to hosts file:
- `[app]`
- `publicIP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/eng84devops.pem` to add to hosts
- `[db]`
- `3.250.109.4 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/eng84devops.pem`
13. ping
- `ping 3.250.74.39`
14. ssh to instances
- find the ssh command in aws
