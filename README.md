# ansiblegoldbuild

## get WSL up and running on win10 with ubuntu.

## install the ansible module - this should also install pip, python3.
sudo apt update

sudo apt install ansible

## check pip,python3,winrm installed.

## install the citrix ADC ansible modules

ansible-galaxy collection install git+https://github.com/citrix/citrix-adc-ansible-modules.git#/ansible-collections/adc

ansible-galaxy collection build

ansible-galaxy collection install citrix-adc-1.1.0.tar.gz

pip3 install nitro-python-1.0_kamet.tar.gz


## set up the netscaler ansible hosts

## Set up winrm on the windows ansible hosts


$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"

$file = "$env:temp\ConfigureRemotingForAnsible.ps1"

(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)

powershell.exe -ExecutionPolicy ByPass -File $file

### For Domain members enable credssp for winrm connections using AD accounts

Enable-WSManCredSSP -Role Server -Force

## Configure ansible for credssp support

pip install requests-credssp

## download and run the playbooks.

git pull https://github.com/tonytregcdw/ansiblegoldbuild

