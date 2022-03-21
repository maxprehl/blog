# Making Windows a git server

all 9 circles of hell

-   https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_server_configuration
-   https://github.com/PowerShell/Win32-OpenSSH/wiki/Setting-up-a-Git-server-on-Windows-using-Git-for-Windows-and-Win32_OpenSSH
-

## Limbo: ssh**d**

-   https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse

Install

> Open Settings, select Apps > Apps & Features, then select Optional Features.
>
> Scan the list to see if the OpenSSH is already installed. If not, at the top
> of the page, select Add a feature, then:
>
> -   Find OpenSSH Client, then click Install
> -   Find OpenSSH Server, then click Install

```ps
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'

# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Start as service (daemon/init)

```ps
# Start the sshd service
Start-Service sshd

# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'
```

Firewall

```ps
# Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

## Lust: sshd\_**config**

ProgramData

## Gluttony: ssh **authorized_keys**

-   https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement#administrative-user

Administrators

## Greed: ssh default shell

-   https://github.com/PowerShell/Win32-OpenSSH/wiki/DefaultShell

```
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

## Anger:

## Heresy:

## Violence: git-upload-pack

aka `git/mingw64/bin`

```
'git-upload-pack' is not recognized as an internal or external command,
```

## Fraud: default shell

`fatal: ''/myrepo.git'' does not appear to be a git repository`

## Treachery: C:\full\paths
