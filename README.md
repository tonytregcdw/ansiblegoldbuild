# Ansible Windows 2019 Citrix Session Host Gold Build

## Environment


- Windows 10 device running a local Ubuntu Windows Subsystem for Linux (WSL): Used to run ansible and terraform scripts
  - WSL can be deployed inside a Win10 or WS2019 management VM hosted on the customer network.  No need for any dedicated infrastructure or licenses to run ansible.

- All windows devices managed remotely using WINRM via https.
  - As long as WINRM is visible, config can be remotely pushed to windows devices without any agents.  These devices can be hosted anywhere, eg: Azure, AWS, onpremise.

- Existing infrastructure for AD DS and file shares.
  - An SMB file share will host all the app installation media
  - Windows session hosts will be joined to an existing active directory domain

- Session Host terraform provisioning.
  - WS 2019 servers deployed into azure
  - Virtual network and peering created so newly provisioned servers can see existing infrastructure
  - Public IPs presented and opened up for:
    - RDP(tcp3389)
    - WINRM(tcp5986)
  - Ansible inventory file auto-generated using terraform template, containing:
    - VM names
    - public IPS
    - admin user
    - auto-generated password

## Ansible Benefits

- Agentless
- Free to use
- Can be easily deployed into a customer environment with minimal prerequisites
- Can be used to remotely configure any windows device that has WINRM enabled.
- Works on prem or in the cloud
- Extensive libraries available
- Flexible

## Preparation

### get WSL up and running on win10 with ubuntu.

### install the ansible module - this should also install pip, python3.
  sudo apt update

  sudo apt install ansible

### check pip,python3,winrm installed.

### Configure ansible for credssp support

  pip install requests-credssp

## Configure ansible managed endpoints

### Set up winrm on the windows ansible hosts

  $url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"

  $file = "$env:temp\ConfigureRemotingForAnsible.ps1"

  (New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)

  powershell.exe -ExecutionPolicy ByPass -File $file

#### For Domain members enable credssp for winrm connections using AD accounts

  Enable-WSManCredSSP -Role Server -Force

#### Test ansible winrm access

  ansible -p win_ping target
  
## Terraform provisioning

  terraform apply

## run the playbooks.

  ansible-playbook ws2019goldbuild.yaml -i azterraform/inventory.yaml
  
  
