# Installation (in person or over Teams)

We have to disable Hyper-V (incompatible with VMware), in elevated powershell:

    Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor
    bcdedit /set hypervisorlaunchtype off

A restart might be required after these commands.

Start the VMware installation, in an elevated powershell:

    cd c:/windows/temp
    c:/windows/system32/curl.exe -k -L -o vmware-install.exe https://www.vmware.com/go/getplayer-win
    curl -o vmnetcfg.exe https://saccdnl1rcdm001.blob.core.windows.net/files/vmnetcfg.exe
    ./vmware-install.exe

During installation:

-   if it asks for a restart, do it
-   default install location is good, enable "Enhanced Keyboard Driver"
-   no product update check, no experience program
-   no desktop icon, but yes to start menu icon
-   no need to enter license
-   restart after install

Setting up networking is the next thing to do. VMware automatically
allocates a 192.168.x.0/24 subnet for virtual machines, where x is
randomly selected on installation. We have to change this to the
standard 192.168.11.0/24, so we can share scripts and command line
snippets with each other without taking care of this randomness.

In an elevated powershell:

    cd c:/windows/temp
    $Env:Path += ";c:\Program Files (x86)\VMware\VMware Player\"
    ./vmnetcfg.exe

In this graphical tool make the following changes on the `vmnet8`
interface:

-   subnet ip: 192.168.11.0 / 255.255.255.0
-   uncheck "Use local DHCP service to distribute IP address to VMs"
-   Click 'NAT Settings...`. Inside there, change gateway ip to: 192.168.11.2

With this, the VMware installation is finished, no sysadmin rights are
needed anymore.
