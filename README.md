# cbridge_image
An SD card image for cbridge, plus script for creating a bridge on the ContinuumBridge server

To download the SD card image, paste the following into a Linux shell:

  wget https://github.com/ContinuumBridge/cbridge_image/releases/download/v1.0/cbridge.zip

Then type:

  unzip cbridge.zip

This will extract a file called 2016-04-20-BridgeMaster into the current folder. 

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

Now you can copy the image onto the SD card:

  sudo dd bs=4M if=./016-04-20-BridgeMaster of=/dev/sdc

Note that if the SD card filesystem was /dev/sdb, for example, use /dev/sdb instead of /dev/sdc.

Once the image has been copied, mount the new SD card filesystem. The easiest way of doing this is to remove and reinsert the SD card into the reader.

Next, clone this GitHub repository:

  git clone https://github.com/ContinuumBridge/cbridge_image.git

Then type:

  cd cbridge_image

For this next bit, you need a ContinuumBridge account, so if you haven't got one, sign up on portal.continuumbridge.com.

You can now register your bridge using this form of command:

  ./newbridge --bridge post --mount /media/ec2aa3d2-eee7-454e-8260-d145df5ddcba --user <your_user_name> --password <your_password>

where:

  mount is the mount point of the main filesystem, which you find by typing df again.
  user and password are your ContinuumBridge user name and password. If you don't type these, you will be prompted for them (which also means that your password won't be visible on the screen).
  
This gives your bridge a default name of the form BIDxxx, where xxx is the bridge number. You can change this with this form of command:

  ./newbridge --bridge patch --name MyBridge --mount /media/ec2aa3d2-eee7-454e-8260-d145df5ddcba --user <your_user_name> --password <your_password>
  
Now remove the SD card, insert it into a Raspberry Pi and power up the Raspberry Pi. See other documentation for how to connect to this. We recommend that you log onto the Raspberry Pi, either by connecting it to a screen and keyboard or using ssh over a LAN, and type:
  sudo raspi-config
  
Choose the Expand Filesystem option and follow instructions. This expands the size of the image to fill the SD card.




