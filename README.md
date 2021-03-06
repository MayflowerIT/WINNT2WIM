# WINNT.wim
The goal of this project is to script the conversion of a Windows XP (or NT5? NT4?) installer ISO to a reusable WIM. Since we can't distribute the ISO, and the ISO is severely limited in utility for any scripted/managed environment, getting a workable WIM needs to be scripted or it'll always be one-off and fragile. 

## Stages
0. Install Hyper-V
1. Prepare source files for automation:
    - Create WINNT.SIF from template using site-specific variables.
    - unattend.xml
    - Create a floppy image with WINNT.SIF and AutoUnattend.xml.
    - Create VM (generation 1) with VHDX, two CD drives, and a floppy drive.
        - A: Floppy
        - C: VHDX
        - D: WinPE
        - E: WINNT.ISO
    - Run the VM, wait for it to shut down.
    - Mount the VHDX
    - Copy source?
    - Capture the WIM
3. Boot WinPE environment (pre-10, which changes something about volumes that [`WINNT32.exe`] can't deal with)

## notes
### [XP to WIM]( https://msfn.org/board/topic/121046-xp-to-wim/?do=findComment&comment=954076 )
#### Easy WinXP CD Install to WIM Conversion

> This basically encapsulates the standard windows xp install method inside a wim file.
>
> Because it will install completely from the hard drive the installation time is decreased significantly.
>
> 1. Boot to WinPE 2 or higher
>
> 2. Setup a regular drive or partition using `diskpart`
>
> 3. 
> - Use Standard Windows XP CD
> - Run `winnt32.exe /syspart:c: /makelocalsource`
>     (where c is the letter of the drive)
> - enter product key etc
>
> 4. use imagex to capture the drive
>
> 5. DEPLOYMENT
> - boot to WinPE 2 or higher
> - set up partition or drive using diskpart (be sure to assign a drive letter and make it active.. etc)
> - `bootsect /nt52 /force /mbr`
> - `imagex /apply xpinst.wim 1 c:`
>
> 6. reboot and run though text mode and gui setup as normal
>
> Brought to you by [Binary Outcast]( http://binaryoutcast.com/ )

### [`WinNT32.EXE`]( https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/winnt32 )
- `/makelocalsource`	Instructs setup to copy all installation source files to your local hard disk. Use /makelocalsource when installing from a cd to provide installation files when the cd is not available later in the installation.
- `/unattend`	On an x86-based computer, upgrades your previous version of Windows NT 4.0 Server (with Service Pack 5 or later) or Windows 2000 in unattended setup mode. All user settings are taken from the previous installation, so no user intervention is required during setup.

### [Installing Windows XP from a network. RIS, but not Microsoft]( http://unattendedxp.com/en/articles/installing-windows-xp-from-network/ )
> We copy the contents of the catalog AMD64 into I386. Now the folder AMD64 is not necessary to us, and we have to delete it. [...] `junction C:\NETBOOT\WINXP\INSTALL\AMD64 C:\NETBOOT\WINXP\INSTALL\I386` [...] As a result, a catalog INSTALL gets a folder AMD64 with the identical contents as in I386, but at the same time the size of the catalog INSTALL stays the same.

...but make sure 5.1-with-5.1 and 5.2-with-5.2...

### Slipstream updates
- `WindowsXP-KB936929-SP3-x86-ENU.exe` __/integrate:drive\path__

### [WinNT.SIF]( https://paradice.board-directory.net/t3-winnt-sif-lists-nearly-all-possible-settings-for-unattended )
### [WinNT.SIF]( https://www.svrops.com/svrops/documents/xpunattend.htm#xpinstall )

## example
https://archive.org/details/xppro2020
