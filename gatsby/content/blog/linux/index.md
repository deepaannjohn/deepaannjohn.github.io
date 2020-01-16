---
title: Linux
date: "2020-01-15T22:12:03.284Z"
description: "Linux quick start"
---

Some basic Linux and a few commands to help beginners in Linux!

**Linux File System**
All files and diectories including OS directories and files in Linux are under the root directory (/)
ls / will list of them

Some of the common root level directories are
/bin 
/sbin
/boot
/dev 
/etc  - configuration files 
/home - user files will be in the sub directories in this folder
/lib  - software library
/root - root user's files
/usr  - additional binaries
/var  - logs, cache etc
/proc 
/dev
/sys

**few commands**
pwd or echo $PWD - command which displays the current (present) working directory

env - displays all environment variables

| - pipe redirects the output of the command to the next command
eg: env | less (less is a file reader which will display data page by page)

myval=5 (no space in between. This will assign 5 to myval in that bash shell)
export myval=5 will set the value for all child shells also

history - displays last run 1500 commands
timedatectl - displays the current time zone used by machine

man <command> - to get help use manualpages 
or wget --help
info - to get the info for all installed utilities


**package managers**
debian based (ubuntu, mint and kali) - apt (online)/dpkg(local)
rhel/fedora/centos - yum or dnf(online) / rpm (local) - dnf will eventually replace yum

**desktops**
cinnamon / mate
kde
gnome (ubuntu uses it)
user can install and easily switch between any of these desktops


**Are you on windows? Want to quickly try these?**
Pull an ubuntu image and run it
docker run -it ubuntu bash (prefix winpty if you are using Git Bash on windows)



when a new terminal is opened, a new shell is formed using a hidden file in home directory called .bashrc
its in home/user 
for root
its in /root folder
if you want to changeany shell values, this is one of the files to edit
this .bashrc file is applied to only non login shell
login shell is when you use ssh login to login. this uses .profile file and some other files. .profile uses.bashrc values as well/. .profile is also in the same location as .bashrc
there is a profile file in /etc also

there are other shells also like ksh, sh

to find pout the shell defaulty allocated,
check the passwd file in /etc


ls -a or ls --all   => list all files including hidden => . means file is hidden
ip add (ip is the command addr argument gets the ip address or you can use ip a)
ls -l (list all propertieties 
ls -l /filelocation
combine commands ls -ltr
command line shortcuts
use tab for auto complete

type cd from anywhere and you will be back to your home directory

touch file1 file2 file3 - creates new files
rm *
removes all file sin dir

backslash - escapa character
touch this\ is\ my\ file


mkdir folderame

new file creation
nano file1
ctrl+x  => y for save


touch filename to create empty file


copy file
cp file1 dirname
cp file* dirname

questionmark for single digit

mv - move
. or ./ current dir
.. or ../ previous dir


rmdir to remove diretcory - directory shud be empty

searcj the files
locate command
locate 'string'  (run sudo updatedb to get recent files also)

less filename - prints a page a t a time
head - only 10 lines
tail last

pipe
cat filename | grep string >> newfilenae
>> append
> replace

nano filename


wc - wordcount 


standard stream
standard in - stdin - 0
stdout - 1
stderr -2

redirection
mysql -u root -p <password.txt

echo Hello > newfile


working with archives
in liux -> tar is the archival mechanism
most files are .tar.gz
.tar tar archive
.gz tells its compressed  using gzip also

to unpack
tar xzf <filename>
x = xtract
z = zipped
f - filename is following 

tar czf newarchive.tar.gz foldernametocompress
c  create

2 steps archive
tar cf
and then
gzip tar file


network
install iproute2 package if ip command not working
ip route show
root@e838c710c3d6:~# ip route show
default via 172.17.0.1 dev eth0
172.17.0.0/16 dev eth0 proto kernel scope link src 172.17.0.2

172.17.0.2 is the ip address

if the above two not present, there is come issue

ip addr - to shw ip addres
root@e838c710c3d6:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1
    link/ipip 0.0.0.0 brd 0.0.0.0
3: ip6tnl0@NONE: <NOARP> mtu 1452 qdisc noop state DOWN group default qlen 1
    link/tunnel6 :: brd ::
1086: eth0@if1087: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

1 => local loopback
1086 => ip address


ifconfig similar to ip addr

netstat -i
all network inerfaces

netstat -l list all prorts


dns
map numeric ip addresses to human readabale addresses
for that db would be needed and dns servers maintain that mapping

to check if dns is reachable from my machine..use host commadn


install host 
host google.com
google.com has address 172.217.12.142
google.com has IPv6 address 2607:f8b0:4006:819::200e
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 10 aspmx.l.google.com.
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.



root@e838c710c3d6:~# cat /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.17.0.2      e838c710c3d6


use this file to se your own domain names


remote connections and ssh
openssh - provides capabilities for ssh as well as security (session encryption)

hot to do ssh
ssh username@ipassres
then type password when propted

login using keypair to an amazon server / aws instance
ssh -i .home/mykey.pem username@ipaddress



scp
securely copy
scp - securely copy file between machine
scp localfilelocation username@ipaddress:remotefilelocation

linux scription / bash scripting
names will be like myfile.sh
sh is not necessary thiugh..its just to identify script file

first line of the script file is to tell linux that its a script file and which shell interpreter has to be used
#!/bin/bash  -(actual location of the bash interpreter)
this line is called shebang (read as shbang)
decalre -i number1 (integer is -i)
read number1  (wait for user input)
exit 0 (to exit) (not necessary)
use $number1 to access the actual value

chmod +x filename.sh  (to make it executable)

how to run run
 ./filename.sh
