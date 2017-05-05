Packer Build AMI Example
===========

Installs and setup Zabbix-Agent 3.0 in Amazon Linux 2017

Installation
------------

    $ git clone https://github.com/erozario/packer-build-ami-example.git

Requirements
------------

  - packer (https://www.packer.io/intro/getting-started/install.html)


Example Packer Build
--------------
    {
        "variables": {
            "aws_access_key": "",
            "aws_secret_key": ""
        },
        "builders": [{
            "type": "amazon-ebs",
            "access_key": "{{user `aws_access_key`}}",
            "secret_key": "{{user `aws_secret_key`}}",
            "region": "us-east-1",
            "source_ami": "ami-c58c1dd3",
            "instance_type": "t2.micro",
            "ssh_username": "ec2-user",
            "ami_name": "name_{{timestamp}}",
            "vpc_id": "vpc-fa123d9h",
            "subnet_id": "subnet-1248214f",
            "associate_public_ip_address": true,
            "tags": {
            "Name": "name"
            }
        }],
        "provisioners": [{
            "type": "shell",
            "inline": ["sleep 10"]
        }, {
            "type": "shell",
            "inline": [
                "sudo pip install ansible"
            ]
        }, {
            "type": "ansible",
            "playbook_file": "./playbook.yml"
        }, {
            "type": "shell",
            "inline": [
                "sudo rm -rf /root/.ssh/authorized_keys"
            ]
        }]
    }

Example Playbook
----------------

    - hosts: all
      become: yes
      roles:
        - {role: erozario.zabbix-agent, zabbix_url: localhost}
        
Execute
----------------

    $ packer build template.json 

License
-------

MIT

Author Information
------------------
https://www.linkedin.com/in/eduardo-rozario/
