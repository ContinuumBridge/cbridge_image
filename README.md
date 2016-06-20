# cbridge_image
SD card images for cbridge, plus script for creating a bridge on the ContinuumBridge server

There are two versions of the image:

* Version 1.0 is compatible with all Raspberry Pi 1 and 2 boards, but not Raspberry Pi 3 (or at least has not been tested).
* Version 2.0 is compatible with Raspberry Pi 2 and 3. It is based on 2016-05-27 version of Raspbian Jessie Lite. This is recommended for all future work and incorporates additional security features.

To download the SD card image, paste one of the following into a Linux shell:

    wget https://github.com/ContinuumBridge/cbridge_image/releases/download/v1.0/cbridge.zip
    wget https://github.com/ContinuumBridge/cbridge_image/releases/download/v2.0/cbridge.zip

Then type:

    unzip cbridge.zip

This will extract a file called 2016-04-20-BridgeMaster or 2016-06-19-Bridge-Master into the current folder. 

Put a SD card into an SD card reader attached to your PC and umount all its partitions. For example, once you have inserted the SD card, type df, and you'll see a response similar to the following:

    $ df
    Filesystem     1K-blocks      Used Available Use% Mounted on
    /dev/sda1       19478204  13594004   4888104  74% /
    udev              496228         4    496224   1% /dev
    tmpfs             101160       840    100320   1% /run
    none                5120         0      5120   0% /run/lock
    none              505780       152    505628   1% /run/shm
    /dev/sdb1      488384032 383657304 104726728  79% /media/FreeAgent GoFlex Drive
    /dev/sdc1        7753728        32   7753696   1% /media/3033-6530

In this case, the SD card filesystem is /dev/sdc1 (you can tell as it's the one that's just appeared). Note that, particularly if you've used the SD card before, there are likely to be more than one partition. Unmount them all. In this case, the unmmount command is:

    umount /dev/sdc1

Now you can copy the image onto the SD card using the appropriate commands:

    sudo dd bs=4M if=./2016-04-20-BridgeMaster of=/dev/sdc
    sudo dd bs=4M if=./2016-06-19-Bridge-Master of=/dev/sdc

Note that if the SD card filesystem was /dev/sdb, for example, use /dev/sdb instead of /dev/sdc.

Once the image has been copied, mount the new SD card filesystem. The easiest way of doing this is to remove and reinsert the SD card into the reader.

Next, clone this GitHub repository:

    git clone https://github.com/ContinuumBridge/cbridge_image.git

Then type:

    cd cbridge_image

For this next bit, you need a ContinuumBridge account, so if you haven't got one, sign up on portal.continuumbridge.com.

You can now register your bridge. First type df again:

    Filesystem     1K-blocks      Used Available Use% Mounted on
    /dev/sda1       19478204  16522460   1959648  90% /
    udev              496228         4    496224   1% /dev
    tmpfs             101160       844    100316   1% /run
    none                5120         0      5120   0% /run/lock
    none              505780       152    505628   1% /run/shm
    /dev/sdb1      488384032 386585948 101798084  80% /media/FreeAgent GoFlex Drive
    /dev/sdc2        2756644   2659352         0 100% /media/ec2aa3d2-eee7-454e-8260-d145df5ddcba
    /dev/sdc1          57288     19920     37368  35% /media/boot

In this case, the filesystem at /dev/sdc2 is what you need in the following command:

    ./newbridge --bridge post --mount /media/ec2aa3d2-eee7-454e-8260-d145df5ddcba --user <your_user_name> --password <your_password>

where:

    mount is the mount point of the main filesystem, which you find by typing df again.
    user and password are your ContinuumBridge user name and password. If you don't type these, you will be prompted for them (which also means that your password won't be visible on the screen).
  
This gives your bridge a default name of the form BIDxxx, where xxx is the bridge number. You can change this with this form of command, which changes the bridge name to MyBridge:

    ./newbridge --bridge patch --name MyBridge --mount /media/ec2aa3d2-eee7-454e-8260-d145df5ddcba --user <your_user_name> --password <your_password>
  
Now remove the SD card, insert it into a Raspberry Pi and power up the Raspberry Pi. See other documentation for how to connect to this. When logging on to the Raspberry Pi, user the following credentials:

    user name: bridge
    password: t00f@r
    
Those are zeros in the password. WE RECOMMEND THAT YOU CHANGE THIS IMMEDIATELY USING THE LINUX passwd COMMAND.

Once you've logged on, type:

    sudo raspi-config
  
Choose the Expand Filesystem option and follow instructions. This expands the size of the image to fill the SD card.

The cbridge software automatically runs when you power-up your Raspberry Pi. You can log onto your account portal.continuumbridge.com and see it. A good first test is just typing return in the portal command window. If the bridge is connected it will response with the date and version number.




