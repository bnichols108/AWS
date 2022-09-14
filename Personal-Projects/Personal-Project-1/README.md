# AWS EC2 Ansible Project

I created this project on my own. I recently passed my AWS CCP exam and am now ready to dive into AWS.

The base of this project is AWS EC2 and Ansible. This project includes the following technologies:
- AWS
- AWS EC2
- AWS AMI
- AWS IaC (Infrastructure as Code)
- Ansible
- Docker
- Jenkins

Manually launched 3 free redhat EC2 instances from AWS EC2. Every instance had the following applied:
- Used the same keys to my personal desktop
- Located in the same VPC (vpc-7539b008)
- Located in the same subnet (subnet-dd0f0390)
- Located in the same security group (launch-wizard-2)

Modified the AWS security group to allow the following:
- My personal IP to the security group for SSH.
- Any IP within the security group to SSH to any IP in the same security group.

# Directories
- ansible:
  - playbooks:
    - install-docker-launch-jenkins-container.yaml
    - launch-ec2-instance.yaml
    -  manage-vim.yaml
  - hosts
- topology:
  - AWS Ansible Topology.drawio
  - AWS Ansible Topology.png
- workflow:
  - part1-configure-ansible
  - part2-playbook-manage-vim
  - part3-playbook-launch-ec2-instance
  - part4-playbook-install-docker-launch-jenkins-container
- README.md


# AWS Ansible Topology
![AWS Ansible Topology](https://github.com/bnichols108/AWS/blob/97a9fc11310abdd21116d7af5bc726284936985a/Personal-Projects/Personal-Project-1/topology/AWS%20Ansible%20Topology.png)

# Workflow
Part1:
- Grab latest packages, install ansible, create ansible user.
- Create ansible user, give sudo privs, allow password authentication for SSH temporarily.
- Create key pair and set up passwordless SSH to Receiver EC2 instances.
- With security in mind, remove the password authentication for SSH.
- Modified the ansible hosts/inventory file.
- Tested ansible connection from Control to Receivers with ping module.
- Ran ad-hoc ansible command to see redhat release of Receivers.


Part2:
- Created playbooks directory and first playbook to manage VIM.
- Ran manage-vim playbook twice. Once for changes, second to confirm no more changes.


Part3:
- Installed boto since it is required for this AWS module.
- Created playbook to launch EC2 instances.
- Set variables for AWS authentication. Then ran launch-ec2-instance playbook to start a new instance with a RHEL image. 


Part4:
- On new EC2 RHEL instance, create ansible user, give sudo privs, allow password authentication for SSH temporarily.
- Set up passwordless SSH to new RHEL EC2 instance.
- With security in mind, remove the password authentication for SSH. 
- Modified the ansible hosts/inventory file to include new docker EC2 instance.
- Tested ansible connection from Control to Docker instance with ping module.
- Created playbook to manage docker container.
- Ran install-docker-launch-jenkins-container playbook to install docker, docker dependencies, pull jenkins image, start jenkins docker all on the new EC2 RHEL instance. 
