# Manage_Windows_Servers_with_Ansible

## WinRM Configuration
When connecting to a Windows server, there are a couple of different options that can be used for authentication.
o	We will use Basic authentication because it support HTTP(5985 port).

```
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file
```
•	If third step is not working then Download the url file” https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1” on to the target machine.
•	And run the 4th point by specifying the path of the file using 2nd point. And Install it.
•	Verify that the Basic authentication is enabled.

```
winrm get winrm/config/Service
winrm get winrm/config/Winrs
```

## Installation

Ansible uses the pywinrm package to communicate with Windows servers over WinRM. At the time of writing this, the package is not installed by default with the Ansible package. Install it manually (I’m using a Debian-based system here):

```
$ sudo apt install python3-pip
$ pip3 install pywinrm
```

## Ansible Configuration

In the inventory file like hosts,

```
$ sudo nano /etc/ansible/hosts
[windows]
Windows.domain.com
10.x.x.x
```

The content of the file group_vars/windows can be seen below.

```
ansible_connection: winrm
ansible_user: ansible
ansible_password: PUT_YOUR_WINDOWS_PASSWORD_HERE
ansible_winrm_transport: ntlm
ansible_port: 5985
ansible_winrm_scheme: http
ansible_winrm_server_cert_validation: ignore
```

Testing using win_ping module

```
$ ansible windows -i hosts -m win_ping 
```

## Troubleshoot

If service is not pingable, do degrade the packages. Then check ping operation, go to Testing using win_ping module section.

•	pywinrm from 0.4.2 to 0.4.1
•	requests from 2.26.0 to 2.24.0
•	pyOpenSSL from 21.0.0 to 19.1.0
•	requests from 1.2.0 to 1.1.1
