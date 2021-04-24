## -1. Get an arch iso image. 
      You can get the link from the ArchWiki. The preffered way to download the iso
      is to use the torrent 
##  0. Create a bootable drive and boot

### NOTE: At this point you will be able to enter commands to install Arch. Now, the 
###       work begins

##  1. Check your keymaps
###    ls /usr/share/kbd/keymaps/**/*.map.gz 
       this will list all available keymaps. 
###    loadkeys <keymap>
       do this if needed. This will set your keymap to <keymap>. Pick out <keymap> from the list

##  2. Check for working internet
###    ping google.com 

##  3. Check if your system clock is accurate
###    timedatectl set-map true
###    timedatectl status

##  4. Time to partition your disk
###    fdisk -l
       this command lists all drives available. Ignore the ones with "loop"
       You are looking for /dev/sdaX where X is an integer
###    fdisk /dev/sda
       This will open up a command prompt. Press 'm' to print a help menu
       i.  Press 'g' to create a new gpt table
       ii. Only if you do not have a EFI drive already on your computer
            a. Press 'n' to create a new partition
            b. Give it a number as a new label
            c. Provide a start sector. Now here's the tricky bit, make sure
               you're not overwriting any of the partitions. Try to validate with 
               fdisk -l. Please go and search on google about how computer memory /
               fdisk works. Not to discourage you, but if you are too uncomfortable with 
               all this, maybe its too early for Arch. 
            d. Now, you will have to give this partition a size. The prompt will tell you
               how to input the number. Here, we want to allocate 550 mega bytes. So type
               +550M. 
            This will be your EFI partition. Remember, if you already have a distro installed
            on your machine, you don't need another EFI partition
       iii. Create two more partitions, one with size about 2G and the other one as big as 
            you want. These will be your swap and main partitions respectively
       iv. At this point, all these newly created partitions have type Linux File System.
           We want to change that. Type 't' to change type and provide the number of the 
           partition. Change the type of your EFI partition to EFI system and your swap partition
           to Linux Swap. Note: you will have to press 't' again after you're done with either of these
           to do the other
       v. Press 'w' to write to table. We are done with the partitioning. This will also exit you out
          of the fdisk prompt
##  5. Create file systems for your partitions
       The numbers following sda might be different for you
###    mkfs.fat -F32 /dev/sda1 
       Do this, only if you created an EFI partition
###    mkswap /dev/sda2 
###    swapon /dev/sda2
###    mkfs.ext4 /dev/sda3
       I did an ext4 for my Linux File System. But I heard btrfs is pretty good and gaining popularity. You might want to 
       try that
       
##  6. Mount the partitions
###    mount /dev/sda3 /mnt
       Again, the number following sda has to be the one which has your main partition. This number might be different 
       for you
##  7. Install the base system for arch
###    pacstrap /mnt base linux linux-firmware

##  8. Generate you file system table file
###    genfstab -U /mnt >> /mnt/etc/fstab

Alright, the weird part is over, it is smooth sailing from here

  

       
