# Dualboot-Fedora-Windows-fix-
Step-by-step guide to repair windows bootloader after installing Fedora (UEFI + GRUB).
Windows Boot Repair after Fedora Installation
wndowsfedora.jpg
This repository documents the process I used to repair a broken Windows
bootloader after installing Fedora on my PC.
Since many people face the same issue (dual-boot problems, EFI
overwritten, etc.), I decided to write down the exact steps that worked
for me. After anything, check that al Windows files are conserved in the HDD, if they still there, this may work for you.

------------------------------------------------------------------------

⚡ Problem

After installing Fedora, my system was unable to boot into Windows.
Instead, I got errors such as:

    /boot/EFI/fedora/grub.cfg not found
    Windows Boot Manager failed to start

------------------------------------------------------------------------

🛠️ Solution

Here is the step-by-step solution I used.

1. Create a Windows Bootable USB

I created a bootable Windows installation USB using Rufus (on another
PC).
⚠️ Fedora Media Writer do not work properly for this case. I tried Ventoy but didnit worked for me. Try with WoeUSB. 

2. Boot into Windows Installer

Insert the USB, boot from it, and select:

    Repair your computer → Troubleshoot → Advanced Options → Command Prompt

3. Run Boot Repair Commands

Inside Command Prompt, run the following commands one by one:

    bootrec /fixmbr
    bootrec /fixboot
    bootrec /scanos
    bootrec /rebuildbcd

If you get Access Denied on bootrec /fixboot, run:

    bootsect /nt60 sys

4. Rebuild EFI Entries

Then assign a letter to the EFI partition and recreate the boot entry:

    diskpart
    list disk
    select disk 0        # usually your main disk
    list partition
    select partition 1   # the EFI partition (around 100–500MB, FAT32)
    assign letter=Z:
    exit

Now run:

    bcdboot C:\Windows /s Z: /f UEFI

5. Restart

Remove the USB and restart your PC.
Now Windows Boot Manager should work again 🎉

------------------------------------------------------------------------

📌 Notes

-   If Fedora or any other Linux overwrote your EFI, these steps will
    restore Windows Boot Manager.
-   You can reinstall GRUB later if you want dual boot.

------------------------------------------------------------------------

📜 License

MIT License – feel free to use and share this guide.
