



## Windows Vista

1. Ensure that the scanner is updated to the latest firmware, particularly for (models). https://www.silverfast.com/problemreport/pl.html?filter=assistant&sub=Nikonfirmware&faq_id=228. This in itself may be a complicated step as it requires that Nikon Scan version 3  
2. Acquire Windows Vista 32-bit ISO.
2. Use the Windows 7 DVD/USB Download Tool, which can be acquired from archive.org, to create a bootable USB. 
3. Follow the USB installer to install Windows Vista. Note that the boot disk must be formatted in an MBR partition scheme, not the newer and more common GPT. I used the `fdisk` utility in Linux to change my GPT-formatted SATA SSD to MBR.
4. Do *not* let Windows Vista connect to the Internet. Install drivers as needed.
5. Install Nikon Scan 4.0.3, which can be acquired from archive.org.
