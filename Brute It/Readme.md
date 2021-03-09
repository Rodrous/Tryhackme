# Brute It

- Recon Stage

    Nmap:

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled.png)

    Gobuster:

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%201.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%201.png)

- Bruteforcing

    [Hydra](https://www.notion.so/Hydra-3d12597583404ce1a2784e572f9ae36f)

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%202.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%202.png)

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%203.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%203.png)

    We got the RSA private key!!

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%204.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%204.png)

- Decrypting Private Key and Logging in

    Using SSH2John to get hash of the Encrypted RSA key

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%205.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%205.png)

    Now use john to crack the hash

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%206.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%206.png)

- Initial Foothold and Escalation

    ```jsx
    chmod 600 rsa_key
    ```

    First give perms to rsa_key 

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%207.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%207.png)

    After Executing Linpeas, its clear that /bin/cat can be used as super user, now time to [exploit this](https://gtfobins.github.io/gtfobins/cat/#file-read)

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%208.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%208.png)

    Now crack the root hash and login as ssh

- Other way to get root flag

    ![Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%209.png](Brute%20It%202107de5f648f4284904b31e6704e5bcd/Untitled%209.png)