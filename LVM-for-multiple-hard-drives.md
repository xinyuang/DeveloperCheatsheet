I started with a disk that I did not have anything on it that I needed.
First you need to create a partition as Linux LVM. You can use fdisk to do this. You can see all the drives Ubuntu sees and how they are listed by running: sudo fdisk -l
The first line in each section should give you enough information to identify your drive. It will look like:

Disk /dev/sda: 500.1 GB, 500107862016 bytes

The part that matters is /dev/sda. Now run: sudo fdisk /dev/sda. You will see:

Command (m for help):

Type p to list the partitions on your drive. You need to delete the partitions that you want to make part of the LVM. So type d to delete. If the drive only has one partition it will remove it (well flag it for removal, it does not happen until we tell it to do it). Otherwise I think (mine only had one) it asks you to enter the number of the one you want to delete.

Now you need to create the new parition. Type n for new. It asks whether extended or primary. Type p for primary. It asks for partition number, type 1. For first cylinder and last cylinder just leave them blank to use the defaults.

Now you need to set it to Linux LVM. Type t. It asks for a hex code, use 8e for the Linux LVM. You shoule see something like:

Changed system type of partition 1 to 8e (Linux LVM)

Finally type w to write out the changes to the disk.

Now we need to instal LVM so run sudo apt-get install lvm2 to install it.
I am gonna be honest and say I am not sure what this step does but the other directions said to do modprobe dm-mod to Load the LVM Module. I did not get any errors so I figure it worked.
We need to edit the /etc/modules file to this module loads on boot. Do sudo nano /etc/modules to open it up to edit. Add dm-mod to the list of items.
We also want to edit the lvm configuration to update the filter so it does not take to long to scan (I think that is why anyway). So do sudo nano -w /etc/lvm/lvm.conf and change the line with:
filter = [ "a/.*/" ]

to be:

filter = [ "a|/dev/hd[ab]|", "r/.*/" ]

Now we need to set up the first LVM. Do sudo vgscan. You should see something like:
Reading all physical volumes. This may take a while...
No volume groups found

Just in case there are any volume groups already set up run sudo vgchange -a y to make them available.

Now run sudo pvcreate /dev/sda1 to set up the partition.
Now run sudo vgcreate media /dev/sda1 replacing media with the name you want the partition to be labeled as.
Now run sudo lvcreate -l100%FREE -nvolume media replacing volume with the name you want it to be called. This will use all the free space available in the partition.
Now we need to format the volume so for ext4 you would do sudo mke2fs -t ext4 /dev/media/volume.
Make the directory you want to mount the volume at. I did sudo mkdir /mnt/media.
Mount the volume by doing sudo mount /dev/media/volume /mnt/media. Now this is only for this session. When you reboot it will not be remounted automatically. To do that we need to edit /etc/fstab file. To do this add sudo nano /etc/fstab and add the line:
/dev/media/volume /mnt/media ext4 defaults 0 1

At this point you could start adding files to the disk, so if you need to clear other disks you want to add you could copy them on here.

Adding another drive to your volume
So follow the steps in the first bullet again but for the new drive.
Now if the drive name is /dev/sdb1 then do sudo vgextend media /dev/sdb1 to add it to the volume.
Now we need to unmount the volume. To do this do sudo umount /dev/media/volume.
Now you can see the stats on your volume now by running sudo vgdisplay. The important part is Free  PE / Size. You need to know how much space you can add to the volume for the next step.
So if you had 150 Gb of space you would do sudo lvextend -L+150G /dev/media/volume.
Now run sudo e2fsck -f /dev/media/volume to check the filesystem.
Now run sudo resize2fs /dev/media/volume to resize everything.
You can run the stats again and verify that Free PE / Size has dropped to what you expect.
Remount the volume by doing sudo mount /dev/media/volume /mnt/media
Rinse and repeat for any other drives.
Also something that I found helpful was I had files I needed to copy off of disks to the LVM I created before I added that disk. So I used cp -r -v so that it would recursively copy files and use the verbose output so I know what it was doing. An example of the full command would be:
cp -r -v /mnt/temp/Movies /mnt/shared/media

Where /mnt/temp/Movies is the folder you want to copy from.