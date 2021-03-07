# ansiblegoldbuild

# Configure ansible management instance

## get WSL up and running on win10 with ubuntu.

## install the ansible module - this should also install pip, python3.
sudo apt update

sudo apt install ansible

## check pip,python3,winrm installed.

## Configure ansible for credssp support

pip install requests-credssp

# Configure ansible managed endpoints

## Set up winrm on the windows ansible hosts

$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"

$file = "$env:temp\ConfigureRemotingForAnsible.ps1"

(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)

powershell.exe -ExecutionPolicy ByPass -File $file

### For Domain members enable credssp for winrm connections using AD accounts

Enable-WSManCredSSP -Role Server -Force

### Test ansible winrm access

ansible -p win_ping target

## download and run the playbooks.

git pull https://github.com/gituser/gitrepository
