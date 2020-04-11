---
title: "Challenges in linuxfromscratch"
date: 2020-04-11T13:55:52+05:30
draft: true
---

I have started with [www.linuxfromscratch.org](www.linuxfromscratch.org), I am facing some challenges and problems which I am going to share with you in this blog. I will keep updating this blog as well as I move forward. 

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

### 2.2. Host System Requirements

#### Version check

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

**Error 1**: 

`/bin/sh does not point to bash`

```sh
sudo ln -sf bash /bin/sh
```

**Error 2**:

`version-check.sh: line 11: bison: command not found`

```sh
sudo apt-get install bison
```

**Error 3**:

`version-check.sh: line 25: gawk: command not found`
```sh
sudo apt-get install gawk
```

**Error 4**:
`version-check.sh: line 36: g++: command not found`

```sh
sudo apt-get install g++
```

**Error 5**:
`version-check.sh: line 48: makeinfo: command not found`
```sh
sudo apt-get update -y
sudo apt-get install -y texinfo
```

Final success output:


```sh
$ bash version-check.sh

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
```
