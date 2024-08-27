# Linux Users - HowTo: Setup Ansible and Python Environment

## Introduction
This guide walks you through the steps required to setup your Ansible and Python environment on a Linux-based system. 
This includes installing necessary packages, configuring Ansible for individual use, and managing playbook directories.

## Requirements
- A Linux-based system.
- Sudo or root access.

## Installation Steps

### 1. Install Ansible and Python Packages
Run the following command to install Ansible, Python, and required dependencies:

```bash
sudo dnf install python3 python3-devel python3-pip python3-setuptools rsync ansible-core tree
```

## User Configuration

### 1. Setup for Individual Users
Navigate to your home directory and install necessary Ansible collections:

```bash
cd ~/
ansible-galaxy collection install --force community.general
ansible-galaxy collection install --force amazon.aws
```

This process creates a local `.ansible` directory where all user-installed modules will reside. 
You can confirm this by listing the contents of your home directory:

```bash
ls -a
```

You should see the `.ansible` directory listed among others.

### 2. Central Directory for Ansible Playbooks
If you maintain a central directory for your Ansible playbooks and wish to redirect users on login, 
the steps to achieve this will depend on your specific system configuration and requirements. 
This section can be customized to provide instructions based on your organization's infrastructure.

Here is an example:

```bash
## create and edit /etc/profile.d/ansibledir.sh
## redirect users on login to /local/ansible
DIR=/local/ansible
if [ -d "$DIR" ]; then
    cd $DIR
    echo "Your current working path is:"
    echo ""
    tput setaf 2; echo `pwd`
    echo ""
    tput setaf 7

fi
```



