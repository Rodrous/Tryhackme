# Undiscovered

First add the ip in |/etc/hosts|

![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled.png)

- Recon Stage
    1. NMAP

    ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%201.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%201.png)

    2. Gobuster

    Here i had to do, 2 tests.

    - Directory Bust

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%202.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%202.png)

    - DNS Bust

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%203.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%203.png)

        Looking at the subdomains, `grep` all with status 200 

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%204.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%204.png)

- Initial Foothold
    - Exploit-Db

        Found this on the first subdomain

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%205.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%205.png)

        Looking for exploits in Exploit-db

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%206.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%206.png)

        Got this 

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%207.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%207.png)

    - bruteforcing

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%208.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%208.png)

    - Getting-shell

        Following the Exploit-db instructions, got the shell

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%209.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%209.png)

        Dont have perms to open Willaim's folder

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2010.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2010.png)

    Looking back at the [NFS share]() 

    - NFS

        lets check `etc/exports` first.

        - What information does etc/exports have?
            - The /etc/exports file controls which file systems are exported to remote hosts and specifies options.
            - Blank lines are ignored in here
            - Each exported file system should be on its own individual line, and any lists of authorized hosts placed after an exported file system must be separated by space characters.

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2011.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2011.png)

        mounting `home/william`

    ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2012.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2012.png)

    ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2013.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2013.png)

    to use this, apparently we have to make a user with same perms as Willaim

    ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2014.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2014.png)

    ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2015.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2015.png)

    to make this accessible through www user, added `777` perms

    - chmod table

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2016.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2016.png)

    ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2017.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2017.png)

    - Loggin in Leonard

        Theres a script in williams folder

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2018.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2018.png)

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2019.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2019.png)

        This script apparently searches for a file in leonard's folder, from this we can grab leonards `id_rsa` key and login via ssh.

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2020.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2020.png)

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2021.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2021.png)

- Getting Root
    1. Checking for suids

        Going to gtfobins and searching for vim enumeration

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2022.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2022.png)

        this didnt worked so searched on viminfo

        viminfo contains the history of vim commands 

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2023.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2023.png)

        Looks like a reverse shell command

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2024.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2024.png)

        So used it go get a root shell

        ![Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2025.png](Undiscovered%2068a5aff6aae84d99896b57ce60b3612b/Untitled%2025.png)