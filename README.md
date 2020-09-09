# SUID-Capabilities-Seccomp

# 1. SUID
### Setting Password

`passwd`

![alt text](/Screenshots/01-demo-1.png?raw=true "Setting Password")

### Check passwd Process

`ps aux | grep passwd`

![alt text](/Screenshots/02-demo-01.png?raw=true "Setting Password")

### Check SUID bit

`which passwd`

`ls -l /usr/bin/passwd`

![alt text](/Screenshots/03-demo-01.png?raw=true "Setting Password")

# 2. Capabilities
## Capabilities Of Host System

### Getting, Setting Capabilities & Privilege Escalation of Python 

`getcap /usr/bin/python3.8`

`sudo setcap cap_setuid+ep /usr/bin/python3.8`

`python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'`

`id`

![alt text](/Screenshots/04-demo-02.png?raw=true "Setting Password")

## Capabilities Using Docker

### Without Capabilities Container

`docker run -it alpine alpine`

![alt text](/Screenshots/05-demo-03.png?raw=true "Setting Password")

### Dropping Capabilities CAP_NET_RAW

`docker run -it --name alpine --cap-drop CAP_NET_RAW  alpine`

![alt text](/Screenshots/06-demo-03.png?raw=true "Setting Password")

### Adding Capabilities CAP_LINUX_IMMUTABLE

`docker run -it --name ubuntu --cap-add CAP_LINUX_IMMUTABLE ubuntu`

`touch touch.txt`

`chattr +i touch.txt`

`lsattr touch.txt`

`rm touch.txt`

`exit`

![alt text](/Screenshots/07-demo-03.png?raw=true "Setting Password")

### Try To Delete Container
`docker ps -a`

`docker rm ubuntu`

![alt text](/Screenshots/08-demo-03.png?raw=true "Setting Password")

### Try To Delete Container By Removing CAP_LINUX_IMMUTABLE Capability

`sudo su -`

`chattr -i /var/lib/docker/overlay2/<filepath>/diff/test.txt`

`lsattr /var/lib/docker/overlay2/<filepath>/diff/test.txt`

`docker rm ubuntu`

![alt text](/Screenshots/09-demo-03.png?raw=true "Setting Password")

# Seccomp

## Seccomp Using Program

### Seccomp Without Specific Paramters

`gcc -o nothing nothing.c`

`./nothing`

![alt text](/Screenshots/10-demo-04.png?raw=true "Setting Password")

### Installing Library Seccomp

`sudo apt-get install libseccomp-dev`

### Seccomp With Specific Paramters

`gcc -o bpf-only-dup bpf-only-dup-sudo.c -lseccomp`

`./bpf-only-dup`

![alt text](/Screenshots/11-demo-04.png?raw=true "Setting Password")


## Seccomp Using Docker

### Container Without Seccomp Profile Applied

`docker run -it --name ubuntu ubuntu`

`ls`

`mkdir test`

`rm test`

`exit`

`docker rm ubuntu`

![alt text](/Screenshots/12-demo-05.png?raw=true "Setting Password")

### Container With Seccomp Profile Applied

`docker run -it --name ubuntu --security-opt seccomp=profile.json ubuntu`

`ls`

`mdkir test`

![alt text](/Screenshots/13-demo-05.png?raw=true "Setting Password")








