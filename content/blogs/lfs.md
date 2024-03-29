---
title: "Challenges in linuxfromscratch"
date: 2020-04-11T13:55:52+05:30
draft: false
type: "post"
tags: ["linux","linuxfromscratch","lfs","LFS","linuxfromscratch.org","ubuntu"]
ogtype: "article"
slug: "challenges-in-linuxfromscratch"
categories: ["Linux"]
cover:
    image: "https://pbs.twimg.com/media/EWmoua6UYAAjwgC?format=jpg&name=large" # image path/url
    alt: "Linux from scratch login screen" # alt text
    caption: "LFS main screen" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page

---

![lfs_screen](https://pbs.twimg.com/media/EWmoua6UYAAjwgC?format=jpg&name=large)

{{< tweet user="SanDiegoZoo" id="1254719517491511296" >}}

I have started with [www.linuxfromscratch.org](https://www.linuxfromscratch.org), I am facing some challenges and problems which I am going to share with you in this blog. I will keep updating this blog as well as I move forward. 

## Before we get start

You need some basic linux command knowledge to use linuxfromscratch guide. I am going to build LFS on my local machine. If your machine has a different OS or different configuration, Then some solution will not work. 

I am going to use the same chapter title as mentioned in linuxfromscratch. I will skip those sections where I haven't faced any difficulties. 

## My Machine Configuration

```sh
OS: Ubuntu 14.04
RAM: 2GB
Disk: 500 GB
```

## Going to build

```sh
LFS version: 9.1
Published: March 1st, 2020
LFS doc:http://www.linuxfromscratch.org/lfs/downloads/9.1/LFS-BOOK-9.1-NOCHUNKS.html
```

## Challenges

## 2.2. Host System Requirements

Doc link: [http://www.linuxfromscratch.org/lfs/downloads/9.1/LFS-BOOK-9.1-NOCHUNKS.html#ch-partitioning-hostreqs](http://www.linuxfromscratch.org/lfs/downloads/9.1/LFS-BOOK-9.1-NOCHUNKS.html#ch-partitioning-hostreqs)

### Version check

```sh
cat > version-check.sh << "EOF"
#!/bin/bash
# Simple script to list version numbers of critical development tools
export LC_ALL=C
bash --version | head -n1 | cut -d" " -f2-4
MYSH=$(readlink -f /bin/sh)
echo "/bin/sh -> $MYSH"
echo $MYSH | grep -q bash || echo "ERROR: /bin/sh does not point to bash"
unset MYSH

echo -n "Binutils: "; ld --version | head -n1 | cut -d" " -f3-
bison --version | head -n1

if [ -h /usr/bin/yacc ]; then
  echo "/usr/bin/yacc -> `readlink -f /usr/bin/yacc`";
elif [ -x /usr/bin/yacc ]; then
  echo yacc is `/usr/bin/yacc --version | head -n1`
else
  echo "yacc not found" 
fi

bzip2 --version 2>&1 < /dev/null | head -n1 | cut -d" " -f1,6-
echo -n "Coreutils: "; chown --version | head -n1 | cut -d")" -f2
diff --version | head -n1
find --version | head -n1
gawk --version | head -n1

if [ -h /usr/bin/awk ]; then
  echo "/usr/bin/awk -> `readlink -f /usr/bin/awk`";
elif [ -x /usr/bin/awk ]; then
  echo awk is `/usr/bin/awk --version | head -n1`
else 
  echo "awk not found" 
fi

gcc --version | head -n1
g++ --version | head -n1
ldd --version | head -n1 | cut -d" " -f2-  # glibc version
grep --version | head -n1
gzip --version | head -n1
cat /proc/version
m4 --version | head -n1
make --version | head -n1
patch --version | head -n1
echo Perl `perl -V:version`
python3 --version
sed --version | head -n1
tar --version | head -n1
makeinfo --version | head -n1  # texinfo version
xz --version | head -n1

echo 'int main(){}' > dummy.c && g++ -o dummy dummy.c
if [ -x dummy ]
  then echo "g++ compilation OK";
  else echo "g++ compilation failed"; fi
rm -f dummy.c dummy
EOF


bash version-check.sh
```

### Error 1: 

`/bin/sh does not point to bash`

```sh
sudo ln -sf bash /bin/sh
```

### Error 2:

`version-check.sh: line 11: bison: command not found`

```sh
sudo apt-get install bison
```

### Error 3:

`version-check.sh: line 25: gawk: command not found`
```sh
sudo apt-get install gawk
```

### Error 4:
`version-check.sh: line 36: g++: command not found`

```sh
sudo apt-get install g++
```

### Error 5:
`version-check.sh: line 48: makeinfo: command not found`
```sh
sudo apt-get update -y
sudo apt-get install -y texinfo
```

### Final success output:

```sh
$ bash version-check.sh
```

```sh
bash, version 4.3.11(1)-release
/bin/sh -> /bin/bash
Binutils: (GNU Binutils for Ubuntu) 2.24
bison (GNU Bison) 3.0.2
/usr/bin/yacc -> /usr/bin/bison.yacc
bzip2,  Version 1.0.6, 6-Sept-2010.
Coreutils:  8.21
diff (GNU diffutils) 3.3
find (GNU findutils) 4.4.2
GNU Awk 4.0.1
/usr/bin/awk -> /usr/bin/gawk
gcc (Ubuntu 4.8.4-2ubuntu1~14.04.4) 4.8.4
g++ (Ubuntu 4.8.4-2ubuntu1~14.04.4) 4.8.4
(Ubuntu EGLIBC 2.19-0ubuntu6.15) 2.19
grep (GNU grep) 2.16
gzip 1.6
Linux version 4.4.0-142-generic (buildd@lcy01-amd64-006) (gcc version 4.8.4 (Ubuntu 4.8.4-2ubuntu1~14.04.4) ) #168~14.04.1-Ubuntu SMP Sat Jan 19 11:26:28 UTC 2019
m4 (GNU M4) 1.4.17
GNU Make 3.81
GNU patch 2.7.1
Perl version='5.18.2';
Python 3.4.3
sed (GNU sed) 4.2.2
tar (GNU tar) 1.27.1
makeinfo (GNU texinfo) 5.2
xz (XZ Utils) 5.1.0alpha
g++ compilation OK
```

## 2.5. Creating a File System on the Partition

Doc link: [http://www.linuxfromscratch.org/lfs/downloads/9.1/LFS-BOOK-9.1-NOCHUNKS.html#ch-partitioning-creatingfilesystem](http://www.linuxfromscratch.org/lfs/downloads/9.1/LFS-BOOK-9.1-NOCHUNKS.html#ch-partitioning-creatingfilesystem)

I had only one partition. I need to create new **physical** partition for LFS.

> It is recommended to use Physical partition only not logical.

I have reinstall my OS with new partition which is long way. You can use [GParted](https://www.howtogeek.com/114503/how-to-resize-your-ubuntu-partitions/) to make partitions. Remember you can create only 4 Physical partition. Give 1 physical partition dedicated to LFS. Below is my partition view.

```sh
$ lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 465.8G  0 disk 
|-sda1   8:1    0   476M  0 part /boot/efi
|-sda2   8:2    0   1.9G  0 part [SWAP]
|-sda3   8:3    0 190.8G  0 part /
`-sda4   8:4    0 272.7G  0 part /lfs
```

### Challenge: My Partition wipe out after restart my system

After restart I again hit the same command `lsblk`. `/lfs` partition was not there. 

In Ubuntu, You need to make entry in `/etc/fstab` in order to mount the partition on restart. 

You have to find first volumeId or UUID to make entry in `/etc/fstab`. You can use `blkid` command to get all UUID:

```sh
$ blkid

/dev/sda1: UUID="1FBD-F51A" TYPE="vfat" 
/dev/sda2: UUID="9f41c453-3b5c-4537-81b0-2fb33f19ded8" TYPE="swap" 
/dev/sda3: UUID="1a16afba-cabb-40c7-b704-1129b7004ffb" TYPE="ext4" 
/dev/sda4: UUID="b3b2eb8c-2a43-405d-97da-af8b46c743e2" TYPE="ext4" 
```

Above my Partition is `/dev/sda4` which is mounted on `/lfs`, So my UUID is `b3b2eb8c-2a43-405d-97da-af8b46c743e2`. Now I have added below line in `/etc/fstab`.


```sh
UUID=b3b2eb8c-2a43-405d-97da-af8b46c743e2 /lfs            ext4    rw,user,exec,errors=remount-ro              0       0
```

Here
* `rw`: Read, Write
* `exec`: Execute
* `errors=remount-ro`: It will mount as Read-Only, If you get any error on mounting.

> Make sure you have added `exec`, Otherwise you cannot execute any binaries from chapter [5.4. Binutils-2.34 - Pass 1](http://www.linuxfromscratch.org/lfs/downloads/9.1/LFS-BOOK-9.1-NOCHUNKS.html#ch-tools-binutils-pass1)


## 5.4. Binutils-2.34 - Pass 1

Doc link: [http://www.linuxfromscratch.org/lfs/downloads/9.1/LFS-BOOK-9.1-NOCHUNKS.html#ch-tools-binutils-pass1](http://www.linuxfromscratch.org/lfs/downloads/9.1/LFS-BOOK-9.1-NOCHUNKS.html#ch-tools-binutils-pass1)

### Error 1:

```sh
bash: ../configure: No such file or directory
```

As I mentioned on above Section, Make sure you have added `exec` in your `/etc/fstab` entry OR Make sure your partition `/lfs` has Read, Write & Execute permission. Below `/etc/fstab` entry worked for me:

```sh
UUID=b3b2eb8c-2a43-405d-97da-af8b46c743e2 /lfs            ext4    rw,user,exec,errors=remount-ro              0       0
```

This setting can be different for different OS.

### Error 2:

I had no idea where to create `build` folder.


Below steps worked for me:

```sh
$ cd $LFS/source
$ tar xvf binutils-2.34.tar.xz
$ cd binutils-2.34
$ mkdir -v build
$ cd build
$ ../configure --prefix=/tools          \
             --with-sysroot=$LFS        \
             --with-lib-path=/tools/lib \
             --target=$LFS_TGT          \
             --disable-nls              \
             --disable-werror

```

## 5.5. GCC-9.2.0 - Pass 1

```sh
cd $LFS/sources
tar -xvf gcc-9.2.0.tar.xz
cd gcc-9.2.0
```

After this you can continue with document flow.

```sh
tar -xf ../mpfr-4.0.2.tar.xz
mv -v mpfr-4.0.2 mpfr
tar -xf ../gmp-6.2.0.tar.xz
mv -v gmp-6.2.0 gmp
tar -xf ../mpc-1.1.0.tar.gz
mv -v mpc-1.1.0 mpc
```

### Error 1:

On `make` command, I got below error:

#### error: 'for' loop initial declarations are only allowed in C99 mode

```
compute_powtab.c: In function 'mpn_compute_powtab_mul':
compute_powtab.c:142:3: error: 'for' loop initial declarations are only allowed in C99 mode
   for (long pi = start_idx; pi >= 0; pi--)
   ^
compute_powtab.c:142:3: note: use option -std=c99 or -std=gnu99 to compile your code
compute_powtab.c: In function 'mpn_compute_powtab_div':
compute_powtab.c:226:3: error: 'for' loop initial declarations are only allowed in C99 mode
   for (long pi = n_pows - 1; pi >= 0; pi--)
   ^
compute_powtab.c:274:13: error: redefinition of 'pi'
   for (long pi = n_pows; pi >= 0; pi--)
             ^
compute_powtab.c:226:13: note: previous definition of 'pi' was here
   for (long pi = n_pows - 1; pi >= 0; pi--)
             ^
compute_powtab.c:274:3: error: 'for' loop initial declarations are only allowed in C99 mode
   for (long pi = n_pows; pi >= 0; pi--)
   ^
compute_powtab.c: In function 'powtab_decide':
compute_powtab.c:296:3: error: 'for' loop initial declarations are only allowed in C99 mode
   for (size_t pn = (un + 1) >> 1; pn != 1; pn = (pn + 1) >> 1)
   ^
compute_powtab.c:304:10: error: redefinition of 'pn'
   size_t pn = un - 1;
          ^
compute_powtab.c:296:15: note: previous definition of 'pn' was here
   for (size_t pn = (un + 1) >> 1; pn != 1; pn = (pn + 1) >> 1)
               ^
compute_powtab.c:308:3: error: 'for' loop initial declarations are only allowed in C99 mode
   for (long i = n_pows - 2; i >= 0; i--)

```

As per (https://stackoverflow.com/a/29338269)[https://stackoverflow.com/a/29338269],

> **This happens because declaring variables inside a for loop wasn't valid C until C99(which is the standard of C published in 1999).**

#### How I sovled this?

Open file:

```sh
vim $LFS/sources/gcc-9.2.0/build/gmp/mpn/compute_powtab.c
```

**We will solve first error:**

```sh
compute_powtab.c:226:3: error: 'for' loop initial declarations are only allowed in C99 mode
   for (long pi = n_pows - 1; pi >= 0; pi--)
```

Changes on line number 226:

```sh
  long pi;

  for (pi = n_pows - 1; pi >= 0; pi--)
    {
```

Just declare `long pi;` outside the loop. Similarly you can solve above all errors. 

**OR**

You can download my edited version of [compute_powtab.c](/download/compute_powtab.c), Place in `$LFS/sources/gcc-9.2.0/build/gmp/mpn/`. 

Hit 

```sh
make
```

Now it should works :)

## 5.7. Glibc-2.31

On below command:

```sh
../configure                             \
      --prefix=/tools                    \
      --host=$LFS_TGT                    \
      --build=$(../scripts/config.guess) \
      --enable-kernel=3.2                \

```

### Error: These critical programs are missing or too old: make

```sh
configure: error:
*** These critical programs are missing or too old: make
*** Check the INSTALL file for required versions.
```

I checked my current `make` version:

```sh
$ make --version
GNU Make 3.81
Copyright (C) 2006  Free Software Foundation, Inc.
This is free software; see the source for copying conditions.
There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.

This program built for x86_64-pc-linux-gnu
```

As per (https://stackoverflow.com/a/31915313)[https://stackoverflow.com/a/31915313]

> **Due to a long-standing unresolved Debian bug report, GNU Make remained the age-old 3.81 in Debian for a very long time, and as a consequence, in Debian-based distributions such as Ubuntu and Mint.**

So we need to upgrade our make version. Below steps worked for me.

```sh
cd /tmp
wget http://ftp.gnu.org/gnu/make/make-4.1.tar.gz
tar xvf make-4.1.tar.gz
cd make-4.1/
./configure
make
sudo make install
sudo mv /usr/local/bin/make /usr/bin/make
```

Now again hit `../configure ..` command, Now it should work :)

## 6.7. Linux-5.5.3 API Headers

### Error

```sh
(lfs chroot) root:/sources/linux-5.5.3# make mrproper

/bin/sh: sed: command not found
Makefile:653: arch//Makefile: No such file or directory
make: *** No rule to make target 'arch//Makefile'.  Stop.
```

Because of my silly mistake. I had not installed the sed package. I just reinstall the package.

## 6.12. Bzip2-1.0.8

### Error:

```sh
collect2: error: ld returned 1 exit status
make: *** [Makefile-libbz2_so:38: all] Error 1
```

Just hit `make clean`.   
Again run `make -f Makefile-libbz2_so`.

## 6.18. Binutils-2.34

```sh
$ make -k check


make[5]: *** [Makefile:2565: check-DEJAGNU] Error 1
make[5]: Leaving directory '/sources/binutils-2.34/build_6.18/ld'
make[4]: *** [Makefile:1902: check-am] Error 2
make[4]: Leaving directory '/sources/binutils-2.34/build_6.18/ld'
make[3]: *** [Makefile:1771: check-recursive] Error 1
make[3]: Leaving directory '/sources/binutils-2.34/build_6.18/ld'
make[2]: *** [Makefile:1904: check] Error 2
make[2]: Leaving directory '/sources/binutils-2.34/build_6.18/ld'
make[1]: *** [Makefile:7568: check-ld] Error 2
make[1]: Leaving directory '/sources/binutils-2.34/build_6.18'
make: *** [Makefile:2222: do-check] Error 2
```

As I research these are few common errors But as I have not found any solution So I jump on to the next step as suggested in various places.

`make tooldir=/usr install`

If you get any solution of above error, Feel free to comment below.

## 6.25. GCC-9.2.0

### Error 1:

```sh
make: *** [Makefile:2295: do-check] Error 2
```

I ignored this and move to next steps.

### Error 2: Got no output with below command:

```sh
echo 'int main(){}' > dummy.c
cc dummy.c -v -Wl,--verbose &> dummy.log
readelf -l a.out | grep ': /lib'
```

Performed below steps as mentioned on this link [https://stackoverflow.com/a/48289145](https://stackoverflow.com/a/48289145):

```sh
gcc -dumpspecs | sed -e 's@/tools@@g' > `dirname $(gcc --print-libgcc-file-name)`/specs
```

Again perform above command:

```sh
echo 'int main(){}' > dummy.c
$ cc dummy.c -v -Wl,--verbose &> dummy.log
readelf -l a.out | grep ': /lib'

     [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
```

## 6.41. Perl-5.30.1

### Error:

```sh
erlio.c:(.text+0x91b): undefined reference to `pthread_getspecific'
/usr/bin/ld: perlio.o:perlio.c:(.text+0x44e4): more undefined references to `pthread_getspecific' follow
collect2: error: ld returned 1 exit status
make: *** [makefile:364: lib/buildcustomize.pl] Error 1
```

Solution which suggested here [https://www.linuxquestions.org/questions/linux-from-scratch-13/errors-with-6-40-perl-5-28-0-a-4175642138/](https://www.linuxquestions.org/questions/linux-from-scratch-13/errors-with-6-40-perl-5-28-0-a-4175642138/) It worked for me.

it's due to not having deleted the extracted Perl-5.28.0 directory after Chapter 5. I deleted the folder. Again untar the file. 

## 8.4. Using GRUB to Set Up the Boot Process 

**NOTE**: I have not performed this section, Becuase I am going to use my host system's (Ubuntu 14.04) GRUB bootloader. Just go to another terminal of host system and hit below command.

```sh
sudo update-grub2
``` 

After this again go to old terminal(LFS) and proceed with next step (The End).


Please comment below if you facing any issue.

Thanks
