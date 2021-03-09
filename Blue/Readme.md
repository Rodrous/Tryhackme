# Blue

- Recon
    - Threader3000 Scan

        ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled.png)

    - Nmap Scans

        ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%201.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%201.png)

        ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%202.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%202.png)

    - Vulnerability Scan

        ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%203.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%203.png)

- Enumeration

    I saw an SMB Port so, i started an `enum4linux` scan.

    ```
    Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sun Dec  6 21:46:13 2020

     ========================== 
    |    Target Information    |
     ========================== 
    Target ........... 10.10.24.133
    RID Range ........ 500-550,1000-1050
    Username ......... ''
    Password ......... ''
    Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none

     ==================================================== 
    |    Enumerating Workgroup/Domain on 10.10.24.133    |
     ==================================================== 
    [+] Got domain/workgroup name: WORKGROUP

     ============================================ 
    |    Nbtstat Information for 10.10.24.133    |
     ============================================ 
    Looking up status of 10.10.24.133
    	JON-PC          <00> -         B <ACTIVE>  Workstation Service
    	WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
    	JON-PC          <20> -         B <ACTIVE>  File Server Service
    	WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections
    	WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
    	..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser

    	MAC Address = 02-EF-88-56-BC-4D

     ===================================== 
    |    Session Check on 10.10.24.133    |
     ===================================== 
    [+] Server 10.10.24.133 allows sessions using username '', password ''

     =========================================== 
    |    Getting domain SID for 10.10.24.133    |
     =========================================== 
    Could not initialise lsarpc. Error was NT_STATUS_ACCESS_DENIED
    [+] Can't determine if host is part of domain or part of a workgroup

     ====================================== 
    |    OS information on 10.10.24.133    |
     ====================================== 
    [+] Got OS info for 10.10.24.133 from smbclient: 
    [+] Got OS info for 10.10.24.133 from srvinfo:
    Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED

     ============================= 
    |    Users on 10.10.24.133    |
     ============================= 
    [E] Couldn't find users using querydispinfo: NT_STATUS_ACCESS_DENIED

    [E] Couldn't find users using enumdomusers: NT_STATUS_ACCESS_DENIED

     ========================================= 
    |    Share Enumeration on 10.10.24.133    |
     ========================================= 
    do_connect: Connection to 10.10.24.133 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)

    	Sharename       Type      Comment
    	---------       ----      -------
    Reconnecting with SMB1 for workgroup listing.
    Unable to connect with SMB1 -- no workgroup available

    [+] Attempting to map shares on 10.10.24.133

     ==================================================== 
    |    Password Policy Information for 10.10.24.133    |
     ==================================================== 
    [E] Unexpected error from polenum:
    Traceback (most recent call last):
      File "/usr/bin/polenum", line 16, in <module>
        from impacket.dcerpc.v5.rpcrt import DCERPC_v5
      File "/usr/local/lib/python3.8/dist-packages/impacket/dcerpc/v5/rpcrt.py", line 122
        0x00000005L : 'rpc_s_access_denied',
                  ^
    SyntaxError: invalid syntax
    [E] Failed to get password policy with rpcclient

     ============================== 
    |    Groups on 10.10.24.133    |
     ============================== 

    [+] Getting builtin groups:

    [+] Getting builtin group memberships:

    [+] Getting local groups:

    [+] Getting local group memberships:

    [+] Getting domain groups:

    [+] Getting domain group memberships:

     ======================================================================= 
    |    Users on 10.10.24.133 via RID cycling (RIDS: 500-550,1000-1050)    |
     ======================================================================= 
    [E] Couldn't get SID: NT_STATUS_ACCESS_DENIED.  RID cycling not possible.

     ============================================= 
    |    Getting printer info for 10.10.24.133    |
     ============================================= 
    Could not initialise spoolss. Error was NT_STATUS_ACCESS_DENIED

    enum4linux complete on Sun Dec  6 21:46:33 2020
    ```

    But as we can see, I didn't got any relevant information out of this, but we saw that there exist a CVE to which this SMB is vulnerable to.

    Using Searchsploit to look for that exploit

    ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%204.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%204.png)

    This exploit also exists in msfconsole, so we're gonna use that.

- Eternal Blue Explanation

    EternalBlue is an exploit that allows cyber threat actors to remotely execute arbitrary code and gain access to a network by sending specially crafted packets. 

    It exploits a software vulnerability in Microsoft’s Windows operating systems (OS) Server Message Block(SMB) version 1 (SMBv1) protocol, a network file sharing protocol that allows access to files on a remote server. 

    This exploit potentially allows cyber threat actors to compromise the entire network and all devices connected to it. Due to EternalBlue’s ability to compromise networks, if one device is infected by malware via EternalBlue, every device connected to the network is at risk. This makes recovery difficult, as all devices on a network may have to be taken offline for remediation. This vulnerability was patched and is listed on Microsoft’s security bulletin as MS17-010

    [Blue%208ec273ee118b42b09beb84834720f3e9/Security-Primer-EternalBlue.pdf](Blue%208ec273ee118b42b09beb84834720f3e9/Security-Primer-EternalBlue.pdf)

- Initial Foothold and Escalation

    This is from rapid7's website

    ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%205.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%205.png)

    ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%206.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%206.png)

    We got the shell 🙂

    ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%207.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%207.png)

    we are `nt authority\system` but that doesnt mean that our process is also `nt authority`,

    so we have to migrate in this situation

    - Why Migrate?

        If you launched a meterpreter payload that doesn't match the target `(windows/meterperter/reverse_tcp vesrus windows/x64/meterpreter/reverse_tcp)` you'll have to migrate processes to get an x64 process.

    - List Processes

    ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%208.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%208.png)

    Find a process towards the bottom of this list that is running at NT AUTHORITY\SYSTEM and write down the process id (far left column).

    ![Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%209.png](Blue%208ec273ee118b42b09beb84834720f3e9/Untitled%209.png)