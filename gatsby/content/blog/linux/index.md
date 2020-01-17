---
title: Linux quick start
date: "2020-01-15T22:12:03.284Z"
description: "Some basic inforamtion to get started with Linux"
---

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

**few useful commands**

pwd or echo $PWD - command which displays the current (present) working directory

env - displays all environment variables

| - pipe redirects the output of the command to the next command
eg: env | less (less is a file reader which will display data page by page)

history - displays last run 1500 commands
timedatectl - displays the current time zone used by machine

ls -a or ls --all   => lists all files including hidden => . int he beginning of the file name means the file is hidden
ip add (ip is the command addr argument gets the ip address or you can use ip a)
ls -l (list all properties as well)
ls -l /filelocation
You can combine aregumets passsed to the command like ls -ltr

Type cd from anywhere and you will be back to your home directory

touch file1 file2 file3 - creates new files
rm *
removes all files in directory

backslash - escapa character
touch this\ is\ my\ file => file 'this is my file' created

mkdir folderame

nano is a utility that can be sued to create and edit files on linux.
nano file1
ctrl+x  => y for save

touch filename to create empty file

copy file
cp file1 dirname
cp file* dirname
cp file? dirname  - questionmark for single character matching,  * for multiple character matching

mv - move

. or ./ current dir
.. or ../ previous dir

rmdir to remove diretcory - directory shud be empty

Search the files
locate command
locate 'string'  (run sudo updatedb to get recent files also)

less filename -> prints one page at a time
head filename -> prints only first 10 lines
tail filename -> prints last 10 lines

pipe
cat filename | grep string >> newfilename
>> append
> replace

wc - wordcount 

standard stream
standardin - stdin - 0
stdout - 1
stderr -2

redirection from stdin / stdout
mysql -u root -p <password.txt
echo Hello > newfile

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


**shells** 

myval=5 (no space in between. This will assign 5 to myval in that bash shell)
export myval=5 will set the value for all child shells also

When a new terminal is opened, a new shell is formed using a hidden file in home/user directory called .bashrc
If you want to change any shell values (eg: colour of the shell), this is one of the files to edit.
.bashrc file is applied to only non login shell.
Login shell is the shell you are in use ssh login to login. This uses .profile file and some other files. 
.profile uses.bashrc values as well/. .profile is also in the same location as .bashrc
There is a profile file in /etc also

There are other shells also like ksh, sh
To find out the default shell, check the passwd file in /etc.

**working with archives**

In Linux -> tar is the archival mechanism (like zip in windows)
most files are .tar.gz
.tar tar archive
.gz tells its compressed  using gzip algorithm

To unpack
tar xzf <filename>
x = xtract
z = zipped
f - filename is following 

To create new archive
tar czf newarchive.tar.gz foldernametocompress
c  create

2 steps archive si as follows:
tar cf
and then
gzip tar file

**Networking**

Install iproute2 package if ip command not working
ip route show => another command to view the ip address
sample output:
default via 172.17.0.1 dev eth0
172.17.0.0/16 dev eth0 proto kernel scope link src 172.17.0.2

172.17.0.2 is the ip address
if the above two not present, there is some networking issue

ip addr -> also displays ip address
sample output:
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

ifconfig - another command similar to ip addr

netstat -i
displays all network interfaces

netstat -l lists all listening ports

DNS
DNS (Domain Name Servers) map numeric ip addresses to human readabale addresses.
DSN servers maintain that mapping.

To check if dns is reachable from your machine, use host command

sample output of host command
host google.com
google.com has address 172.217.12.142
google.com has IPv6 address 2607:f8b0:4006:819::200e
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 10 aspmx.l.google.com.
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.


/etc/hosts - This file can be used to set up your own domain names locally. This will override DNS mappings
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.17.0.2      e838c710c3d6

**Remote connections and ssh**

Openssh - provides capabilities for SSH as well as session security (session encryption)

ssh command - ssh username@ipaddress
Ttype password when prompted to login

Login using keypair to an amazon server / aws instance
ssh -i .home/mykey.pem username@ipaddress


SCP - Secure Copy
Securely copies files between machines
scp localfilelocation username@ipaddress:remotefilelocation

**linux scripting / bash scripting**

Naming conventionf or scripting files is myfile.sh
.sh a convention to identify the scripting files and not a system requirement.

First line of the script file is to tell linux that its a script file and which shell interpreter has to be used
#!/bin/bash  - (actual location of the bash interpreter)
This line is called shebang (read as shbang)
few scripting commands:
declare -i number1 (integer is -i)
read number1  (wait for user input)
use $number1 to access the actual value

chmod +x filename.sh  (to make it executable)

how to run a script file
 ./filename.sh
