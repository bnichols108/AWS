# AWS-Splunk-Ansible-Docker-Wordpress-Project

I created this project on my own. I recently passed my AWS CCP exam and I'm diving into AWS and many more technologies.

The base of this project is AWS EC2 and Splunk. This project includes the following technologies:
- AWS
- AWS EC2
- AWS Route 53
- AWS IaC (Infrastructure as Code)
- Splunk
- Ansible
- Docker
- Wordpress
- MySQL

# Directories
- configuration:
  - ansible:
    - playbooks:
      - install-docker-and-launch.yaml
      - launch-ec2-instance-v2.yaml
      - manage-rpms.yaml
      - pull-and-launch-wordpress-container.yaml
    - roles:
      - manage-rpms:
        - tasks:
          - main.yml
      - wordpress-docker:
        - tasks:
          - main.yml
        - templates:
          - docker-compose.yml
    - hosts
- screenshots
- splunk-fundamentals:
  - commands
- topology:
  - AWS-Splunk-Ansible-Docker-Wordpress-Project.drawio
  - AWS-Splunk-Ansible-Docker-Wordpress-Project.png
- workflow:
  - part1
  - part2
  - part3
  - part4
  - part5
  - part6
  - part7
  - part8
- README.md

# AWS EC2 and Splunk Topology
![AWS-Splunk-Ansible-Docker-Wordpress-Project](https://user-images.githubusercontent.com/7240371/121840769-8ac70880-cca2-11eb-9d31-efdce9063811.png)


# Workflow
## Part 1 - Spinning up new EC2 instance for splunk, configuring it, installing splunk, and starting it:
- Created new AWS EC2 instance for splunk via my current IaC Ansible playbook.
- In Amazon EC2 Dashboad, renamed the splunk instance.
- Created splunk user on splunk-dev.
- Installed wget, downloaded splunk enterprise rpm, installed rpm.
- Switched to splunk user, started splunk while accepting the license.
- Added the new EC2 instance to the Ansible hosts/inventory file under its own group.
- On new EC2 splunk instance, created ansible user, give sudo privs, allowed password authentication for SSH temporarily.
- Set up password-less SSH between aws-ansible-control and new splunk-dev EC2 instance.
- With security in mind, remove the password authentication for SSH on splunk-dev EC2 instance.
- Tested ansible connection from aws-ansible-control to splunk-dev host group with ping module.

## Part 2 - Reserved a Route 53 domain for Splunk portal, utilized a NLB (Network Load Balancer), Target Group, Registered Target, and an A record:
- Reserved a domain name for 1 year with AWS Route 53 for the splunk-dev EC2 instance. Name is: splunk-dev.link.
- Configured Route 53 - Network Load Balancer, target group, registered target, and Route 53 A records.
- While these DNS changes propogated, I used the EC2 Public IPv4 DNS assigned by AWS.
- Later confirmed that the entire configuration was working properly by hitting the splunk-dev.link:8000 page successfully

## Part 3 - Within the Splunk portal configured a power role and completed the Splunk 7.x Fundamentals training:
- Create a user with the power role.
- Started working on the splunk lab. While uploading practice data to splunk, splunk crashed.
- I determined the AWS free EC2 instance option wasn't able to keep up with splunk.
- Turned off the splunk-dev EC2 instance and changed the instance type.
- Also had to add changes to the server.conf file in order to upload the lab data.

## Part 4 - Spinning up new EC2 instance for Wordpress Docker container, installing Docker, pulling Wordpress and MySQL images, and launching the images: 
- Created a version 2 of my ec2 launcher playbook in order to be more dynamic and allow variables for instance name and security group.
- Ran the v2 ec2 launcher playbook.
- Added the new EC2 instance to the Ansible hosts/inventory file under its own group and also updated the group structure.
- On new EC2 instance, created ansible user, give sudo privs, allow password authentication for SSH temporarily.
- Set up password-less SSH between aws-ansible-control and new aws-docker-wordpress EC2 instance.
- With security in mind, remove the password authentication for SSH on aws-docker-wordpress EC2 instance.
- Tested ansible connection from aws-ansible-control to docker-wordpress host group with ping module.
- Create new playbook to only install and launch docker.
- Create new playbook to only pull and launch wordpress container.
- Ran pull-and-launch-wordpress-container.yaml playbook.
- Added the wordpress-docker role to the pull-and-launch-wordpress-container.yaml file.
- Create directories for the wordpress-docker role.
- Created the tasks for the wordpress-docker role.
- Created the template for the wordpress-docker role.
- Ran the playbook.

## Part 5 - Configuring Wordpress:
- I'm now able to hit port 80 for the public IP for my wordpress instance.
- Configure wordpress.

## Part 6 - Setup the Splunk forwarder from aws-docker-wordpress to splunk-dev:
- Modified the manage-rpms.yaml playbook to now install wget.
- Run the manage-rpms playbook.
- Downloaded the splunk forwarder on aws-docker-wordpress EC2 instance and push file to the docker instance.

## Part 7 - Configure Splunk receiver:
- Log in to splunk frontend
- Configure the receiver on port 9997
- Reset splunk

## Part 8 - Install and configure the Splunk forwarder:
- Install the splunk forwarder.
- Configure the Splunk forwarder.

## Part 9 - Confirm Splunk has received logs from Wordpress container:
- Success!
![screenshot 117](https://user-images.githubusercontent.com/7240371/121838753-24d88200-cc9e-11eb-9c52-1b06a1040d5a.jpg)

# Security Groups
1. SSH-from-personal-external-IP
- Inbound rule: TCP, Port 22, from personal public IP
- EC2 Instances: splunk-dev, AWS-Ansible-Control, AWS-Docker-Wordpress

2. Ansible-Clients
- Inbound rule: TCP, Port 22, from the AWS-Ansible-Control EC2 instance only
- EC2 Instances: splunk-dev, AWS-Docker-Wordpress

3. AWS-Docker-Wordpress-Container
- Inbound rule: TCP, Port 80, from personal public IP
- EC2 Instances: AWS-Docker-Wordpress

4. splunk-dev
- Inbound rule: TCP, Port 8000, from personal public IP
- EC2 Instances: splunk-dev

5. Splunk-Forwarder
- Inbound rule: TCP, Port 9997, from the AWS-Docker-Wordpress EC2 instance only
- EC2 Instances: splunk-dev

# Additional Thoughts and Ideas to Implement in Future Projects
1. Configure Wordpress frontend site (or any public facing site) to use HTTPS (port 443) versus HTTP (port 80).
2. Add passwords to a vault and then place the hashed passwords in the config files instead of plain text.
3. Set-up a proper jump host to allow SSH from only the jump host to the rest of the topology.
