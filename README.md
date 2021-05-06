# AnsibleAWS

### task

- installing dependencies:
- `sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
sudo apt install python3-pip -y
pip3 install awscli
pip3 install boto boto3`
- then update and upgrade


- `scp -i ~/.ssh/eng84devops.pem -r eng84devops.pem ubuntu@public_ip_controller:~/.ssh/` to copy a file
- `[app]
publicIP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/eng84devops.pem` to add to hosts
- `[db]
3.250.109.4 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/eng84devops.pem`

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
