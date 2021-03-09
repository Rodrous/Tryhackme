# Anthem

- Recon Stage
    - Nmap

        ![Anthem%20398efc4338574f2e852919b387a51189/Untitled.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled.png)

        ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%201.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%201.png)

    - Dirsearch

        ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%202.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%202.png)

- Website Analysis

    ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%203.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%203.png)

    1. Robots.txt

    ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%204.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%204.png)

    ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%205.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%205.png)

    it was written by

    ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%206.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%206.png)

    we know that its a `umbraco` cms.

    and from crawling through website we know the mail account is [Redacted]@anthem.com

- Logging in CMS

    ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%207.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%207.png)

- Access to Machine

    We are going to access the machine using the username and password we gained from above questions and we will use [RDP](https://www.notion.so/RDP-9af7b35383814de58dec025fba0a6594) for that.

    ![Anthem%20398efc4338574f2e852919b387a51189/add_this_to_notion_Anthem.png](Anthem%20398efc4338574f2e852919b387a51189/add_this_to_notion_Anthem.png)

    - getting usertext

        ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%208.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%208.png)

- Escalation
    - getting admin pass

        From hint i found out there is some hidden directory so i looked it up

        ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%209.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%209.png)

        ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%2010.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%2010.png)

        So we cannot access restore.txt, lets change the perms.

        we can use `icalcs` to do that

        - How do i came across it?

            ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%2011.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%2011.png)

        Command: `ICACLS * /T /Q /C /RESET`

        it will reset perms for all files folders and subfolders

        ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%2012.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%2012.png)

        Now accessing the file.

        ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%2013.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%2013.png)

    ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%2014.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%2014.png)

    ![Anthem%20398efc4338574f2e852919b387a51189/Untitled%2015.png](Anthem%20398efc4338574f2e852919b387a51189/Untitled%2015.png)

- Flags

    THM{F1ND_1T_Y0uRS3L7}