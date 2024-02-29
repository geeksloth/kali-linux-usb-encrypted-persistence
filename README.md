# Kali Linux USB Live Boot with Encrypted Persistence
GeekSloth's basic example of making Kali Linux USB Liveboot with Encrypted Persistence.

After achived this procedures, you will get the powerful *Kali Linux USB Live Boot with Encrypted Persistence* to be used in your daily GEEK LIFE. Especially in educational purposes, your security, your privacy, and for **ETHICAL Hacking**. I've reviewed some of the result in this video, don't forget to check it out (click the below thumbnail to see the video):

[![GeekSloth's review of Kali Linux USB Live Boot with Encrypted Persistence](https://img.youtube.com/vi/ZUuXnljSLNI/0.jpg)](https://www.youtube.com/watch?v=ZUuXnljSLNI)

*สำหรับภาษาไทย แนะนำให้ดูในคลิป Youtube ได้เลยนะครับ คลิกที่รูปข้างล่างได้เลยครับ คลิปแรกเป็นการสาธิการใช้งานเบื้องต้น คลิปที่สองเป็นวิธีทำครับ*

## Prerequisites
1. USB 3.0 (or above) Thumbdrive. I recommend to use at least 64GB.
2. Personal Computer which running **Linux** e.g. *UBUNTU*, *Debian*, *Linux MINT*, or any distribution you love. BTW, I've not tested in Windows or *WSL2*, but I think it should be work as well.
3. Download the *ISO* 64-bit from [https://www.kali.org/get-kali/#kali-installer-images](https://www.kali.org/get-kali/#kali-installer-images)

## Let's get started
If you don't wan to walk alone, let's see this video and do it together!

[![GeekSloth's how to make Kali Linux USB Live Boot with Encrypted Persistence](https://img.youtube.com/vi/Ob8XsIZZwGw/0.jpg)](https://www.youtube.com/watch?v=Ob8XsIZZwGw)

1. Open a terminal and navigate `cd` to your downloaded *ISO* file folder, ie. *Downloads* folder:
```bash
cd ~/Downloads
```
Tips: following commands are mostly run by `sudo` so if you understand what you're doing, you can login to *sudo* user by ```sudo -i```. Then following commands are not require the term `sudo` anymore.


2. Plug in *the USB Thumbdrive* to *PC* and check the device path. Remember your `sdX` device carefuly, because if it is the wrong one, you might be say goodbye to your data forever. In my case, it is `sda`:
```bash
sudo ls /dev/sd*
```
The above command, you just try to run it before and after inserting the USB Thumbdrive to see the difference result.


3. Copy the whole data and partitions to the USB. This might take a while depends on your USB thumbdrive speed:
```bash
sudo dd if=kali-linux-2023.4-live-amd64.iso of=/dev/sda conv=fsync bs=4M
```
```bash
sudo fdisk /dev/sda <<< $(printf "n\np\n\n\n\nw")
```
When fdisk completes, the new partition should have been created at /dev/sda3; this can be verified with the command lsblk.


4. Making Encryption for the partition with **LUKS**:
```bash
sudo cryptsetup --verbose --verify-passphrase luksFormat /dev/sda3
```
```bash
sudo cryptsetup luksOpen /dev/sda3 my_usb
```


5. Making Persistence for the *Encrypted* partition, and then label it:
```bash
sudo mkfs.ext4 -L persistence /dev/mapper/my_usb
```
```bash
sudo e2label /dev/mapper/my_usb persistence
```


6. Mount the partition and create your persistence.conf so changes persist across reboots:
```bash
sudo mkdir -p /mnt/my_usb
```
```bash
sudo mount /dev/mapper/my_usb /mnt/my_usb
```
```bash
sudo echo "/ union" | sudo tee /mnt/my_usb/persistence.conf
```
```bash
sudo umount /dev/mapper/my_usb
```


7. Close the partition:
```bash
sudo cryptsetup luksClose /dev/mapper/my_usb
```

## Congratulation!
You just finished this tutorial. Just close everthing and pull off the *USB Thumbdrive*, which is now called **Kali Linux USB Live Boot Encrypted Persistence**, from the *PC*. Then let's try it by yourself!

**Share with love:** I made this tutorial and related videos with a mindset of *sharing*, *educational purposes*, myself (and yourself) security and privacy. Hopefully this will benefit to you all.

**If you like this**, don't forget to *hit a like* on the youtube videos, and subscribe for more great stuffs at [https://youtube.com/@geeksloth](https://youtube.com/@geeksloth)

*disclaimer* many parts in this tutorial are refered from the official document from [Kali Linux Docs](https://www.kali.org/docs/usb/usb-persistence-encryption/)
