# Volatility

- Obtaining Memory Samples

    Obtaining a memory capture from machines can be done in numerous ways, however, the easiest method will often vary depending on what you're working with. For example, live machines (turned on) can have their memory captured with one of the following tools:

    ```
    FTK Imager - [Link](https://accessdata.com/product-download/ftk-imager-version-4-2-0)
    Redline - [Link](https://www.fireeye.com/services/freeware/redline.html) *Requires registration but Redline has a very nice GUI
    DumpIt.exe
    win32dd.exe / win64dd.exe - *Has fantastic psexec support, great for IT departments if your EDR solution doesn't support this

    ```

    These tools will typically output a .raw file which contains an image of the system memory. The .raw format is one of the most common memory file types you will see in the wild.

    ![Volatility%2045d991905c424f54834deb7b02c56134/Untitled.png](Volatility%2045d991905c424f54834deb7b02c56134/Untitled.png)

    Offline machines, however, can have their memory pulled relatively easily as long as their drives aren't encrypted. For Windows systems, this can be done via pulling the following file:

    **%SystemDrive%/hiberfil.sys**

    hiberfil.sys, better known as the Windows hibernation file contains a compressed memory image from the previous boot. Microsoft Windows systems use this in order to provide faster boot-up times, however, we can use this file in our case for some memory forensics!

    Things get even more exciting when we start to talk about virtual machines and memory captures. Here's a quick sampling of the memory capture process/file containing a memory image for different virtual machine hypervisors:

    VMware - .vmem file
    Hyper-V - .bin file
    Parallels - .mem file
    VirtualBox - .sav file *This is only a partial memory file. You'll need to dump memory like a normal bare-metal system for this hypervisor

    These files can often be found simply in the data store of the corresponding hypervisor and often can be simply copied without shutting the associated virtual machine off. This allows for virtually zero disturbance to the virtual machine, preserving it's forensic integrity.

- Examining the Patient
    - Figuring out which profile to use

        ```jsx
        volatility -f MEMORY_FILE.raw imageinfo
        ```

        - Running the imageinfo command in Volatility will provide us with a number of profiles we can test with, however, only one will be correct. We can test these profiles using the pslist command, validating our profile selection by the sheer number of returned results. Do this now with the command `volatility -f MEMORY_FILE.raw --profile=PROFILE pslist`.

        ![Volatility%2045d991905c424f54834deb7b02c56134/Untitled%201.png](Volatility%2045d991905c424f54834deb7b02c56134/Untitled%201.png)

        To get the process list we can do

        `volatility -f MemFile.raw --profile=PROFILE netscan`

        - It’s fairly common for malware to attempt to hide itself and the process associated with it. That being said, we can view intentionally hidden processes via the command `psxview`.

        ![Volatility%2045d991905c424f54834deb7b02c56134/Untitled%202.png](Volatility%2045d991905c424f54834deb7b02c56134/Untitled%202.png)

        - In addition to viewing hidden processes via psxview, we can also check this with a greater focus via the command 'ldrmodules'. Three columns will appear here in the middle, InLoad, InInit, InMem. If any of these are false, that module has likely been injected which is a really bad thing. On a normal system the grep statement above should return no output.

            ![Volatility%2045d991905c424f54834deb7b02c56134/Untitled%203.png](Volatility%2045d991905c424f54834deb7b02c56134/Untitled%203.png)

        - Processes aren't the only area we're concerned with when we're examining a machine. Using the 'apihooks' command we can view unexpected patches in the standard system DLLs. If we see an instance where Hooking module: <unknown> that's really bad. This command will take a while to run, however, it will show you all of the extraneous code introduced by the malware.
        - Injected code can be a huge issue and is highly indicative of very very bad things. We can check for this with the command `malfind`. Using the full command `volatility -f MEMORY_FILE.raw --profile=PROFILE malfind -D <Destination Directory>` we can not only find this code, but also dump it to our specified directory.

            ![Volatility%2045d991905c424f54834deb7b02c56134/Untitled%204.png](Volatility%2045d991905c424f54834deb7b02c56134/Untitled%204.png)

            - we can view all of the DLLs loaded into memory. DLLs are shared system libraries utilized in system processes. These are commonly subjected to hijacking and other side-loading attacks, making them a key target for forensics. Let's list all of the DLLs in memory now with the command `dlllist`

                ![Volatility%2045d991905c424f54834deb7b02c56134/Untitled%205.png](Volatility%2045d991905c424f54834deb7b02c56134/Untitled%205.png)

            - Now that we've seen all of the DLLs running in memory, let's go a step further and pull them out! Do this now with the command `volatility -f MEMORY_FILE.raw --profile=PROFILE --pid=PID dlldump -D <Destination Directory>` where the PID is the process ID of the infected process we identified earlier

                ![Volatility%2045d991905c424f54834deb7b02c56134/Untitled%206.png](Volatility%2045d991905c424f54834deb7b02c56134/Untitled%206.png)

- Extra Cred

    Interested in going further? Here's a slew of awesome resources (both paid and free in no particular order) to
    check out and learn more!

    **AlienVault Open Threat Exchange (OTX)** - An open-source threat tracking system. Create pulses based on your malware analysis work and check out the work of others. [Link](https://otx.alienvault.com/dashboard/new)

    ![https://i.imgur.com/JKq44GK.png](https://i.imgur.com/JKq44GK.png)

    **SANS 408** - Windows Forensic Analysis [Link](https://www.sans.org/blog/for408-windows-forensic-analysis-has-been-renumbered-to-for500-windows-forensics-analysis/)

    ![https://i.imgur.com/rWF7AEa.png](https://i.imgur.com/rWF7AEa.png)

    ["Memory Forensics with Vol(a|u)tility"](https://youtu.be/dB5852eAgpc) - A great talk on learning the basics of Volatility and the GUI plugin [VolUtility](https://github.com/kevthehermit/VolUtility) made by [@chupath1ngee](https://twitter.com/chupath1ngee?lang=en)

    "The Art of Memory Forensics" - [Link](https://www.amazon.com/Art-Memory-Forensics-Detecting-Malware/dp/1118825098/ref=sr_1_2?crid=W5I8MKDUXVWD&keywords=the+art+of+memory+forensics&qid=1582172165&sprefix=the+art+of+memory+%2Caps%2C152&sr=8-2)

    ![https://i.imgur.com/j2BBJxa.jpg](https://i.imgur.com/j2BBJxa.jpg)