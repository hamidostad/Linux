##How to change root password in Ubuntu:
Type the following command to become root user and issue passwd:
sudo -i
passwd

OR set a password for root user in a single go:
sudo passwd root

Test it your root password by typing the following command:
su -


/bin user binaries
/sbin system binaries
/etc config files
/dev devices files
/proc process info
/var varaible files
/tmp temp files
/usr user programs
/usr/bin: User commands
/usr/sbin: system administration commands
/usr/local: Locally customized software
/home home directories
/lib system libraries
/mnt mount directories
/media /removable devices
/srv services data
/boot >> files needed in order to start boot process


/tmp >> 10days
/var/tmp >> 30 days


env = list environment variables
unset = delete variables
set = shows all varaibles and functions
export = makes a varaible an environment varaible

## to improve performance, how to safely set the limit of process for super user to be unlimited?
unlimit -u
unlimit -u unlimited

#useradd
useradd -d /home/kavian -m -G adm,gdm,cdrom,mysql kavian
usermod -s /bin/sh kavian
userdel -r kavian
groupadd -g 666666 groupname
usermod -aG groupname kavian

useradd -d /home/mimo -m -G adm,gdm,cdrom,mysql mimo
usermod -s /bin/sh -c "comment" mimo
cat /etc/passwd
userdel -r mimo
usermod -L mimo (to lock account)
usermod -U mimo (to unlock account)
groupadd -g 666999 mygroup
vim /etc/group
usermod -aG mygroup mimo
groupmode -g 777888 mygroup
groupdel mygroup
chage -l kavian
chage -E 2020-10-10 kavian



## create a user with a pre defined uid, shell and home directory?
useradd -m -d /home/user -s /bin/bash -u 9000 kavian

## how to delete a user with his home directory?
userdel -r kavian

## how to create a user specifying a primary/secondary group?
useradd kavian -g primary -G <other groups>

##how to change primary group for any user?
usermod -g primarygroup kavian

##how to give a normal user all the root privileges?
visudo
Allow root to run any commands anywhere
add user here

##give sudo access to user without asking him to provide password every time he runs:
same thing without password
add user here

##info about last login user info
lastlog
lastlog -u kavian

##how to view the user's login and logout detail:
last kavian

##view all the current logged in users:
w

##how to lock abd unlock the user accoutn?
passwd -l
passwd -u


## how to check the limits of opened files in linux?
cat /proc/sys/fs/file-max

## how to increase the limits of opened files in linux?
vi /etc/sysctl.conf
fs.file-max=98321
sysctl -p 
cat /proc/sys/fs/file-max



##modules:
lsmod
rmmod <mod name> >> to remove module
modprobr <mod name> >> to install module
insmod <modulename>  >> to install module

lsmod:
Lists the kernel modules (drivers) that are loaded into memory
lscpu:
Displays a summary of the CPU and its features/configuration
lsscsi:
Displays information on any SCSI devices detected 
lsraid:
Displays any RAID (Redundant Array of Inexpensive Disks) on your system
lsusb:
Displays USB device IDs and general information about detected devices 
lsblk:
Display block devices (disks) that are attached to the running system 


vim /etc/modules
vim /etc/modprobe.d


##dmesg:
dmesg vs /var/log/dmesg
The dmesg command shows the current content of the kernel syslog ring buffer messages while the /var/log/dmesg file contains what was in that ring buffer when the boot process last completed.

dmesg >> realtime
cat /var/log/mseg >> during startup


## What is LILO?
LILO is a boot loader for Linux. It is used mainly to load the Linux operating system into main memory so that it can begin its operations.

## Swap space is a certain amount of physical disk space used by Linux to temporarily hold some concurrently running programs. Swap space is generally used when the RAM memory is full, i.e. when RAM runs out of memory space to hold all programs that are executing. Swap space is much slower than RAM, and therefore the programs it executes are also much slower. Small price to pay to prevent your machine from freezing up, which can happen if RAM gets overloaded and there's no swap space to fall back onto. Swap space is generally useful/relevant for low-spec machines with very little RAM memory available, i.e. around 4gb RAM or less. For machines with 16gb+ RAM, swap space should really never be necessary, unless it's a high resource load server...


##boot:
run level 0 >> shutdown Halt
run level 1 >> single user (recovery)
run level 2 >> Debian default >> multi user without net access (Debian/Ubuntu default)
run level 3 >> Redhat and SUSE default >> multi user with net access (RHEL/Fedora/SUSE text mode)
run level 4 >> not used(empty) >> customizable (free)
run level 5 >> Redhat and SUSE >> graphical mode (RHEL/Fedora/SUSE graphical mode)
run level 6 >> reboot

runlevel:
Displays the current runlevel (and the previous, if available) 

systemctl get-default:
Displays the current default runlevel target

systemctl set-default [new.target]
Will set the default runlevel target to the indicated value 

systemctl list-units --type=target:
List all the active system targets 

systemctl isolate [runlevel.target]:
Allows you to set the runlevel of the system without changing the default 
systemctl isolate graphical.target:
would change the system to the full desktop graphical mode 

uptime
Displays the amount of time the system has been running since the last boot/reboot 

shutdown - will reboot OR halt the system:
•  -h - halt the system (shut it down) 
•  -r - reboot the system 
•  -P - power off (if ACPI is available) 
•  -c - cancel shutdown 
•  -k [message] - broadcasts a 'wall' message to logged in users

shutdown -r 30
systemctl halt - halt, do not power off
systemctl poweroff - halt and power off (if ACPI is available)
systemctl reboot - halt and reboot 

shutdown -r 60 Reloading updated kernel
shutdown -r 14:45 server reboot mishe
shutdown -r now server reboot mishe

wall:
Allows you to broadcast a message to all logged in users, limited to 20 lines of text
For example:
wall “This system is going offline in five minutes - wrap it up and save your files” 
• NOTE: this message will appear on every terminal, often overwriting or interrupting terminal text 

wall "Hello"
shutdown -r 30 -k "You better get off now"
shutdown -c >> to cancel the shutdown 


##listing services in certain runlevel:
ls -l /etc/rc2.d/
ls -l /etc/rc?.d/*rsyslog

##to show current runlevel:
runlevel

##to change runlevel:
telinit 3
vim /etc/inittab
or
vim /etc/init/rc-sysinit.conf

##enbale/disable a process from starting at boot time
update-rc.d -f apache2 remove
update-rc.d apache2 defaults
ls -l /etc/rc?.d/*apache2
or
update-rc.d apache2 disable >> changes all to kill
update-rc.d apache2 disable 3 >> changes to kill on runlevel 3
update-rc.d apache2 enable >> revert to default


bootmanager:

bootloader
/etc/lilo.conf
/boot/grub/grub.cfg
/boot/grub/menu;lst

cat /etc/default/grub 
cat /boot/grub/grub.cfg >> grub v2
cat /boot/grub/menu.lst >> grub v1

If you want to reconfigue grub file, first edit the grub:
sudo vim /etc/default/grub
then use the following command:
sudo grub-mkconfig -o /boot/grub/grub.cfg


init.d >> services.msc in windows!

/etc/init.d/network status|start|stop|restart
systemctl status network


##hard disk design:
simplest partition: 
/boot /Root partition /swap

network desktop design partition: 
/boot /Root partition /home /swap

server design partition: 
disk 1:/boot /root partition /home /swap /var
disk 2:/home /var

create raid 0 for /home and /var

dd >> create an image from disk
dd if=/dev/sdb of=usb-storage.img
dd if=usb-storage.img of=/dev/sdb

*creating an image file from USB:
sudo dd if=/dev/sdb of=usb-storage.img



partitioning:
fdisk -l
fdisk /dev/sdc
m
n
p
1
1
+500M
p >> print
w
t
L
swapon -s
mkswap /dev/sdc2
swapon /dev/sdc2
mkfs -t ext2 /dev/sdc4 or mkfs.ext2 /dev/sdc

fdisk /dev/sdb
t
1
L
86 (NTFS)
p
t
2
L
82 (SWAP)
p
t
3
c (FAT32)
p

swapon -s
mkswap /dev/sdb2
swapon /dev/sdb2

formatting Linux file system:
mkfs
For example:
mkfs -t ext4 -b 8192 -m 10 -L LargeData -O sparse_super /dev/sde4
Would create an 'ext4' partition, with a block size of '8192' (meaning that the minimum space allocated to ANY file is 8192 - something you may do for a data drive with lots of large files), reserving 10% for root use, and using the 'sparse_super' option (which reduces the number of superblock copies available for recovery) 

mkfs.[fstype]
Equivalent command for each filesystem type to format the indicated partition
For example:
mkfs.ext3 /dev/sda3


mkfs -t ext2 /dev/sdb5
mkfs -t ext3 /dev/sdb6
mkfs -t ext4 /dev/sdb7
mkfs -t vfat /dev/sdb3
or
mkfs.ext3 /dev/sdb
mkfs.ext4 /dev/sdb
mkfs.xfs /dev/sdb

#df
Reports on file system disk space usage 
• -a (or --all) - include all filesystems (including 'dummy' filesystems) 
• --direct - show stats for a file instead of mount • --total - print a grand total 
• -h (or --human-readable) - print sizes in readable format (K/M/G/TB) 
• -l (or --local) - include only local file systems 
• -t (or --type) - limit listing to the indicated type
For example:
df -lh --total

df -h (human readable)
df -i (showing inode info)
df -ih

#du
Responsible for 'estimating' disk space usage on files and/or directories 
• -a (or --all) - write counts for all files and not just directories 
• -c (or --total) - produce a grand total 
• -h (or --human-readable) - print sizes in readable format (K/M/G/TB) 
• -s (or --summarize) - display only a summary for any argument
For example:
du -sh /home/user/.bash* 
du -sch /home
du -sch /var/*

du -h
du -h folderpath
du -h Downloads/
du -h --summarize

##find disk usage by the largest directories
du -S | sort -n | more


## What is the command to calculate the size of a folder?
du –sh folder1 


fsck /dev/sdb1
*in order to check disk, first it should be unmonted.
umount /dev/sdb1
mount
fsck /dev/sdb1

mount -t ext3 /dev/sdb1 /dev/folder1
mount

##journal
## view all messages generated by the system since the last reboot (redhat and centos)
## use journalctl to view the systemd journal. the boot process, kernel and all systemd services put messages into the systemd journal.
journalctl
journalctl | grep ssh

to add journal feature:
tune2fs -0 has_journal /dev/sdb1
dumpe2fs /dev/sdb1 | grep feature

to remove journal feature:
tune2fs -0 ^has_journal /dev/sdb1

## two different ways of showing the kernel messages?
dmesg
journalctl -k

## how to continously monitor logs as they come in?
journalctl -f
tail -f /var/log/messages


## how to extend SWAP space?
lsblk
vgs
lvs
lvextend -L 900M /dev/server1/swap
swapoff -v /dev/server1/swap
mkswap /dev/server1/swap
swapon -va
swapon -s

#Disk quota
apt-get install quota
vim /etc/fstab
(default,usrquota,grpquota)
:wq
umount /dev/sdb1 
mount /dev/sdb1
quotaoff -a
quotacheck -cug /dev/sdb1
edquota -u kavian
quotaon -/dev/sdb1
repquota -a
repquota /dev/sdb1



## What is the typically recommended size for a swap partition under a Linux system?
The recommended size for a swap partition is twice the amount of physical(RAM) memory available on the system. If this is not possible, then the minimum size should be the same as the amount of physical memory installed. 


##how to create a volume group
lsblk
pvs
vgs
vgcreate new_vg /dev/sdb
vgs


##how to create a logical volume
lsblk
pvs
vgs
lvcreate -n test_lv -L 500M new_vg
mkfs.ext4 /dev/new_vg/test_lv
df -h
mkdir testdir
mount /dev/new_vg/test_lv /testdir
df -h
to permanentlt mount, it should be added to fstab as well
cat /etc/fstab

##how to extend a logical volume?
lsblk
vgs
lvs
lvextend -L 700M /dev/new_vg/test_lv
lvs
resize2fs /dev/new_vg/test_lv

## how to create a physical volume after the disk space added:
lsblk
pvcreate /dev/sdb
pvs

## is it possible to increse the logical volume on fly?
YES, LVM has the feature to increase the volume without unmounting it.
lvextend -L 8G /dev/vg/lv

## is it possible to reduce the logical volume on fly?
NO, we can't resuce the lv on fly.
#steps to reduce:
lvs
#un-mount the filesystem
unmount /test/
#run e2fsck on the volume device
e2fsck /dev/new_vg/test_lv
#reduce the logical volume using lvreduce
#reduce the filesystem using resize2fs
resize2fs /dev/new_vg/test_lv
lvreduce -L 400M /dev/new_vg/test_lv
mount the filesystem back for production
lvs
mount dev/new_vg/test_lv/test/
df -h

## how to scan disks for existing volume groups
pvscan
vgscan

## how to scan a logical volume from existing volume group
lvscan

## how to stop/diactivate a logical volume
lvchange -an /dev/vg_name/lv_name

## how to activate the logical volume
lvchange -ay /dev/vg_name/lv_name

## how to disable/diactivate the volume group
vgchange -an volume_group_name

## how to enable/activate the volume group?
vgchange -ay volume_group_names

## what is the default size of a physical extent in LVM
4MB

## how to list of all available logical volumes
lvs

## how to list of all available physical volume
pvs

## how to see the detailed volume group info
vgdisplay vgname



/media
General purpose 'parent' directory that is often used for 'removeable' filesystems (CDs, DVDs, etc)
Mount point(s) for removable media (CD, DVD, etc) 

/mnt
General purpose 'parent' directory that is often used for mounting disk/partitions that are NOT part of the filesystem installation or within the 'root' structure of the OS (i.e. a backup or purely data drive)
Temporarily mounted filesystems 

/etc/fstab
Single line mount configuration for local (and remote) filesystems that are to be mounted on boot
Contents of the columns are: 
1. The device (i.e. /dev/sda1 OR LABEL=data OR UUID=44d27f92-d3df-420780ea-22830afccf03) 
2. Mount point (i.e. / OR /mnt/data, etc) 
3. Filesystem - supported filesystem type (i.e. ext3 OR xfs, etc) 
4. Options - comma separated list: 
• noauto - do not automatically mount on boot (prevents removeable devices that may cause issues from being accessed on boot) 
• defaults - common disk option made up of rw, setuid, dev, exec, auto, nouser, async 
• user - only the user that mounts the disk (CD-ROM/DVD/Removeable Disk) can unmount it 
• users - any user can unmount the disk (CD-ROM/DVD/Removeable Disk) 
• ro - mount read only 
5. dump - value of '0' will prevent the 'dump' command from affecting it 
6. fsck - value of '1' will indicate the filesystem should be the first checked 



##library:
shared library to reduce ram and disk usage.
cd /etc/lib
.so >> standard library format
.so.<number> >> version number of library
.so.<number1>.<number2> >> release number
cd /etc/lib
cd /usr/lib
cd /lib64/
cd /lib/`
cd /usr/local/lib
to locate lib location of a application:
ldd /bin/ping
ldd /sbin/ifconfig
/etc/ld.so.conf >> list of all links of libraries
ldconfig >> put all links of libraries in a binary at ld.so.cache (/etc/ld.so.cache)
to add a local path for library >> export LD_LIBRARY_PATH=/home/kavian/lib:/home/kavian/Documents

## How do you uninstall the libraries in Linux?
sudo apt-get remove library_name


##package management Debian based:

dpkg >> old method (not recommended! )
dpkg --install
dpkg --list
dpkg --listfiles
dpkg --purge
dpkg --remove (remove only the program)
dpkg --search (display package supplying files)

reconfigure package:
dpkg-reconfigure tzdata

to see contents of a downloaded package:
dpkg --contents name.deb

dpkg -s name.deb >> status of a downloaded package

dpkg -P telnet >> remove the package and all its configuration files

dpkg -L telnet >> to check the files and directories a package installed.


for updating a single package:
apt-get install telnet

for upgrading whatever installed
apt-get upgrade

going to a new distribution
apt-get dist-upgrade

apt-get update
apt-get upgrade
apt-get install (check fot dependency as well)
apt-get remove
apt-get purge
apt-get autoremove (removes all unused packages)

#apt-cache (local)
apt-cache show packagename
apt-cache depends (showing dependencies of this package)
apt-cache rdepends (showing which packages/libraries are depends on this package)
apt-cache search packagename
apt-cache search firefox
apt-cache show

apt-cache search <package name>
apt-cache depends <package name> >> which application depends on our pachage
apt-cache rdepend <package name> >> reverse
apt-get --help

aptitude
aptitude install firefox
aptitude --help

list of online repositories:
vim /etc/apt/sources.list

##package management redhat based:
rpm install
rpm remove
rpm signature checking
rpm verifying
rpm querying
rpm -i --nodeps
rpm -i *.rpm
rpm -iv *.rpm (v >> verbose )
rpm -K openssh-clients.rpm (check the healthy of downloaded package)
rpm -qi openssh-clients (query info)
rpm -qa (list of all installed packages)
rpm -qa | grep ssh
rpm -qRp telnet
rpm -Va
-Va - verify ALL installated packages on the system


rpm2cpio
Extracts the contents of the *.rpm package and streams it to standard out (or it can be redirected to a file) 
For example:
rpm2cpio telnet-0.17-60.el6.x86_64.rpm > telnet.cpio
Will unpack the rpm file indicated and create the cpio archive 'telnet.cpio'
rpm2cpio >> extracting rpm package
rpm2cpio webmin.rpm > webmin.cpio

alien >> converting .deb to .rpm

yum
yum install firefox
yum install -y telnet (ignore prompt)
yum update >> update a package or packages on your system
yum upgrade >> update packages taking obsoletes into account
yum install --downloadonly telnet

/etc/yum.repos.d
in the repository configuration log, the location is defined as either a 'baseurl' (direct link) to a repository or a 'metalink', which will return a list of mirror sites to use.

only repositories that are set to 'ENABLED=1' in the repo file will be used by the yum utility.

cd /etc/yum.repos.d/
cat CentOS-Base.repo

/etc/yum.conf
/etc/yum.repos.d

yumdownloader
--source - download only source RPM 
--urls - display the URL of the files without downloading 
--destdir - allows you to indicate the directory to store the package download 
--resolve - includes any dependencies 
yumdownloader <package name> >> to download rpm package (without downloading dependencies)
yumdownloader --resolve <package name> >> download full package with all dependencies
yumdownloader --source telnet
yumdownloader --urls telnet
yumdownloader --resolve telnet
yumdownloader --destdir /root telnet  (specify destination to download)


/var/cache/yum/[architecture]/[version]/base/packages - the directory the package will be downloaded to.
cd /var/cache/yum/x86_64/6/base/packages


##find
find . >> showing all files in current path and its subfolders
find . -name "u*"
find . -name "k*"
find . -name "[a-c]*.*"
find . -name "[1]*.*"
find / -name "text.txt" -exec chmod 700 {} \;
find . -size +1M > bigger than 1 MB
find . -size -1M > less than 1 MB
find . -size -3M -name "*.jpg"
find . -type d >> directory
find . -type f >> file	
find . -type f -size +5M
find . -type d > showing directories
find . -type f > showing files
find / -type l >> find symobil link
find / -typr f -name "*.log"
find . -atime +2 > accessed time since 2days ago
find . -ctime +5 > changed time since 5days ago
find . -mtime +1 > modified time since 1days ago
find / -mmin -10 >> modified last 10 minutes
find / -perm 755
find / -perm -u+s >> all files that set to user id
find / -perm -g+s >> all groups that set to group id
find / -perm /u+s,g+s >> or
find / -iname "text.txt"
find /myfolder -not -name "text.txt"
find / "*.zip" | cpio -o > ./zipfiles.cpio
'-exec [cmd] {} [output] \;' 
find /home/user/bin -name “*.sh” -exec cp -f {} /home/ user/backup/bin \; 

find / -iname “*.sh” 2> /dev/null
Will display the results of the found matches without displaying error messages related to permissions 

find / -iname “*.sh” 2> /dev/null > output.txt 
The same as the above example, but matches will be redirected to the output.txt file instead of displayed on the screen 

find / -iname “*.sh” > /dev/null 2>&1
Redirects standard error to standard output (2>&1>) and the whole output stream is redirected to /dev/null (discarded) 

find / -type f -size -100k -o +5M | wc -l
find haystack/ -type f -name "needle.txt" -exec mv {} ~/Desktop \;
**you can use the -exec option to execute another command on each of the results - remember to end with \;


##tee
Accepts a standard input stream and sends one (identical) output stream to an indicated file.
Often used when you want to capture the output of an application but also need to see the results on screen.
for example:
find / -name “*.sh” | tee visibleresults.txt

##xargs
Takes an input stream (the result(s) of another command - commonly the find command), and then feeds them to another command as indicated.
For example:
find / -name “*.sh” | xargs ls -al > myresults.txt


##locate
locate kavian (locate is not realtime)
sudo updatedb >> to accelerate locate db


##grep
grep oo file.txt
grep -n oo file.txt
grep -i goo >> search with no case sensetivie
grep ^o file.txt
grep -i ^o file.txt
grep s$ file.txt
grep .o file.txt
egrep '^(o|g)' file.txt
egrep -i '^(o|g)' file.txt  =  egrep '^([o|g]|[O|G])' file.txt
egrep '^([a-g]|[A-G])' file.txt
fgrep a$ file.txt
fgrep ^ file.txt

##how to find an error in a file?
grep error file.txt
grep error /var/log/messages
grep -i error /var/log/messages


grep -cv “/bin/bash” /etc/passwd
Would display only lines that do NOT contain the value '/bin/bash' and display the count
you would know the number of user accounts defined that do not have bash listed as the default shell

egrep
grep command without having to specify the -E
-E [ext. regex] - use the indicated extended regular expression for finding a match 

##pgrep
Allows for the testing (debugging) of the pkill command
For example:
pgrep -u root,apache httpd
Will display any httpd process owned by root OR apache

For example:
pgrep -u root apache
Will display only processes owned by root AND apache


##sed
sed -e 's/character1/character2/' file.txt
sed -re 's/^(g|G)/U/' file.txt


##compression
gzip usb-storage.img 
gunzip usb-storage.img.gz

bzip2 usb-storage.img
bunzip usb-storage.bz2

##tar (archiver)
• -c - create the archive 
• -t - displays the contents of the archive 
• -x - extract the contents of the archive 
•  NOTE: one of the three options above is ALWAYS required 
• -f - the name of the file to create 
• -j - compress/uncompress with bzip2 (not always available by default but is the best compression method) 
• -z - compress/uncompress with gzip (available by default and is most commonly used compression method) 
• -v - verbose messages 

tar -cvf files.tar *.jpg
tar -cvf data_backup.tar /data (c compress v verbos f file)
tar -tvf files.tar >> just showing NOT extracting

archiving + compression:
tar -cvzf files.tar.gz ./files
tar -cvjf files.tar.bz2 ./files

tar -xvf files.tar >> extracting
tar -xvzf file.tar.gz
tar -xvjf file.tar.bz2

#create a backup script to backup all data in home directory:

#!/usr/bin/bash
tar -czf backup.tar.gz ~/{Documents,Download,Desktop,Pictures,Videos}

or

tar -czf backup.tar.gz ~/{Desktop,Download,Videos,Documents,Pictures} 2>/dev/null

## location of system configuration files that should be backed up regularly
tar -cvf /dev/tape /etc/

cpio 
Usually used by receiving input from a file or another command and then sends the files to either standard output (default) or a file if redirected 
• -o (or --create) - runs in copy-out mode 
• -O [archivefile] - creates the indicated file instead of using standard output 

#cpio
ls | cpio -o > ./result.cpio
find . "*.zip" | cpio -o > ./result.cpio

For example:
ls -al /home/user/bin/*.sh | cpio -o > scriptbkup.cpio 

Zip:
zip pictures.zip 1.jpg 2.jpg 3.jpg
unzip pictures.zip
(by default zip command can not zip folders)
zip -r folder01.zip folder01

gzip (and/or gzip2)
Compression utility for files and directories 
• -r - recursive, include all files and directories
For example:
gzip /home/user/myfile.txt 

bzip2
A better (in terms of size) compression utility for files and directories
CAUTION: compresses the original file and replaces it with the new compressed version
For example:
bzip2 /home/user/myfile.txt 

xz:
New(er) compression utility for files and directories.
Better performance than bzip2 (but not better compression) 
• -z (or --compress) - compress the file indicated 
• -d (or --uncompress or --decompress) - decompress the file indicated 

xz -z /home/user/myfile.txt 
This would compress the /home/user/myfile.txt file, leaving /home/user/ myfile.txt.xz in its place (the uncompressed version is removed)


##top
• d [#] - run and update the processes display every '#' of seconds 
• i - show only active processes 
• -b - run in batch mode 
• -n [#] - run '#' of updates and exit

For example:
top -b -n 5 > output.txt
Will run top in batch mode and update 5 times, write the results to the output.txt file and exit.

z running process
c absolute path of processes
d to change refresh interval
P sort by CPU
m sort by memory
k to kill pid
h help
top -u kavian


##htop: (colorzied version of top)
sudo yum install htop
htop


##ps
Displays processes that were started and are running as the current or indicated user
ps -eo pid,pcpu,nice,comm,user | grep cal
ps -eo pid,ppid,ni,comm

For example:
ps aux (Linux)
ps -ef (Unix)

ps aux = ps -ef 
ps aux | grep -i postfix | grep -v grep

ps -ux >> current user
ps -aux >> all users
ps -U username

ps aux
kill <PID>
killall <process name>


##priority
• Default process priority is '0' 
Range of priorities is '0' to '19' and '0' to '-20' 
(the 'lower' a process is, the more resources it will receive,
with -20 being the highest possible priority) 
•  Any user can start processes with priorities from 0 to 19 
•  Only root can start processes with priorities from 0 to -20


##nice
Allows a user to start a process with a priority lower than the default (again '0' is default, nice will start with '10') 

For example:
nice myscript.sh
Will start the myscript.sh with a lower priority of '10'
upper priority means lower nice level
-20 < nice level < +19
-20 >> most priority (Realtime in Windows)
sudo nice -n 5 tar -czf backup.tar.gz ./Documents/*
sudo nice --adjustment=5 tar -czf backup.tar.gz ./Documents/*
sudo nice -5 tar -czf backup.tar.gz  ./Documents/*
ps -alx >> showing nice level per process
ps -eo pid,pcpu,nice,comm,user


##renice
Changes the priority of a process
Commonly used to lower a process priority that may be consuming too many resources or to raise a process priority to that it can complete faster

+[#] [PID] - change the priority to a higher number (which is a lower priority)
All users can lower the priority of a running process (raise the number) 
-[#] [PID] - change the priority to a lower number (which is a higher priority) 
Only root can raise the priority of a running process (lower the number)

renice 10 1292

renice -n 14 -p <PID> change nice level during process running
renice -n  -12  -p 1055
renice 17 -u kavian >> change all processess of kavian to 17
renice -n -2  -u apache


##vim editor:
/etc/vimrc
Global vim configuration file

/home/user/.vimrc
User specific configuration for vim

/ forward searching 
? backward searching
n search again
i insert
esc return to command mode
c change letter
ce change word
d cut or delete line or letter
dd cut or delete the whole line
dw cut or delete whole word
yy copy or yank whole line
o insert a new line
p paste 

##mount
The configuration file /etc/fstab contains the necessary information to automate the process of mounting partitions.
In general fstab is used for internal devices, CD/DVD devices, and network shares (samba/nfs/sshfs). Removable devices such as flash drives *can* be added to fstab, but are typically mounted by gnome-volume-manager and are beyond the scope of this document.

vim /etc/fstab
/dev/sdb3 /folder1 ext3 ro,auto,user 0 0

mount -a >> to mount all items in fstab
umount -a >> to un mount all items in fstab

##Sticky bit 
•  Used to keep users that do not own a file from deleting files in a directory they otherwise have group/all write permissions in (changes the 'write' permission for a directory - delete file can only be done by the owner of the file, the owner of the directory or the root user, even if the user would normally have full write permissions to the directory the files are in) 
•  a+t (or +t) - symbolic notation
chmod 1755 file


##SGID(Set Group ID)
Provides group ownership of any NEW file created in a directory to the group owner of the directory (or when applied to a file, permissions to access/run a program as if they were group owner)
g+s - symbolic notation 
chmod 2755 file
chmod g+s file

##SUID - Set User ID
Permits a user to access/run a program as if they were the OWNER of the program
4 - value of the SUID permission
u+s - symbolic notation
chmod 4755 file

##symbol link
ln -s <SOURCE> <LINK_NAME>
#Create a symbolic link to a file: 
ln -s /path/to/file /path/to/symlink 
#Create a symbolic link to a directory: 
ln -s /path/to/directory /path/to/symlink 


ln folder/file.text hardfile.text >> hard link
ln -s folder/file/text softlink.text >> soft link

##profiles
globally >> /etc/profile and /etc/profile.d (profile is for environment variables)
globally functions and aliases >> /etc/bash.bashrc and /etc/bashrc (rc is for functions and aliases)
individually functions and aliases >> /home/user/.bashrc 
environment vairaible for specific user >> /home/user/.bash_profile /.bash_login ./profile
The /etc/skel directory contains files and directories that are automatically copied over to a new user's home directory when such user is created by the useradd program.
/etc/skel >> whatever we put in this folder, every user has it in his/her home folder
env=lists environment variables
set=shows variables and functions (set only shows not set anything)
unset= delete variables 
export= makes a variables an environment variables

## difference between .bash_profile and .bashrc
when you login to a Linux machine, .bash_profile is executed but if you are already logged in and you open a new terminal then .bashrc file is executed.


##three files that are automatically created inside any user's home directory:
.bashc
.bash_profile
.bash_history


set:
Used to view any shell settings or shell variables for the session 
-o [option] - turns the option ON
+o [option] - turns the option OFF
noclobber - default OFF, will not allow the '>' redirect to overwrite an existing
history - default ON, allows HISTFILE to be read to find history file

set
set -o noclobber
set +o noclobber

variables:
hello='Hello World'
echo $hello
this is a regular variables and by reloading shell it disappeard. to make it persistent we should use export.
export hello


##user/group management:
/etc/passwd >> username:password:uid:gid:info:homedir:shell
/etc/group >> groupname:password:gid:member
/etc/shadow >> username:password:timesincepasswwordchange:minpasslifetime:maxpassfiletime:warningdays:inactivedays:accountdisable
/etc/gshadow >> groupname:password:gid:members

##useradd
-d specify home directory
-m create a home directory
-S type of shell
-g group number
-G additional group by name
-u UID
-c comment
useradd -d /home/messi -m -G adm,gdm,cdrom,mysql -s /bin/sh messi
passwd messi

##usermod
-L locking users
-U unlocking users
-aG adding users to group
-s change shell
usermod -s /bin/sh messi
usermod -L messi
usermod -U messi
usermod -aG mygroup messi

##chage
##to view and change the expiry date for user?
-L checks users aging
-E expiration date YYYY-MM-DD
chage -l messi
chage -E 2020-10-10 messi

schedule tasks (cron jobs)
/etc/crontab
crontab -e
/etc/cron.d >> is the place that installed applications put their cron jobs
/etc/cron.{hourly,daily,weekly,monthly}
var/spool/cron 

vi /etc/crontab
(DO NOT INPUT ANY CRON JOB IN THIS FILE)

create cron job:
crontab -e
(min) (hour) (day of a month) (month) (day of a week) (commmand)
* * * * * echo "hello world" >> ~/Desktop/hello.txt

10	*	*	*	*	cat /home/kavian/heyyou.txt | mail -s "<3 U" kavian.k@mtnirancell.ir

*/15 per 15 minutes
*/3 every 3 days



#at
yum -y install at
sudo systemctl start atd
sudo systemctl enable atd
at now +20min
atq
atrm

at 22:00
at 15:00 08/12/2020
atq
atrm 1
/etc/cron.allow
/etc/cron.deny
/etc/at.allow
/etc/at.deny

## crond service
systemctl status crond
systemctl stop crond
systemctl start crond



##To automatically synchronize your server internal clocks using ntp servers online, run the commands below to install NTP client.

sudo yum install ntpdate
sudo ntpdate server 0.ir.pool.ntp.org

Finally, run the commands below to change the hardware clock.
sudo hwclock --systohc


##Packet filtering
iptables -L
iptables -L | grep FORWARD
iptables -P FORWARD DROP
iptables  --flush
iptables -A INPUT --protocol icmp --in-interface enp0s3 -j DROP
iptables -A INPUT --protocol icmp --in-interface enp0s3 -j REJECT

#iptables
iptables -nvl
iptables -nvl -t nat
iptables -A INPUT -s 192.168.12.133 -j DROP
iptables -A INPUT -s 192.168.12.133 -j REJECT
iptables -D INPUT -s 192.168.12.133 -j DROP
iptables -R INPUT 1 -j ACCEPT
iptables -L

vim /etc/iptables/rules.v4
-A INPUT -p tcp -i eth0 -s 192.168.12.0/24 --dport 22 -j ACCEPT
-A INPUT -p tcp -m multiport --dport 8.,443 -j ACCEPT
-A OUTPUT -d 185.60.218.35 -j DROP
-A INPUT -p icmp -i eth0 -j DROP
-A INPUT -p tcp -j DROP

/etc/init.d/iptables-persistent reload
iptables -L

##Firewall settings
systemctl status firewalld
vim /etc/sysconfig/iptables-config
iptables -A INPUT -p tcp -s 0/0 --sport 1024:65535 -d 0/0 --dport 80 -j REJECT
iptables -A OUTPUT -p tcp --dport 80 --sport 1024:65535 -j REJECT
iptables-restore < /etc/sysconfig/iptables

##ss
utility to investigate sockets
ss -t -a >> all tcp sockets that are opened
ss -t -o
ss -tn sport = :80


##nmap
yum install nmap
nmap
nmap -A localhost
nmpa -A -sS localhost

yum install iptraf
iptraf

dstat


##tcpdump
tcpdump
tcpdump -i any
tcpdump -D
tcpdump -i eth0
tcpdump -i eth0 -w date.pcap >> write to a file
tcpdump -i eth0 -r date.pcap >> read the file
tcpdump -i eth0 -c 2 >> capture 2 packets
tcpdump -i eth0 src host 192.168.1.133 >> capture incoming packets from specific host
tcpdump -i eth0 port 22
tcpdump -i eth0 icmp
tcpdump -i eth0 -n >> only IPs


##cronjob
/etc/crontab >> for system not specific user
/etc/cron.d
/etc/cron.{hourly,daily,weekly,monthly}
/var/spool/cron

min/hour/day of month/month/day of week/command

crontab -e

at 22:00
/bin/ping
ctril+d

at now + 1 week
ls
ctrl+d

at 15:00 08/12/2020
at 9:00 April 20
at -c 34
atrm 34

atq

atrm 1 >> remove

ps aux | grep cron
/etc/crontab
crontab -l -u kavian
crontab -e
cd /var/spool/cron/crontabs


##how to remove files older than 7 days by creating a cron job to run every night?
find /var/log/security -type f -mtime +7 -exec rm -rf {} \;
cat /etc/crontab
0 2 * * * /bin/find /var/log/security -type f -mtime +7 -exec rm -rf {} \;


##mail:
to send: mail kavian
to recieve: mail
to set forwarding: nano /etc/aliases
newaliases
or
nano .forward
and put the name of person mail should forward to


##hostname
vim /etc/hostname >> persistent
hostname <newname> >> temporarily

##server hostname
hostname
hostnamectl
hostnamectl set-hostname servername
cat /etc/hostname


##config dns IP:
/etc/resolv.conf
nameserver 192.168.1.1
nameserver 8.8.8.8


##DNS config files and its location
named
/etc/named.conf

##DNS zone files
/var/named

##network
vim /etc/hosts >> local dns sever
vim /etc/resolv.conf >> to set our DNS server (nameserver IPaddress)

vim /etc/nsswitch.conf

vim /etc/sysconfig/network.scripts/ifcfg.eth0 >> redhat
vim /etc/sysconfig/network >> redhat  >>persistent

vim /etc/network/interfaces >> Debian  >>persistent
auto eth0
iface eth0 inet static
	address 172.16.1.244
	netmask 255.255.0.0
	network 172.16.0.0
	gateway 172.16.1.1
	dns-nameservers 8.8.8.8

reboot
or restart the service
service networking restart


##static IP (Debian)
nano /etc/network/interfaces
#iface eth0 inet dhcp
iface eth0 inet static
	address 192.168.1.110
	broadcast 255.255.255.0lsb
	gatewat 192.168.1.1

/etc/inet.d/networking restart


##redhat
nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
BOOTPROTO=static
IPADDR=192.168.1.100
NETMASK=255.255.255.0

systemctl restart network


ifconfig eth up
ifconfig eth down
ifconfig eth0 172.16.1.250 >> temporarily
ifconfig eth0 netmask 255.255.255.0 >> temporarily
ifconfig eth0 hw ether 11:22:33:44:55:66

ifup eth0
ifdown eth0

route
netstat
netstat -n
netstat -tuna

netstat
netstat -a
netstat -a | less
netstat -at | less >> only tcp
netstat -au | less >> only udp


route
route -n
route add default gw 172.16.1.1

dig google.com

#determine the order for DNS queries:
/etc/nsswitch.conf



## ifconfig
ifconfig
ifconfig -a
ifconfig eth01 up
ifconfig eth01 down
ifconfig eth01 192.168.1.10 netmask 255.255.255.0


##NFS_CIFS
server side configuration:
yum install nfs-utils nfs-utils-lib
chkconfig nfs on
service nfs start
cd /home
mkdir share
chmod 755 share
man exports
vim /etc/exports
/home	192.168.1.0/24(rw,no_root_squash)
/var/share	192.168.1.0/24(ro)
exportfs -A
service nfs restart

client side configuration:
yum install nfs-utils nfs-utils-lib
cd /mnt
mkdir nfs_home
mkdir nfs_share
mount 192.168.1.131:/home /mnt/nfs_home/
mount 192.168.1.131:/var/share /mnt/nfs_share/
df -h
or to permanent mount
vim /etc/fstab
192.168.1.131:/home 	/mnt/nfs_home 	nfs 	rw,sync,hard,intr 	0  0
192.168.1.131:/var/nfs 	/mnt/nfs_share  nfs     rw,sync,hard,intr   0  0 
mount -a


##SMTP service
cd /etc/postfix
vim man.cf
myorigin = kavian.mydomain.com

mydestination = $myhostname, localhost.$mydomain, localhost, kavian, kavian.mydomain.com

vim transport
lastline:
kavian	 local:
kavian.mydomain.com     local:
.kavian  local:
.kavian.mydomain.com   local:

vim /etc/hosts
52.3.220.14		kavian.mydomain.com kavian


postmap /etc/postfix/transport



## statically route IP traffic
netstat  -rn
ip route show
traceroute 20.20.20.50

route add -net 172.17.0.0 netmask 255.255.255.0 gw 192.168.1.131 dev enp0s3 (old method)
ip route add 172.17.42.0/24 via 192.168.1.131 dev enp0s3 (new method)

netstat -rn

ip route delete 172.17.0.0/24
ip route delete 172.17.42.0/24


##ssh
default port=22
ssh server config file=/etc/ssh/sshd_config
ssh client config file=/etc/ssh/ssh_config
service sshd restart

apt-get install openssh-server openssh-client
aptitude search openssh-server
vim /etc/ssh/sshd_config

sudo yum install openssh-server

ssh root@192.168.1.110

##change maximum allowed sessions through ssh
uncomment Maxsession 10 in sshd_config
grep MaxSessions /etc/ssh/sshd_config

##disable ssh root login
/etc/ssh/sshd_config
'PermiRootLogin yes' >> 'PermiRootLogin no'
then restart ssh service

##allow only specific users to ssh to your server
/et/ssh/sshd_config
AllowUsers user1 user2
then restart ssh server

##ssh version
ssh -V

## enable password less ssh authentication
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub server2


##SCP:
secure copy
it uses ssh for data transfer
scp file1 server2:/home/user1/Desktop

scp -v sourcefile username@destinationIP:/destinationDirecotoryPath
scp -v ~/folder/file1 user@52.5.99.114:/home/user/recieve
or
scp user@52.5.99.114:/home/user/recieve/file.txt /home/kavian/Desktop/

scp -v /home/user/transfer/file.txt user@52.5.99.114:/home/user/receive
scp user@52.5.99.114:/home/user/transfer/file.txt ~/receive

scp file.txt username@ip:/home/username
scp -r folder username@ip:/home/username

scp username@ip:/home/username/filename.txt /home/kavian/Desktop

##SFTP
cd /etc/ssh
vim sshd_config
uncomment SFTP (Subsystem sftp /usr/lib/openssh/sftp-server)
sftp user@52.5.99.114
get file.txt /home/kavian/Desktop
quit


##keep an eye on something
watch df -h
watch iptables -nvl
watch lsusb

##system's resource usage:
free -m
cat /proc/meminfo | less
du -h
du -h --max-depth=1


##netstat
netstat -ua
netstat -ta
netstat -nra
netstat -nta
netstat -s | less

ss
ss | less
netstat -ua
netstat -ta
netstat -nra
netstat -nta
netstat -s | less

ss
ss | less


##free
Provides information about system memory
• -b (or --bytes) - displays the memory in bytes 
• -k (or --kilo) - displays the memory in kilobytes (default) 
• -m (or --mega) - displays the memory in megabytes 
• -g (or --giga) - displays the memory in gigabytes 
• -h (or --human) - displays the memory if a more 'human-readable' format 
• -c (or --count) - the number of times to display the output (must be used with -s option) 
• -s (or --seconds) - how many seconds between each display output (used with -c) 
• -t (or --total) - display a line showing each columns totals 
• -l (or --lohi) - display low and high memory statistics '


## get virtual and physical memory statistics
free -m
free -g
vmstat -a >> active and inactive
vmstat -d >> disk
vmstat -t >> time

## to check cpu utilization stats
sar -u >> for current day
sar -u 2 3 ( every 2 secs with 3 intervals)
sar -r >> memory
sar -S >> swap space


## list of all open fies by specified user
lsof -u kavian

## list of all open fies by specified command
lsof -c cat

## list of all open fies by specified port
lsof -i 22


## list of created users and send them to a file:
cut -d: -f1 /etc/passwd > users.txt

## list only the 2nd colomn of a file
awk '{print $2}' file.txt

## how to broadcast a message to all logged in users:
wall -n "server is going down!!" 

## how to create a user with no login access?
useradd -s /sbin/nologin kavian

## how to schedule a server reboot in 15 minutes
shutdown -r +15

#reboot at 23:00
shutdown -r 23:00


## what necessary steps should be taken to enhance the security of a server:
update the server packages (yum update)
disable unnecessary services(ftp, sendmail)
enable firewall and configure it
complex password policy
disable root login on ssh

## how to disable ping
temp solution:
echo "1" > /proc/sys/net/ipv4/icmp_echo_ignore_all

permanent solution:
edit the sysctl.conf file and add the following line:
net.ipv4.icmp_echo_ignore_all = 1

execute sysctl -p to enforce this setting immediately.
sysctl -p

## how to check if a port is listening?
netstat -anp | grep 22

## NIC bonding (NIC Teaming)
1. Add two interfaces
2. check status of NetworkManager service
systemctl status NetworkManager
3. List all available network interfaces
nmcli dev status
enp0s8
enp0s9
4. Load bonding driver in Kernel
modprobe bonding
5. verify
modinfo bonding
6. add logical interface "bond0" with load balancing policy "round-robin", IP 192.168.1.120/24 and gateway 192.168.1.1:
nmcli con add type bond con-name bond0 ifname bond0 mode balance-rr \ip4 192.168.1.120/24 gw4 192.168.1.1
7. check file
cat /etc/sysconfig/network-scripts/ifcfg-bond0
8. add two added interfaces as slaves:
add 1st interface enp0s8
nmcli con add type bond-slav ifname enp0s8 master bond0
add 2nd interface enp0s9
nmcli con add type bond-slav ifname enp0s9 master bond0
9.check files
cat /etc/sysconfig/network-scripts/ifcfg-bond-slave-enp0s8
cat /etc/sysconfig/network-scripts/ifcfg-bond-slave-enp0s9
10.activate bond0
nmcli con up bond0
11.show connection info for bond and slaves:
nmcli con show | egrep 'bond0|enp0s8|enp0s9'
12.reboot

## timezone
#list time zones:
timedatectl list-timezone
#set your time to new york time zone
timedatectl set-timezone America/New_York
#check your system time zone
timedatectl


timezone:
tzconfig (deprecated)

tzselect
 
nano /etc/timezone

cd /usr/share/zoneinfo (Timezone file location)
ls
cd Asia
ls
cat Tehran
cat localtime



maintain System time:
data
hwclock

cat /etc/adjtime

to set manually:
hwclock --set --date="03/20/2003 17:30"

to sync from system clock:
hwclock -w

to change local to UTC
hwclock -u -w

to set hwclock from local time 
hwclock --localtime -w

to sync from ntp server:
ntpdate serveraddress

yum install ntp

serviec ntp start (debian)
systemctl start ntpd (Redhat)

nano /etc/ntp.conf
(add ntp server to this file)

systemctl stop ntpd
systemctl start ntpd



##umask
#for regular users
initial permissions = 666 for files
initial permissions = 777 for directories
umask = 002
666 - 002 = 664
777 - 002 = 775


## Write a command that will look for files with an extension "c", and has the occurrence of the string "banana" in it.
find ./ -name "*.c" | xargs grep –i "banana"


## Write a command that will do the following: 
-look for all files in the current and subsequent directories with an extension c,v 
-strip the ,v from the result
-use the result and use a grep command to search for all occurrences of the word “banana” in the files.
find ./ -name "*.c,v" | sed 's/,v//g' | xargs grep "banana"


##static ip Redhat:
#Configure Static IP Address manually using network-scripts (ifcfg-) files
ip addr
#Go to the directory “/etc/sysconfig/network-scripts” and look for the file ‘ifcfg- enp0s8’, if it does not exist then create it with following content:
[root@linuxtechi-rhel8 network-scripts]# vi ifcfg-enp0s8
TYPE="Ethernet"
DEVICE="enp0s8"
BOOTPROTO="static"
ONBOOT="yes"
NAME="enp0s8"
IPADDR="192.168.1.91"
PREFIX="24"
GATEWAY="192.168.1.1"
DNS1="4.2.2.2"
#Save and exit the file and then restart network manager service to make above changes into effect:
systemctl restart NetworkManager
or
service network restart

ip addr show enp0s8

##static ip Debian: 
nano /etc/network/interfaces
#iface eth0 inet dhcp
iface eth0 inet static
	address 192.168.1.110
	broadcast 255.255.255.0
	gatewat 192.168.1.1

/etc/inet.d/networking restart

##static ip Debian (netplan)****
sudo nano /etc/netplan/01-netcfg.yaml
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      dhcp4: no
      dhcp6: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
sudo netplan apply

##Configure a DHCP address with Netplan
Here is the configuration to get the network configuration for IPv4 and IPv6 from a DHCP server.

# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
 version: 2
 renderer: networkd
 ethernets:
   ens33:
     dhcp4: yes
     dhcp6: yes

#To apply the changes, run:
sudo netplan apply


##Network configuration on Ubuntu 12.04 - 17.04
sudo nano /etc/network/interfaces

auto lo eth0
iface lo inet loopback
iface eth0 inet dynamic

#Ubuntu Systems with systemd (like Ubuntu 16.04 and newer), the network interface is named ens33 instead of eth0 now and the word 'dynamic' has been replaced with 'dhcp'.
A configuration where the IP address get's assigned automatically by DHCP will look like this:

# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto ens33
iface ens33 inet dhcp

#Statically configured network cards will have a section like this on older Ubuntu versions:
Here is an example for an older Ubuntu Release:
auto lo eth0
iface lo inet loopback
iface eth0 inet static
	address 192.168.1.100
	netmask 255.255.255.0
	gateway 192.168.1.1

#And here an example for Ubuntu 16.04 and newer:

# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# test

# The primary network interface
auto ens33
iface ens33 inet static
 address 192.168.1.100
 netmask 255.255.255.0
 network 192.168.1.0
 broadcast 192.168.1.255
 gateway 192.168.1.1
 dns-nameservers 8.8.8.8 8.8.4.4


##change default gateway
route add default gw server-ip


echo {Sunday,Monday,Tuesday}.log
echo file{1..3}.txt
echo file{a,b}{1,2}.txt
echo Today is 'date +%A'.
echo The time is ${date +%M} minutes past ${date +%1%p}.

ls a*
ls *a*
ls [ac]*
ls ????

filenames beginning with "b" >> b*
filenames ending in "b" >> *b
Only filenames containing a "b"
filenames where first character is not "b" >> [!b]*
filenames at least 3 character >> ???*
filenames that contains a number >> *[[:digit]]*
filenames that begin with an upper-case >> *[[:upper:]]*


touch tv_season{1..2}_episode{1..6}.ogg

touch mystery_chapter{1..8}.odf

touch {jan,feb,mar,apr,may,jun,jul,aug,sep,oct,nov,dec},{2017..2020}/file{1..100}

mkdir Videos/season{1..2}

mv tv_season1* Videos/season1
mv tv_season2* Videos/season2

pidof <name of the process>
kill <pid>
kill -KILL <pid> >> force kill
kill -9 <pid>

#cal
cal -A 1 12 2018 >> 1 month after
cal -B 1 12 2018 >> 1 month before


##history
history -c; history -w  >> to clear history

##sort
**you can sort tabular data using the -k option
ls -l /etc | head -n 20 | sort -k 5nr
ls -lh /etc | head -n 20 | sort -k 5hr

ls -lh /etc | head -n 20 | sort -k 6M


SQL
apt-get install mysql-server
service mysql start
mysql -u root
SHOW DATABASES;
USE mysql;
SHOW TABLES;
CREATE DATABASE itpro_db;
SHOW DATABASES;
USE itpro_db;
SHOW TABLES;
CREATE TABLE forum (id INT(20), forum CHAR(20), video CHAR (20));
CREATE TABLE users (user CHAR(20), record INT(20), activity CHAR(20));
SHOW TABLES;
SELECT * FROM forum;
INSERT INTO forum (id,forum,video) VALUES ('1','network','500')
INSERT INTO forum (id,forum,video) VALUES ('2','linux','600')
INSERT INTO forum (id,forum,video) VALUES ('3','icdl','600')
INSERT INTO forum (id,forum,video) VALUES ('4','graphic','800')
SELECT * FROM forum;
INSERT INTO users (user,record,activity) VALUES ('kavian','100','800')
INSERT INTO users (user,record,activity) VALUES ('kamran','200','800')
INSERT INTO users (user,record,activity) VALUES ('ali','300','800')
INSERT INTO users (user,record,activity) VALUES ('hossein','400','800')
SELECT * FROM users;

USE itpro_db;
SELECT * FROM forum;
SELECT * FROM users;
SELECT id FROM forum;
SELECT * FROM forum WHERE forum = 'linux';
SELECT * FROM forum WHERE forum != 'linux';
SELECT * FROM forum ORDER BY video;
SELECT * FROM users GROUP BY activity;
UPDATE users SET user='kaviani' WHERE user='kavian';
DELETE FROM users WHERE user='ali';
SELECT * FROM forum JOIN users ON forum.video=users.record;


#syslogging:
nano /etc/syslog.conf

to be a listener:
nano /etc/sysconfig/syslog
(-r allow remote logging)

to configure a server as syslog server:
nano /etc/rsyslog.conf
uncomment below ones:
$ModLoad imudp
$UDPServerRun 514
$ModLoad imtcp
$TCPServerRun 514

service rsyslog restart

to configure a server to send logs to a syslog server:
nano /etc/rsyslog.conf
add below one:
#Remote logging TCP/UDP
*.*	@syslogserverIP:514 (UDP)
*.*	@@syslogserverIP:514 (TCP)

service rsyslog restart

cd /var/log

/var/log/messages

manually generate log:
logger comment


MTA:
mail

mail forwarding:
nano /etc/aliases
receiver:forwarder
newaliases

manually forwarding setting:
nano .forward
name of receiver user

newaliases

Printers:
localhost:631

cd /etc/cups
ls
nano cupd.conf
nano printer.conf

lpq
lpq -a
lpq -Pprintername
lprm 3 (jobnumber)

lpr -PHP file.txt
lpq -a

remove all printer job on a printer:
lprm -PHP -
lpq -a

lpc status all

to reject queuing:
cupsreject HP

cupsaccept HP

disable a printer:(pausing)
cupsdisable HP

cupsenable HP

move a job to another printer:
lpmove 18 HP (HP is job id)

lpmove firstPrinter secondprinter
