# Overpassed 2

- Forensics

    Looking at the pcap file and following the TCP stream, found this

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled.png)

    Got some password hashes in the same file, cracking them using john

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%201.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%201.png)

- Initial Foothold

    Crack the backdoor hash in pcap

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%202.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%202.png)

    use this hash and add salt from the original [code](https://github.com/NinjaJc01/ssh-backdoor/blob/master/main.go)

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%203.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%203.png)

    Cracking with hashcat

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%204.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%204.png)

    Used the backdoor to get in system

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%205.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%205.png)

- Priv Esc

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%206.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%206.png)

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%207.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%207.png)

    ![Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%208.png](Overpassed%202%2021d8ec8fb3434d0892ca8be51e520553/Untitled%208.png)