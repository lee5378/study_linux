# How to use awk

These notes are inspired by Derek Taylor's YouTube video, "[Learning Awk is Essential For Linux Users](https://www.youtube.com/watch?v=9YOZmI-zWok)." The below examples were run in Ubuntu Linux Server 20.10.

## Returning a specific column with awk

`ps` prints to the screen some information about active processes in a tabular format with columns and headers.

```
ghost@practice1:~$ ps
    PID TTY          TIME CMD
   9522 pts/0    00:00:00 bash
  57321 pts/0    00:00:00 ps
```

`awk` is great at interacting with data sets that come formatted in columns, such as that produced by `ps`. Let's try our first `awk` command in the form of `ps | awk '{print $1}'`:

```
ghost@practice1:~$ ps | awk '{print $1}'
PID
9522
57325
57326
```

Notice that we've filtered the output of `ps` down to strictly the `PID` column, which is the first column. By default, `awk` recognizes a space character as the column delimiter. The number that you specify after the dollar sign tells `awk` which column to print to the screen, in this case, the first. Note that by entering a value of `0` you'll be able to return all columns.

## Messing with column delimiters

Let's try something a little more difficult. The file, `etc/passwd`, stores user account information. This is a very important file in Linux from a security standpoint. If you are unfamiliar with this file, [Linuxize](https://linuxize.com/post/etc-passwd-file/) has a great writeup where they break down what each column means. Let's see what it looks like using `cat /etc/passwd`: 

```
ghost@practice1:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:101:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
usbmux:x:111:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
sshd:x:112:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
ghost:x:1000:1000:ghost:/home/ghost:/bin/bash
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
```

Because this data set uses `:` instead of a space character as the column delimiter, we'll need to tell `awk` about this so it can function as desired. The `-F` option tells `awk` to use a different column delimiter. Let's try `awk -F ":" '{print $1}' /etc/passwd` and see if we can return only the first column of `etc/passwd`:

```
ghost@practice1:~$ awk -F ":" '{print $1}' /etc/passwd        
root                                                          
daemon                                                        
bin                                                           
sys                                                           
sync                                                          
games                                                         
man                                                           
lp                                                            
mail                                                          
news                                                          
uucp                                                          
proxy                                                         
www-data                                                      
backup                                                        
list                                                          
irc                                                           
gnats                                                         
nobody                                                        
systemd-timesync                                              
systemd-network                                               
systemd-resolve                                               
messagebus                                                    
syslog                                                        
_apt                                                          
tss                                                           
uuidd                                                         
tcpdump                                                       
landscape                                                     
pollinate                                                     
usbmux                                                        
sshd                                                          
systemd-coredump                                              
ghost                                                         
lxd                                                           
```

This returned a list of usernames for this computer, a very useful piece of information. We can return multiple columns from `etc/passwd` and change the column delimiter from a colon to a tab using this command, `awk -F ":" '{print $1"\t"$6"\t"$7}' /etc/passwd`:

```
ghost@practice1:~$ awk -F ":" '{print $1"\t"$6"\t"$7}' /etc/passwd
root    /root   /bin/bash
daemon  /usr/sbin       /usr/sbin/nologin
bin     /bin    /usr/sbin/nologin
sys     /dev    /usr/sbin/nologin
sync    /bin    /bin/sync
games   /usr/games      /usr/sbin/nologin
man     /var/cache/man  /usr/sbin/nologin
lp      /var/spool/lpd  /usr/sbin/nologin
mail    /var/mail       /usr/sbin/nologin
news    /var/spool/news /usr/sbin/nologin
uucp    /var/spool/uucp /usr/sbin/nologin
proxy   /bin    /usr/sbin/nologin
www-data        /var/www        /usr/sbin/nologin
backup  /var/backups    /usr/sbin/nologin
list    /var/list       /usr/sbin/nologin
irc     /var/run/ircd   /usr/sbin/nologin
gnats   /var/lib/gnats  /usr/sbin/nologin
nobody  /nonexistent    /usr/sbin/nologin
systemd-timesync        /run/systemd    /usr/sbin/nologin
systemd-network /run/systemd    /usr/sbin/nologin
systemd-resolve /run/systemd    /usr/sbin/nologin
messagebus      /nonexistent    /usr/sbin/nologin
syslog  /home/syslog    /usr/sbin/nologin
_apt    /nonexistent    /usr/sbin/nologin
tss     /var/lib/tpm    /bin/false
uuidd   /run/uuidd      /usr/sbin/nologin
tcpdump /nonexistent    /usr/sbin/nologin
landscape       /var/lib/landscape      /usr/sbin/nologin
pollinate       /var/cache/pollinate    /bin/false
usbmux  /var/lib/usbmux /usr/sbin/nologin
sshd    /run/sshd       /usr/sbin/nologin
systemd-coredump        /       /usr/sbin/nologin
ghost   /home/ghost     /bin/bash
lxd     /var/snap/lxd/common/lxd        /bin/false
```

Here is another technique you can use to alter the column delimiter character while selecting specific columns to output. In this example, we'll change all colons to hyphens and only return columns 1, 6, and 7 using the command `awk 'BEGIN{FS=":"; OFS="-"} {print $1,$6,$7}' /etc/passwd`:

```
ghost@practice1:~$ awk 'BEGIN{FS=":"; OFS="-"} {print $1,$6,$7}' /etc/passwd 
root-/root-/bin/bash                                                         
daemon-/usr/sbin-/usr/sbin/nologin                                           
bin-/bin-/usr/sbin/nologin                                                   
sys-/dev-/usr/sbin/nologin                                                   
sync-/bin-/bin/sync                                                          
games-/usr/games-/usr/sbin/nologin                                           
man-/var/cache/man-/usr/sbin/nologin                                         
lp-/var/spool/lpd-/usr/sbin/nologin                                          
mail-/var/mail-/usr/sbin/nologin                                             
news-/var/spool/news-/usr/sbin/nologin                                       
uucp-/var/spool/uucp-/usr/sbin/nologin                                       
proxy-/bin-/usr/sbin/nologin                                                 
www-data-/var/www-/usr/sbin/nologin                                          
backup-/var/backups-/usr/sbin/nologin                                        
list-/var/list-/usr/sbin/nologin                                             
irc-/var/run/ircd-/usr/sbin/nologin                                          
gnats-/var/lib/gnats-/usr/sbin/nologin                                       
nobody-/nonexistent-/usr/sbin/nologin                                        
systemd-timesync-/run/systemd-/usr/sbin/nologin                              
systemd-network-/run/systemd-/usr/sbin/nologin                               
systemd-resolve-/run/systemd-/usr/sbin/nologin                               
messagebus-/nonexistent-/usr/sbin/nologin                                    
syslog-/home/syslog-/usr/sbin/nologin                                        
_apt-/nonexistent-/usr/sbin/nologin                                          
tss-/var/lib/tpm-/bin/false                                                  
uuidd-/run/uuidd-/usr/sbin/nologin                                           
tcpdump-/nonexistent-/usr/sbin/nologin                                       
landscape-/var/lib/landscape-/usr/sbin/nologin                               
pollinate-/var/cache/pollinate-/bin/false                                    
usbmux-/var/lib/usbmux-/usr/sbin/nologin                                     
sshd-/run/sshd-/usr/sbin/nologin                                             
systemd-coredump-/-/usr/sbin/nologin                                         
ghost-/home/ghost-/bin/bash                                                  
lxd-/var/snap/lxd/common/lxd-/bin/false                                      
```

## Shells

The `/etc/shells` file lists the full file paths to all installed shells on the computer.

```
ghost@practice1:~$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
```

What if this is too much information, and I'd prefer to view a list of shells and not their file paths? We can use `awk` for this purpose. This time we'll need to play around the `/` character as the field separator. We want to tell `awk` to only return the final string in the file path, which is the string after the last occurrence of `/`. Starting with `awk -F '/ /'` we'll need to add a pattern in between the forward slashes that `awk` will search for. Let's find the beginning of every line that starts with `/` which is accomplished using the `^` symbol: `awk -F "/" '/^\/ /'`. Note that we had to use an escape character (`\`) to search for an actual forward slash instead of ending the search pattern prematurely.

The command to do this is `awk -F "/" '/^\// {print $NF}' /etc/shells`:

```
ghost@practice1:~$ awk -F "/" '/^\// {print $NF}' /etc/shells
sh
bash
bash
rbash
rbash
dash
dash
tmux
screen
```

We're almost done, but you may have noticed there are duplicate entries in the list of shells. Additionally, there is no sorting taking place. Let's fix that by adding piping as well as the `uniq` and `sort` operators.

```
ghost@practice1:~$ awk -F "/" '/^\// {print $NF}' /etc/shells | uniq | sort
bash
dash
rbash
screen
sh
tmux
```

## String searches and other operations

The `df` command returns information about the Linux file system in a nice column-formatted output.

```
ghost@practice1:~$ df
Filesystem                        1K-blocks    Used Available Use% Mounted on
tmpfs                                100092    1088     99004   2% /run
/dev/mapper/ubuntu--vg-ubuntu--lv   9219412 4951888   3779488  57% /
tmpfs                                500444       0    500444   0% /dev/shm
tmpfs                                  5120       0      5120   0% /run/lock
tmpfs                                  4096       0      4096   0% /sys/fs/cgroup
/dev/sda2                            999320  221548    708960  24% /boot
tmpfs                                100088       4    100084   1% /run/user/1000
```

Now let's search for the string `/dev/` and only return the first column. Once again we'll need to use a search string with escape characters.

```
ghost@practice1:~$ df | awk '/\/dev\// {print $1}'
/dev/mapper/ubuntu--vg-ubuntu--lv
tmpfs
/dev/sda2
```

Let's print the first, third, and fourth columns. We'll also include tabs between the columns.

```
ghost@practice1:~$ df | awk '/\/dev\// {print $1"\t"$3"\t"$4}'
/dev/mapper/ubuntu--vg-ubuntu--lv       4951888 3779488
tmpfs   0       500444
/dev/sda2       221548  708960
```

We can perform mathematical operations against multiple columns returned by `awk`. In our case, let's add the third and fourth columns together.

```
ghost@practice1:~$ df | awk '/\/dev\// {print $1"\t"$3+$4}'
/dev/mapper/ubuntu--vg-ubuntu--lv       8731376
tmpfs   500444
/dev/sda2       930508
```

`awk` can count the number of characters in a line and only return lines that meet the criteria. Let's filter `etc/shells` for lines with more than seven characters.

```
ghost@practice1:~$ awk 'length($0) > 7' /etc/shells
# /etc/shells: valid login shells
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
```

Much like `grep`, `awk` can read a file and return only lines that match a user-provided criteria. For this example, we can write a statement where if the last field matches the string criteria, print the line (signified by the `print $0` command).

```
ghost@practice1:~$ ps -ef | awk '{ if($NF == "[loop7]") print $0}'
root        1852       2  0 Jun28 ?        00:00:00 [loop7]
```

# Scripting awk

We can use a for loop within an `awk` command. In this example, we're iterating through the integers 1 through 10 and squaring the number.

```
ghost@practice1:~$ awk 'BEGIN { for(i=1; i<=10; i++) print i, "squared is", i*i;}'
1 squared is 1
2 squared is 4
3 squared is 9
4 squared is 16
5 squared is 25
6 squared is 36
7 squared is 49
8 squared is 64
9 squared is 81
10 squared is 100
```

What about returning only the lines that begin with a specific character? In this example, we're searching for lines that begin with a `b` or a `c` and then returning the full lines to the screen. The target of our search is the user's `.bashrc` file.

```
ghost@practice1:~$ awk '$1 ~ /^[b,c]/ {print $0}' .bashrc
case $- in
case "$TERM" in
        color_prompt=yes
        color_prompt=
case "$TERM" in
```
