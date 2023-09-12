# ansible-playbook-nigix-install

Configuration Management using Ansible:

1. Installed Ansible:

sudo apt update
sudo apt install ansible
verify  ansible --version

2. Connecting passwordless from Ansible server to Target server:

Connect to Ansible and target instance
Generate key using ssh-keygen in both
copy the content of id_rsa.pub and paste it in authorized_keys



copied the public ip of ansible server to target server -- whitelisted

3. ansible adhoc commands -- for small tasks(one or two task)

In ansible server created inventory file and paste private Ip of target and then run:

ansible -i inventory privateIP/all -m "shell" -a "touch demo"

Demo file is created in target server


Gruop servers in Inventory(execute tasks in certain number of vm using grouping) as everthing(server details) is configured in inventory file:

Inventory file:

[dbservers]
IPs

[webservers]
IPs

Use command:
ansible -i inventory dbservers/webservers -m "shell" -a "touch demo"

ansible -i inventory privateIP/all -m "shell" -a "touch demo"

Playbook is used when number of command is reuired to hit(multiple tasks)

4. Playbook

start wrinting ansible playbooks in yml

ubuntu@ubuntu:~/ansible$ cat ansible-playbook.yml
---
- name: Install and Start nginx
  hosts: all
  become: true

  tasks:
    - name: Install nginx
      apt: name=nginx state=latest
    - name: Start nginx
      service:
        name: nginx
        state: started


ansible-playbook -i inventory name

ansible-playbook -i inventory ansible-playbook.yml

Response:

ubuntu@ubuntu:~/ansible$ ansible-playbook -i inventory ansible-playbook.yml

PLAY [Install and Start nginx] *********************************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
ok: [172.31.14.121]

TASK [Install nginx] *******************************************************************************************************************************************************
changed: [172.31.14.121]

TASK [Start nginx] *********************************************************************************************************************************************************
ok: [172.31.14.121]

PLAY RECAP *****************************************************************************************************************************************************************
172.31.14.121              : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

ubuntu@ubuntu:~/ansible$


Issues encountered:

PS C:\Users\H P\Desktop\Riddhi\devops\AWS-CLI\TargetServerKey> ssh -i "TargetServerKey.pem" target@ec2-52-66-68-168.ap-south-1.compute.amazonaws.com
target@ec2-52-66-68-168.ap-south-1.compute.amazonaws.com: Permission denied (publickey).

Resolution:
Permission of Public Key of EC2 instance was not satisfying.


While executing playbook:

TASK [Install nginx] *******************************************************************************************************************************************************
fatal: [172.31.14.121]: FAILED! => {"cache_update_time": 1684202990, "cache_updated": false, "changed": false, "msg": "'/usr/bin/apt-get -y -o \"Dpkg::Options::=--force-confdef\" -o \"Dpkg::Options::=--force-confold\"      install 'nginx'' failed: E: Failed to fetch http://security.ubuntu.com/ubuntu/pool/main/t/tiff/libtiff5_4.3.0-6ubuntu0.4_amd64.deb  404  Not Found [IP: 13.233.101.120 80]\nE: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?\n", "rc": 100, "stderr": "E: Failed to fetch http://security.ubuntu.com/ubuntu/pool/main/t/tiff/libtiff5_4.3.0-6ubuntu0.4_amd64.deb  404  Not Found [IP: 13.233.101.120 80]\nE: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?\n", "stderr_lines": ["E: Failed to fetch http://security.ubuntu.com/ubuntu/pool/main/t/tiff/libtiff5_4.3.0-6ubuntu0.4_amd64.deb  404  Not Found [IP: 13.233.101.120 80]", "E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?"], "stdout": "Reading package lists...\nBuilding dependency tree...\nReading state information...\nThe following additional packages will be installed:\n  fontconfig-config fonts-dejavu-core libdeflate0 libfontconfig1 libgd3\n  libjbig0 libjpeg-turbo8 libjpeg8 libnginx-mod-http-geoip2\n  libnginx-mod-http-image-filter libnginx-mod-http-xslt-filter\n  libnginx-mod-mail libnginx-mod-stream libnginx-mod-stream-geoip2 libtiff5\n  libwebp7 libxpm4 nginx-common nginx-core\nSuggested packages:\n  libgd-tools fcgiwrap nginx-doc ssl-cert\nThe following NEW packages will be installed:\n  fontconfig-config fonts-dejavu-core libdeflate0 libfontconfig1 libgd3\n  libjbig0 libjpeg-turbo8 libjpeg8 libnginx-mod-http-geoip2\n  libnginx-mod-http-image-filter libnginx-mod-http-xslt-filter\n  libnginx-mod-mail libnginx-mod-stream libnginx-mod-stream-geoip2 libtiff5\n  libwebp7 libxpm4 nginx nginx-common nginx-core\n0 upgraded, 20 newly installed, 0 to remove and 0 not upgraded.\nNeed to get 183 kB/2689 kB of archives.\nAfter this operation, 8335 kB of additional disk space will be used.\nIgn:1 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libtiff5 amd64 4.3.0-6ubuntu0.4\nErr:1 http://security.ubuntu.com/ubuntu jammy-updates/main amd64 libtiff5 amd64 4.3.0-6ubuntu0.4\n  404  Not Found [IP: 13.233.101.120 80]\n", "stdout_lines": ["Reading package lists...", "Building dependency tree...", "Reading state information...", "The following additional packages will be installed:", "  fontconfig-config fonts-dejavu-core libdeflate0 libfontconfig1 libgd3", "  libjbig0 libjpeg-turbo8 libjpeg8 libnginx-mod-http-geoip2", "  libnginx-mod-http-image-filter libnginx-mod-http-xslt-filter", "  libnginx-mod-mail libnginx-mod-stream libnginx-mod-stream-geoip2 libtiff5", "  libwebp7 libxpm4 nginx-common nginx-core", "Suggested packages:", "  libgd-tools fcgiwrap nginx-doc ssl-cert", "The following NEW packages will be installed:", "  fontconfig-config fonts-dejavu-core libdeflate0 libfontconfig1 libgd3", "  libjbig0 libjpeg-turbo8 libjpeg8 libnginx-mod-http-geoip2", "  libnginx-mod-http-image-filter libnginx-mod-http-xslt-filter", "  libnginx-mod-mail libnginx-mod-stream libnginx-mod-stream-geoip2 libtiff5", "  libwebp7 libxpm4 nginx nginx-common nginx-core", "0 upgraded, 20 newly installed, 0 to remove and 0 not upgraded.", "Need to get 183 kB/2689 kB of archives.", "After this operation, 8335 kB of additional disk space will be used.", "Ign:1 http://ap-south-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libtiff5 amd64 4.3.0-6ubuntu0.4", "Err:1 http://security.ubuntu.com/ubuntu jammy-updates/main amd64 libtiff5 amd64 4.3.0-6ubuntu0.4", "  404  Not Found [IP: 13.233.101.120 80]"]}


Resolution:

sudo apt update in target server
