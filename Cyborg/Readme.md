# Cyborg

- Recon

    The Most Important Phase in any Pentest is Recon, you have to know about what you are facing.

    - Threader3000 Scan

        ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled.png)

    - Nmap Scan

        ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%201.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%201.png)

        So we have 2 ports with ssh and a apache service running.

        - Search for Available service Exploit:

            OpenSSH

            ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%202.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%202.png)

            We have a username enumeration exploit but not RCE, we can try guessing the username and then brutefore for password but `BruteForcing` must be a last resort so lets this bit for a while.

            Apache

            ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%203.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%203.png)

            Some of the exploits here give an RCE but we have to see if the backend is PHP or not.

            Lets move on to Enumeration Phase.

- Enumeration

    Lets see whats in the webserver

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%204.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%204.png)

    Looks like a default Apache Page.

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%205.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%205.png)

    And Source is not really giving much info, lets enumerate the Directories.

    - GoBuster

        ```go
        gobuster dir -u 10.10.202.186 -w /usr/share/dirb/wordlists/common.txt -s 200,204,301,302,307 -x php,txt
        ===============================================================
        Gobuster v3.0.1
        by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
        ===============================================================
        [+] Url:            http://10.10.202.186
        [+] Threads:        10
        [+] Wordlist:       /usr/share/dirb/wordlists/common.txt
        [+] Status codes:   200,204,301,302,307
        [+] User Agent:     gobuster/3.0.1
        [+] Extensions:     php,txt
        [+] Timeout:        10s
        ===============================================================
        2021/02/03 08:25:42 Starting gobuster
        ===============================================================
        /admin (Status: 301)
        /etc (Status: 301)
        /index.html (Status: 200)
        ===============================================================
        2021/02/03 08:25:44 Finished
        ===============================================================
        ```

        ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%206.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%206.png)

        ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%207.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%207.png)

        Intresting Files, let open it

        ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%208.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%208.png)

        ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%209.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%209.png)

        We got a hash of somekind, lets note that and Move on further enumerating the website.

    ```go
    ip/admin 
    ```

    Notice that we are already Logged as a Admin, it might be a case of login-bypass or website is made that way. Either way good for us.

    Well here is an interesting Chat

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2010.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2010.png)

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2011.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2011.png)

    From `admin/archive.tar` we got a tar file, lets see whats inside.

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2012.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2012.png)

    Apparently this is borg Backup.

- Getting Access to the machine

    We know that file is a borg backup and borg docs are linked to the Readme File.

    We first have to Install borg`apt install borgbackup`

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2013.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2013.png)

    Wait its asking for a password?, we have a hash from `/etc` folder, lets see what kind of hash is that and crack it and try it here.

    - Am gonna use `name-that-hash-tool` from `bee-san` (real cool person), they also have a nth server running on their [website.](https://nth.skerritt.blog/)

        Here is a link to GitHub repo if y'all are interested (it have a CLI version too).

        [HashPals/Name-That-Hash](https://github.com/HashPals/Name-That-Hash.git)

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2014.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2014.png)

    The hash type is MD5(APR), lets crack it with hashcat.

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2015.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2015.png)

    lets use this password on the archive.

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2016.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2016.png)

    we got a Home Directory :)

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2017.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2017.png)

    we got a SSH user and Pass.

- Priv Esc

    `sudo -l`

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2018.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2018.png)

    We can run this file as sudo, interesting

    lets see the perms on this file

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2019.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2019.png)

    Lets add a write perm to it too

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2020.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2020.png)

    Lets add put a reverse shell in the file and start a listener

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2021.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2021.png)

    Running it, as root

    ![Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2022.png](Cyborg%206e4f2e00989348238d127284610896cc/Untitled%2022.png)