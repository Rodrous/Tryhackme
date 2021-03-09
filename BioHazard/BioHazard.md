# BioHazard

- Initial Recon

    Nmap

    ```
    # Nmap 7.91 scan initiated Fri Jan  1 05:45:23 2021 as: nmap -p21,22,80 -sV -sC -T4 -Pn -oA 10.10.255.244 10.10.255.244
    Nmap scan report for 10.10.255.244
    Host is up (0.16s latency).

    PORT   STATE SERVICE VERSION
    21/tcp open  ftp     vsftpd 3.0.3
    22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 c9:03:aa:aa:ea:a9:f1:f4:09:79:c0:47:41:16:f1:9b (RSA)
    |   256 2e:1d:83:11:65:03:b4:78:e9:6d:94:d1:3b:db:f4:d6 (ECDSA)
    |_  256 91:3d:e4:4f:ab:aa:e2:9e:44:af:d3:57:86:70:bc:39 (ED25519)
    80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
    |_http-server-header: Apache/2.4.29 (Ubuntu)
    |_http-title: Beginning of the end
    Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    # Nmap done at Fri Jan  1 05:45:36 2021 -- 1 IP address (1 host up) scanned in 13.16 seconds
    ```

    Nikto

    ```
    - Nikto v2.1.6/2.1.5
    + Target Host: 10.10.255.244
    + Target Port: 80
    + GET The anti-clickjacking X-Frame-Options header is not present.
    + GET The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
    + GET The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
    + GET Server may leak inodes via ETags, header found with file /, inode: 2b4, size: 592e838050f26, mtime: gzip
    + HEAD Apache/2.4.29 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
    + OPTIONS Allowed HTTP Methods: GET, POST, OPTIONS, HEAD 
    + OSVDB-3268: GET /css/: Directory indexing found.
    + OSVDB-3092: GET /css/: This might be interesting...
    + OSVDB-3268: GET /images/: Directory indexing found.
    + OSVDB-3233: GET /icons/README: Apache default file found.
    ```

- Enumeration

    ```
    10.10.202.251 subfolders:
    /mainmansion/ -> /diningroom/ 
    /diningroom/ -> base64 file{<!-- SG93IGFib3V0IHRoZSAvdGVhUm9vbS8= -->} -> 'How about /tearoom/' | /diningroom/emblemslot/ | secret_sheild_key.html
    /emblem.php -> emblem{fec832623ea498e20bf4fe1821d58727}
    /teaRoom/ -> /masterofunlock/ -> Flag and suggestion to move to /artRoom/
    /artRoom/ -> mansionmap.html
    /barRoom/ -> /musicalnote.html -> NV2XG2LDL5ZWQZLFOR5TGNRSMQ3TEZDFMFTDMNLGGVRGIYZWGNSGCZLDMU3GCMLGGY3TMZL5 -> music_sheet{flag} | /secrtetbaroom/ -> gold_emblem_flag -> vig_cipher -> 
    /diningRoom2F/ -> ceaser_cipher -> diningroom/sapphire.html -> Flag
    /armorRoom/ -> Crest3
    /galleryRoom/ -> Note.txt -> Crest2
    /attic -> Crest4
    /studyRoom/ -> Examine
    ```

    ```
    Info on crests:
    The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
    Crest3: c3M6IHlvdV9jYW50X2h
    Crest2: h1bnRlciwgRlRQIHBh
    Crest4: pZGVfZm9yZXZlcg==
    Crest1: RlRQIHVzZXI6IG

    FTP Creds = Crest1+Crest2+Crest3+Crest4 | base64 -d
    ```

- FTP

    Downloaded all files in FTP

    ![BioHazard%20ac89a379dafc480685bbf0a4fd5e5ce0/Untitled.png](BioHazard%20ac89a379dafc480685bbf0a4fd5e5ce0/Untitled.png)

    - Further Enumeration

        Extracted `001-key.jpg` using Steghide to get key 1, used `Strings` to get key 2, used `binwalk-e`  to get key 3.

        Thus `key1 + key2 + key3  | base64 -d`

        Found a secret_folder, entered one of the found flags and got 2 Files: 

        - One of the file contained a ssh password for a user and other contained, some encrypted text.
            - The encryption was vignere cipher, we can use this to decode:

                [Vigenère Cipher (automatic solver) | Boxentriq](https://www.boxentriq.com/code-breaking/vigenere-cipher)

        We missed out one of the folders: `/Studyroom` going back at it again, and we got user.

- SSH

    username: `umbrella_guest`

    password: `T_virus_rules`

    Now all we have to do is enumerate.

    - we can find hidden folders using `ls -la`
    - there is a folder called .prision_*, in that we got information of what happened and a possible key to decrypt the cipher we got in one of the webfolder.
    - Further looking in `weasker` folder we found some the story on what actually happened.

While looking in Folders and spending a few minutes in `umbrella` i remembered that i already have a password ( which i decrypted[ in /secretfolder]), so just used that to upgrade my shell to `weasker`.

- getting root.txt

    this user have access to all commands, we can just `sudo su` and get root.

 

---

![BioHazard%20ac89a379dafc480685bbf0a4fd5e5ce0/Untitled%201.png](BioHazard%20ac89a379dafc480685bbf0a4fd5e5ce0/Untitled%201.png)

---

Live Notes:

```
# Nmap 7.91 scan initiated Fri Jan  1 05:45:23 2021 as: nmap -p21,22,80 -sV -sC -T4 -Pn -oA 10.10.255.244 10.10.255.244
Nmap scan report for 10.10.255.244
Host is up (0.16s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c9:03:aa:aa:ea:a9:f1:f4:09:79:c0:47:41:16:f1:9b (RSA)
|   256 2e:1d:83:11:65:03:b4:78:e9:6d:94:d1:3b:db:f4:d6 (ECDSA)
|_  256 91:3d:e4:4f:ab:aa:e2:9e:44:af:d3:57:86:70:bc:39 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Beginning of the end
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Jan  1 05:45:36 2021 -- 1 IP address (1 host up) scanned in 13.16 seconds
|------------------------------------------------------------------------------------------------------------------------------------------|
- Nikto v2.1.6/2.1.5
+ Target Host: 10.10.255.244
+ Target Port: 80
+ GET The anti-clickjacking X-Frame-Options header is not present.
+ GET The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ GET The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ GET Server may leak inodes via ETags, header found with file /, inode: 2b4, size: 592e838050f26, mtime: gzip
+ HEAD Apache/2.4.29 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ OPTIONS Allowed HTTP Methods: GET, POST, OPTIONS, HEAD 
+ OSVDB-3268: GET /css/: Directory indexing found.
+ OSVDB-3092: GET /css/: This might be interesting...
+ OSVDB-3268: GET /images/: Directory indexing found.
+ OSVDB-3233: GET /icons/README: Apache default file found.
|------------------------------------------------------------------------------------------------------------------------------------------|
10.10.202.251 subfolders:
/mainmansion/ -> /diningroom/ 
/diningroom/ -> base64 file{<!-- SG93IGFib3V0IHRoZSAvdGVhUm9vbS8= -->} -> 'How about /tearoom/' | /diningroom/emblemslot/ | secret_sheild_key.html
/emblem.php -> emblem{fec832623ea498e20bf4fe1821d58727}
/teaRoom/ -> /masterofunlock/ -> Flag and suggestion to move to /artRoom/
/artRoom/ -> mansionmap.html
/barRoom/ -> /musicalnote.html -> NV2XG2LDL5ZWQZLFOR5TGNRSMQ3TEZDFMFTDMNLGGVRGIYZWGNSGCZLDMU3GCMLGGY3TMZL5 -> music_sheet{362d72deaf65f5bdc63daece6a1f676e} | /secrtetbaroom/ -> gold_emblem_flag -> vig_cipher -> 
/diningRoom2F/ -> ceaser_cipher -> diningroom/sapphire.html -> Flag
/armorRoom/ -> Crest3
/galleryRoom/ -> Note.txt -> Crest2
/attic -> Crest4
/studyRoom/ -> Examine 

|------------------------------------------------------------------------------------------------------------------------------------------|
Some Info in the url
http://10.10.202.251/barRoom357162e3db904857963e6e0b64b96ba7/
|------------------------------------------------------------------------------------------------------------------------------------------|
Info on crests:
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
Crest3: c3M6IHlvdV9jYW50X2h
Crest2: h1bnRlciwgRlRQIHBh
Crest4: pZGVfZm9yZXZlcg==
Crest1: RlRQIHVzZXI6IG
|------------------------------------------------------------------------------------------------------------------------------------------|
Final:
RlRQIHVzZXI6IGh1bnRlciwgRlRQIHBhc3M6IHlvdV9jYW50X2hpZGVfZm9yZXZlcg==

FTP_Creds:
FTP user: hunter, FTP pass: you_cant_hide_forever
|------------------------------------------------------------------------------------------------------------------------------------------|
Key1: cGxhbnQ0Ml9jYW
Key2: 5fYmVfZGVzdHJveV9
Key3: aXRoX3Zqb2x0

Helmetkey = Key1 + Key2 + Key3 | base64 -d
|------------------------------------------------------------------------------------------------------------------------------------------|
Secret_Folder: /hidden_closet/
|------------------------------------------------------------------------------------------------------------------------------------------|
ReadFile= wpbwbxr wpkzg pltwnhro, txrks_xfqsxrd_bvv_fy_rvmexa_ajk | weasker login password, stars_members_are_my_guinea_pig
SSH password: T_virus_rules
SSH uswe: umbrella_guest
|------------------------------------------------------------------------------------------------------------------------------------------|
hunter: FTP 
umbrella: /.jailcell/chris.txt
	MO disk 2: albert
weasker: weasker_note/ {status: Dead}
	 /Desktop: Empty
```