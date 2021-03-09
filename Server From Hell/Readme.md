# The Server From Hell

- Enumeration
    - Using Threader3000

        ![The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled.png](The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled.png)

        we know hella ports are open

        From the hint we get  to know to start from `port 1337`

        - Using nmap on `port 1337`

            ![The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%201.png](The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%201.png)

            it says troll is hiding in first 100 ports, so making a small script would help us get that

            ```python
            import os,sys
            hell = '{ip}'
            try:
                 for i in range(100):
            	print(os.system(f"telnet {hell} {i}"))
            except Exception:
                 pass
            ```

            Running this we got a hint

            ![The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%202.png](The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%202.png)

            - Now getting to that port

            ![The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%203.png](The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%203.png)

            We got a hint of the nfs share

- NFS Share

    Use [Nfs Enumeration](https://www.notion.so/Enumeration-81b73bf2116240699ede5b5847811ab4) and mount the share

    From there youll get a zip file, convert that zip to hash using `zip2john` and crack the hash to get hold of the files.

- Foothold

    There will be a ssh-rsa file in the zip, use that to get hold of the machine

    In one of the files, its said  that the ssh port is somewhere between 2500 and 4500

    so a simple code to get that port.

    ```python
    import os
    print(os.system("echo ~"))
    try:
        for i in range(2500,4500):
            os.system(f"ssh hades@10.10.170.213 -i id_rsa -p {i}")
    	
    except Exception:
        pass
    ```

    ![The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%204.png](The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%204.png)

    Instead of a common shell, we got something `irb(main):001:0>` which is something related to ruby.
    you can execute ``system('/bin/bash')`` to get a shell

    ![The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%205.png](The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%205.png)

- Escalation

    From hint i got to know that i use  `getcap` to find binaries with capabilities 

    - What is getcap?

        Using getcap we can find the root capabilities.

        - What are capabilities?

            Linux capabilities are special attributes in the Linux kernel that grant processes and binary executables specific privileges that are normally reserved for processes whose effective user ID is 0 (The root user, and only the root user, has UID 0).

    ```bash
    getcap / -r 2>>/dev/null
    ```

    ![The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%206.png](The%20Server%20From%20Hell%203624f94bda8f4858ade4de32209e9097/Untitled%206.png)

    we got to know that we can exploit tar capability

    Now use tar to zip `/etc/shadow` and get the root password 

    or

    zip '/root' folder to get  the flag