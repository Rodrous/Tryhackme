# Willow

- Enumeration
    - Threader3000 Scan

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled.png)

    - Nmap Scan

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%201.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%201.png)

    - We got a nfs port open lets [enumerat](https://www.notion.so/Enumeration-81b73bf2116240699ede5b5847811ab4)e it further

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%202.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%202.png)

    - Checking out the website

        its titled as recovery

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%203.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%203.png)

    - Exploring it more on cyberchef

        After too much of trial and error i found that its encoded in hex

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%204.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%204.png)

- Mounting NFS
    - Creating a Dir

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%205.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%205.png)

    - Mounting the share

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%206.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%206.png)

    we got this

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%207.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%207.png)

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%208.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%208.png)

- RSA

    What we see there is the public and private key pair (which shows its an Assymetric Rsa key), which can be used for encryption and decryption of data.

    [if you wanna know more about RSA just go here, its a brilliant [article](https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/) written by Muir]

    We got our encrypted data by exploring the website

    Now getting into Maths,

    Our first question should be How do we encrypt or decrypt given the (key,value) pair.

    we know that we can encrypt the text by using:

    `character^e mod n`

    and decrypt using:

    `encrypter_char^d mod n`

    where `e` is the encryption key, `d` is the decryption key and `n` is product of p and q. [ i would really recommend looking at the post] if you have no idea fuck this means.

    now lets just get into code:

    ```python
    arr = [2367,2367,2367,2367,2367,9709,8600,28638,18410,1735,33029,16186 ....]
    decrypted_text = "".join([chr(i**61527%37627) for i in arr])

    print(decrypted_text)

    with open('private_key', 'w') as f:
        file = f.write(decrypted_text)
    ```

    - Decrypted!!

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%209.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%209.png)

        Its an Encrypted RSA key. Lets just decrypt it now using [John](https://www.notion.so/John-the-Ripper-db1063d54bb74d288cc07463d6afc720)

- Brute Forcing

    we got the encrypted RSA key so, lets just convert it into hash using ssh2john.

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2010.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2010.png)

    now lets get into cracking.

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2011.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2011.png)

    now we've cracked the password lets login as willow

- Initial Foothold

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2012.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2012.png)

- Privilege Escalation

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2013.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2013.png)

    visiting the /dev/ folder

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2014.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2014.png)

    we see that there is one file here which can contain something interesting.

    Also we can access `/bin/mount` as sudo, lets check GTFO bins and see what we can do

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2015.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2015.png)

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2016.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2016.png)

    we got the user and root creds.

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2017.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2017.png)

    Easy right!.

- Getting the Flags

    we know that there is a user.jpg file in willows folder.

    lets try get that.

    - using python http server

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2018.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2018.png)

    its not working, so lets get to our second option, `scp`

    ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2019.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2019.png)

    p.s. u can also use the rsa key we found earlier.. am using password cuz am doing this after finding out the passwords.

    - User Flag

        Its just there.

    - Root Flag.

        its classic Stegnography.

        lets try if we can do something with stegoveritas

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2020.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2020.png)

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2021.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2021.png)

        well it found something.

        lets try steghide.

        using root pass as pass phrase, we did it!!

        ![Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2022.png](Willow%20a33824f4226747e39a4ae89f6e82efc5/Untitled%2022.png)