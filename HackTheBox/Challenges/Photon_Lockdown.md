# Photon Lockdown Write-up

Category: Hardware

 CHALLENGE DESCRIPTION

We've located the adversary's location and must now secure access to their Optical Network Terminal to disable their internet connection. Fortunately, we've obtained a copy of the device's firmware, which is suspected to contain hardcoded credentials. Can you extract the password from it?

    ┌──(kali㉿kali)-[~/Downloads]
    └─$ unzip Photon\ Lockdown.zip 
    Archive:  Photon Lockdown.zip
       creating: ONT/
    [Photon Lockdown.zip] ONT/hw_ver password: 
     extracting: ONT/hw_ver              
      inflating: ONT/rootfs              
     extracting: ONT/fwu_ver             
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads]
    └─$ ls
       ONT  'Photon Lockdown.zip'
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads]
    └─$ cd ONT      
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads/ONT]
    └─$ ls
    fwu_ver  hw_ver  rootfs
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads/ONT]
    └─$ sudo unsquashfs rootfs
    [sudo] password for kali: 
    Parallel unsquashfs: Using 4 processors
    865 inodes (620 blocks) to write
    
    [==========================================================================================================/] 1485/1485 100%
    
    created 440 files
    created 45 directories
    created 187 symlinks
    created 238 devices
    created 0 fifos
    created 0 sockets
    created 0 hardlinks
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads/ONT]
    └─$ cd squashfs-root 
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads/ONT/squashfs-root]
    └─$ ls -l 
    total 48
    drwxrwxr-x 3 root root 4096 Aug  9  2022 bin
    lrwxrwxrwx 1 root root   13 Aug  9  2022 config -> ./var/config/
    drwxrwxr-x 2 root root 4096 Aug  9  2022 dev
    drwxrwxr-x 7 root root 4096 Oct  1 02:48 etc
    drwxrwxr-x 3 root root 4096 Oct  1 02:51 home
    drwxrwxr-x 2 root root 4096 Oct  1 02:53 image
    drwxrwxr-x 6 root root 4096 Aug  9  2022 lib
    lrwxrwxrwx 1 root root    8 Aug  9  2022 mnt -> /var/mnt
    drwxrwxr-x 2 root root 4096 Aug  9  2022 overlay
    drwxrwxr-x 2 root root 4096 Aug  9  2022 proc
    drwxrwxr-x 2 root root 4096 Aug  9  2022 run
    lrwxrwxrwx 1 root root    4 Aug  9  2022 sbin -> /bin
    drwxrwxr-x 2 root root 4096 Aug  9  2022 sys
    lrwxrwxrwx 1 root root    8 Aug  9  2022 tmp -> /var/tmp
    drwxrwxr-x 3 root root 4096 Aug  9  2022 usr
    drwxrwxr-x 2 root root 4096 Aug  9  2022 var
                                                                                                                                                                                                                                                                                                                                                                                                      
    ┌──(kali㉿kali)-[~/Downloads/ONT/squashfs-root/home]
    └─$ grep --include=*.{txt,conf,xml,php} -rnw '.' -e 'HTB' 2>/dev/null
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads/ONT/squashfs-root/home]
    └─$ ls   
                                                                                                                                                                                                                                               
    ┌──(kali㉿kali)-[~/Downloads/ONT/squashfs-root/home]
    └─$ cd ../               
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads/ONT/squashfs-root]
    └─$ cd etc 
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads/ONT/squashfs-root/etc]
    └─$ ls
    config                 inetd.conf                    omci_ignore_mib_tbl.conf  rc_log_dsp             services
    config_default_hs.xml  init.d                        omci_mib.cfg              rc_reset_dsp           setprmt_reject
    config_default.xml     inittab                       orf                       resolv.conf            shells
    cups                   insdrv.sh                     passwd                    rtk_tr142.sh           simplecfgservice.xml
    default                irf                           ppp                       run_customized_sdk.sh  smb.conf
    dhclient-script        mdev.conf                     profile                   runoam.sh              TZ
    dnsmasq.conf           modules-load.d                protocols                 runomci.sh             version
    ethertypes             multiap.conf                  radvd.conf                runsdk.sh              version_info
    fstab                  omci_custom_opt.conf          ramfs.img                 samba                  wscd.conf
    group                  omci_ignore_mib_tbl_10g.conf  rc_boot_dsp               scripts
                                                                                                                                 
    ┌──(kali㉿kali)-[~/Downloads/ONT/squashfs-root/etc]
    └─$ grep --include=*.{txt,conf,xml,php} -rnw '.' -e 'HTB' 2>/dev/null
    ./config_default.xml:244:<Value Name="SUSER_PASSWORD" Value="HTB{FLAG-REDACTED}"/>
                                                                                                                             

