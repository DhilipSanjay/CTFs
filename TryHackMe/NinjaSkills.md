# Ninja Skills

**Date:** 12, June, 2021

**Author:** Dhilip Sanjay S

---

[Click Here](https://tryhackme.com/room/ignite) to go to the TryHackMe room.

## SSH Login

```bash
ssh new-user@10.10.207.108
The authenticity of host '10.10.207.108 (10.10.207.108)' can't be established.
ECDSA key fingerprint is SHA256:Uk/7Yw38zrwwZM+BdODYPaKhM4fqgXm3gIxGpgMvupo.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.207.108' (ECDSA) to the list of known hosts.
new-user@10.10.207.108's password: 
Last login: Wed Oct 23 22:13:05 2019 from ip-10-10-231-194.eu-west-1.compute.internal
████████╗██████╗ ██╗   ██╗██╗  ██╗ █████╗  ██████╗██╗  ██╗███╗   ███╗███████╗
╚══██╔══╝██╔══██╗╚██╗ ██╔╝██║  ██║██╔══██╗██╔════╝██║ ██╔╝████╗ ████║██╔════╝
   ██║   ██████╔╝ ╚████╔╝ ███████║███████║██║     █████╔╝ ██╔████╔██║█████╗  
   ██║   ██╔══██╗  ╚██╔╝  ██╔══██║██╔══██║██║     ██╔═██╗ ██║╚██╔╝██║██╔══╝  
   ██║   ██║  ██║   ██║   ██║  ██║██║  ██║╚██████╗██║  ██╗██║ ╚═╝ ██║███████╗
   ╚═╝   ╚═╝  ╚═╝   ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝╚═╝     ╚═╝╚══════╝
        Let the games begin!
[new-user@ip-10-10-207-108 ~]$ ls
files
```

---


## Locate all the files

```bash
[new-user@ip-10-10-207-108 shm]$ cat files 
8V2L
bny0
c4ZX
D8B3
FHl1
oiMO
PFbD
rmfX
SRSq
uqyw
v2Vb
X1Uy

$ [new-user@ip-10-10-207-108 shm]$ while read -r name; do find / -name $name 2>/dev/null 1>>locations ; done < files

[new-user@ip-10-10-207-108 shm]$ cat locations 
/etc/8V2L
/mnt/c4ZX
/mnt/D8B3
/var/FHl1
/opt/oiMO
/opt/PFbD
/media/rmfX
/etc/ssh/SRSq
/var/log/uqyw
/home/v2Vb
/X1Uy
while read -r name; do find / -name $name 2>/dev/null 1>>locations ; done < files
```


## Questions

### Which of the above files are owned by the best-group group(enter the answer separated by spaces in alphabetical order)
- **Answer:** D8B3 v2Vb
- **Steps to Reproduce:** 

```bash
[new-user@ip-10-10-207-108 home]$ find / -group best-group 2>/dev/null 
/mnt/D8B3
/home/v2Vb
```

---

### Which of these files contain an IP address?
- **Answer:** oiMO
- **Steps to Reproduce:** 
    - `H` option in grep -> Prints the file name
    - `E` - Extended Regex

```bash
[new-user@ip-10-10-207-108 shm]$ while read -r file; do grep -EHo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' $file ; done < locations
/opt/oiMO:1.1.1.1
```

---

### Which file has the SHA1 hash of 9d54da7584015647ba052173b84d45e8007eba94
- **Answer:** c4ZX
- **Steps to Reproduce:** 

```bash
[new-user@ip-10-10-207-108 shm]$ while read -r file; do sha1sum $file ; done < locations
0323e62f06b29ddbbe18f30a89cc123ae479a346  /etc/8V2L
9d54da7584015647ba052173b84d45e8007eba94  /mnt/c4ZX
2c8de970ff0701c8fd6c55db8a5315e5615a9575  /mnt/D8B3
d5a35473a856ea30bfec5bf67b8b6e1fe96475b3  /var/FHl1
5b34294b3caa59c1006854fa0901352bf6476a8c  /opt/oiMO
256933c34f1b42522298282ce5df3642be9a2dc9  /opt/PFbD
4ef4c2df08bc60139c29e222f537b6bea7e4d6fa  /media/rmfX
acbbbce6c56feb7e351f866b806427403b7b103d  /etc/ssh/SRSq
57226b5f4f1d5ca128f606581d7ca9bd6c45ca13  /var/log/uqyw
7324353e3cd047b8150e0c95edf12e28be7c55d3  /home/v2Vb
59840c46fb64a4faeabb37da0744a46967d87e57  /X1Uy
```

---

### Which file contains 230 lines?
- **Answer:** bny0
- **Steps to Reproduce:** 
    - Only one file is missing here. That file must have the size of 230!

```bash
[new-user@ip-10-10-207-108 shm]$ while read -r file; do wc -l $file ; done < locations
209 /etc/8V2L
209 /mnt/c4ZX
209 /mnt/D8B3
209 /var/FHl1
209 /opt/oiMO
209 /opt/PFbD
209 /media/rmfX
209 /etc/ssh/SRSq
209 /var/log/uqyw
209 /home/v2Vb
209 /X1Uy
```

---

### Which file's owner has an ID of 502?
- **Answer:** 
- **Steps to Reproduce:** 

```bash
[new-user@ip-10-10-207-108 shm]$ while read -r file; do echo $file && ls -l $file | awk '{print $3}' | xargs id ; done < locations
/etc/8V2L
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/mnt/c4ZX
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/mnt/D8B3
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/var/FHl1
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/opt/oiMO
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/opt/PFbD
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/media/rmfX
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/etc/ssh/SRSq
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/var/log/uqyw
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/home/v2Vb
uid=501(new-user) gid=501(new-user) groups=501(new-user)
/X1Uy
uid=502(newer-user) gid=503(newer-user) groups=503(newer-user)
```

---

### Which file is executable by everyone?
- **Answer:** 
- **Steps to Reproduce:** 

```bash
[new-user@ip-10-10-207-108 shm]$ while read -r file; do find $file -perm -o=x ; done < locations
/etc/8V2L
```

---