# Relevant

- Recon Stage
    - Threader3000 to check open ports

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled.png)

    - Running Nmap on these ports

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%201.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%201.png)

    - Port 80 had nothing in it, so running a gobuster scan on port 49663

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%202.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%202.png)

    We can see we have a Microsoft WBT Server, share open `port 3389`, lets enumerate that.

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%203.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%203.png)

- Initial Foothold

    Create a msfvenom Reverse shell

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%204.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%204.png)

    and put it in system using smb share

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%205.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%205.png)

    Use 

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%206.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%206.png)

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%207.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%207.png)

- Escalation
    - Executing `whoami /priv`

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%208.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%208.png)

    We can see there is a `SetImpersonatePrivlege`

    you can read about this [here](https://hunter2.gitbook.io/darthsidious/privilege-escalation/juicy-potato) 

    - You can use something called `PrintSpoofer` which is available [here](https://github.com/dievus/printspoofer).

    now put this exploit in the machine using the SMB share.

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%209.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%209.png)

    Now Run the exploit and go get root

    ![Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%2010.png](Relevant%202ca2f67e3ae54deb89a00394e2349a89/Untitled%2010.png)