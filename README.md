donsible
========
PowerShell tool for creating and provisioning Docker images with Ansible

Motivation
----------
Often one uses extensively Ansible for provisioning the target infrastructure. But when we create Docker images it is quite often that we repeat the installation steps already available via Ansible roles/playbooks. We do it using plain shell (or PowerShell).

Implementation
--------------
After having looked at different solutions to the task of provisioning Docker containers with Ansible I decided to implement my own. It's implemented in PowerShell.

Quick Example
-------------
```powershell
donsible build -t some-container-name:latest -from ubuntu:18.04 -role `
  @( `
    'openjdk', `
    'dotnet-core', `
    @{ role = 'git:configure'; `
       vars = @{ git_version = '1.8', git_user = 'john.doe', git_email = 'john.doe@mail.com' } })
```

Installation
------------

### Via Install-Module
```powershell
Install-Module donsible
```

### Via source code
```powershell
wget https://github.com/exones/donsible/releases/donsible.<version>.psm1 -OutFile /opt/donsible/donsible.psm1
```

In this installation you will have to use a [dot-source operator](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing) in every PowerShell session like this:
```powershell
. /opt/donsible/donsible.psm1
```

Or, alternatively, use [Import-Module function](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/import-module) with appropriate arguments either from command line or directly from the script:
```powershell
Import-Module /opt/donsible/donsible.psm1

donsible build -t test-container -from ubuntu:18.04 -roles @('python3:install', 'openjdk:install')
```

Global installation for bash
----------------------------
```powershell
donsible install -for bash
```

Usage
-----

Config file
-----------
```json
{
  "roles": {
    "git:install": {
      "vars": {
        "git_version" : "3.1"
      }
    }
  }
}
```

Supported host OS
-----------------
* Windows with PowerShell 5+
* Debian-like with PowerShell Core 1.0+
* RedHat-like with PowerShell Core 1.0+

Supported target OS
-------------------

Contributing
------------
