

## Server software requirements

Ubuntu [18.04 LTS minimal](https://help.ubuntu.com/community/Installation/MinimalCD) server or similar Linux distribution on a virtual or physical machine is required. Unless you're an expert Linux admin with [Ansible](https://www.ansible.com/) skills, use the [64-bit Ubuntu 18.04 "Bionic Beaver" mini.iso](http://archive.ubuntu.com/ubuntu/dists/bionic/main/installer-amd64/current/images/netboot/mini.iso) netboot image or [64-bit Ubuntu 18.04 Server](https://www.ubuntu.com/download/server) image in case netboot will not work in your environment. The mini.io netboot creates the smallest footprint server so it's the most secure and requires minimal hardening for security.

Setup a Windows Hyper-V, VMware, VirtualBox, or other hypervisor VM:  

 - RAM: minimum 4096 megabytes
 - Storage:   minimum 32 gigabytes, preferably 256 gigabytes 
 - Network: Accessible  outbound to the Internet (both IPv4 and IPv6), inbound access not required
 - Firewall Route: The publically accessible IP should point to  this server, a Linux firewall is automatically managed by CHIE

## Setup the operating system

First, install Ubuntu 18.04.1 LTS or above minimal server on your preferred hypervisor. The smaller the footprint, the safer and more secure the system. You can use most of the defaults, but provide the following defaults when you are asked to make choices:

 - Hostname: *chie*.
 - Default User Full Name: *CHIE Admin*
 - Default User Name: *chieadmin*
 - Default User Password: *chieAdminDefault!*
 - Disk Partitioning: *Guided - use entire disk*
 - PAM Configuration: *Install security updates automatically*
 - Software Packages: *OpenSSH is the only package that must be installed by default*
 - NOTE: the user you create is called the admin user below.
## Prepare your CHIE appliance for our software
After installation is completed, log into the server as user chieadmin.
Install the following core utilities:

    sudo apt update && sudo apt install net-tools curl -y && \
    curl https://raw.githubusercontent.com/CitusHealth/citus-health-integration-engine-appliance-setup/master/bin/bootstrap.sh | bash

After bootstrap.sh is complete, exit the shell.

## Setup your account-specific CHIE variables

The Citus Health Customer Success Team will supply you with a appliance.secrets.conf.yml configuration file via secure e-mail or other mechanism. Copy the contents of that file into the CHIE server.
Login to the server as chieadmin and run:

    sudo vi /etc/appliance-setup-framework/conf/appliance.secrets.conf.yml

In addition, if you are using WellSky's CPR+ integration:

    sudo vi /etc/appliance-setup-framework/conf/wellsky-cprplus.secrets.conf.json
Once you've done that, the /etc/appliance-setup-framework/conf will look something like this:

     > ls -al /etc/appliance-setup-framework/conf    
        drwxr-xr-x 2 root root 4096 Nov  6 14:59 .
        drwxr-xr-x 7 root root 4096 Nov  6 14:35 ..
        -rwxr-xr-x 1 root root 2252 Nov  6 14:35 appliance.common.conf.yml
        -rw-r--r-- 1 root root 1115 Nov  6 09:53 appliance.secrets.conf.yml
        -rwxr-xr-x 1 root root 1103 Nov  6 14:35 appliance.secrets.conf-tmpl.yml
        -rwxr-xr-x 1 root root   56 Nov  6 14:35 .gitignore
        -rw-r--r-- 1 root root 1222 Nov  6 14:35 prometheus-osquery-exporter-config.yaml
        -rw-r--r-- 1 root root 2935 Nov  6 14:35 ubuntu-18.04-netboot.iso-preseed.cfg
        -rw-r--r-- 1 root root  168 Nov  6 09:03 wellsky-cprplus.secrets.conf.json
        -rw-r--r-- 1 root root  163 Nov  6 14:35 wellsky-cprplus.secrets.conf-tmpl.json

* The **appliance.secrets.conf-tmpl.yml** file is a template (sample), and the **appliance.secrets.conf.yml** will be the file given to you by the Citus Health Customer Success Team.
* The **wellsky-cprplus.secrets.conf-tmpl.json** file is a template (sample), and **wellsky-cprplus.secrets.conf.json** customer need to modify the values based on the wellsky-cprplus db host, db name , username and password. Need to make sure Variables in the template file must be same as template file 

The bin/setup.sh utility will run all numbered playbooks in numerical order.
Install software

    cd /etc/appliance-setup-framework
    bash bin/setup.sh

After setup is completed, reboot the server (Docker setup will be incomplete without a reboot):

    sudo reboot
After reboot is completed, log into the server as user chieadmin.  

## Getting started with CHIE's Filing Center

The CHIE has automated support for the Citus Health Filing Center ("FC"). By default, the FC is installed via Samba as user filing_center and connected to your Citus Health Account automatically.
You can connect your Windows PC inside your network to the CHIE Appliance using:

    net use F: \\chie\filing_center
The user name and password are provided in the appliance.secrets.conf.yml file:
appliance_filing_center_user is the share's username

appliance_filing_center_samba_passwd is the share's password

## Getting started with CHIE's Virtual Printer
A default Virtual Printer has already been installed automatically during the CHIE setup process. To set up the printer in your pc follows the instruction given in the link [How to set up a virtual printer?](https://docs.citushealth.com/overview/cupssetup/).

## Getting started with CHIE's CPR+ FHIR Server
CPR+ FHIR Server will be automatically installed and will start to syncronize new patient to Citus health FHIR server.  To verify the changes in the Citus Health Application, please login to the application as a staff. The The Citus Health Customer Success Team will supply the Application URL and the credentials.

