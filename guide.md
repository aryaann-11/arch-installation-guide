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

Alright, the difficult part is over, it is smooth sailing from here

##  9. Change into the root directory of your new installation
###    arch-chroot /mnt
       Notice how this changes your prompt

##  10. Basic set up
### i.  ln -sf /usr/share/zoneinfo/<Region>/<city>/etc/localtime 
        This sets up your time zone. 
        To find available regions : ls /usr/share/zoneinfo 
        To find cities in your region : ls /use/share/zoneinfo/<region> 

### ii. hwclock --systohc
        This sets up your hardware clock

### iii.pacman -S vim
        This is where you install a text editor. It is completely upto you. Check out the arhc wiki for 
        available text editors in the pacman repostiory. vim , nano and emacs are among the famous ones.

### iv. Edit the /etc/locale.gen file and uncomment your locale. Most probably you might want to uncomment 
        the line which looks like #en-us.UTF-8 UTF-8

### v.  locale -gen "locale"
        Here "locale" is the one you uncommented in the previous step. In my case, en-us.UTF-8

### vi. Create a new file /etc/hostname and type in your desired hostname. Usually, it is the name of your computer
        Let us assume you typed in "hostname"

### vii.Create a file /etc/hosts and add the following lines
        a. 127.0.0.1 localhost
        b. ::1       localhost
        c. 127.0.1.1 hostname.localdomain hostname 

### viii. passwd 
          THis will set up the password for your root account. A prompt will appear asking you for the password

### ix. useradd -m "name of user (the username you want)"
        This will add a new user with the username you provide. This account will be the one you normally log into

### x. passwd "username of the newly created user"
       This will set up password for the new user.

### xi. usermod -aG wheel,audio,video,optical,storage "username"
        This will add the user to the listed groups. 

### xii.pacman -S sudo
        Install the sudo package from pacman

### xiii.pacman -S efibootmgr dosfstools os-prober mtools
         This packages will also be requried for your system

### xiv.mkdir /boot/EFI

### xv. mount /dev/sda1 /boot/EFI

### xvi.grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck

### xvii.grub-mkconfig -o /boot/grub/grub.cfg

### xviii.pacman -S networkmanager
          I prefer networkmanager to handle my internet. You can install the package of your choice

### xix. systemctl enable NetworkManager
         Here you must enable the service that you have installed. 

### xx. 1. exit
        2. umount -l /mnt
        3. reboot

And that is it, you have successfully installed Arch !

However, when you reboot and login with the new user account you created, you might be dissapointed because
there will be no graphical interface.

What comes next is the post installation but that is outside the scope of this guide. 

Basically, this is what you want to do
    1. Install a display server like xorg (or wayland) 
    2. Install a display manager for your login screen (my preference is lightdm)
    3. Install a desktop environment or window manager, popular choices are:
            i. Desktop environments:
                a.  Gnome
                b.  KDE
                c.  XFCE
                d.  LXDE
                e.  Mate
            ii. Window managers:
                 a. open box (floating window manager)
                 b. i3      (tiling window manager)
                 c. dwm     ( "        "       "  )
                 d. xmonad  ( "        "       "  )

    4. Install an aur helper like "yay"

At this point you will have most things to get you going. The arch wiki is a greate resouce to learn
more about these packages that i have listed and most importantly, how to install them. 


  

       
