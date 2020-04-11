---
title: "Install python3.6, pip3.6, pipenv on  Ubuntu 14.04 LTS"
date: 2020-04-02T17:52:19+05:30
draft: false
type: "post"
ogtype: "article"
slug: "install-python3.6-pip3.6-pipenv-on-ubuntu14.04"
tags: ["ubuntu","ubuntu14.04","python3.6","pip3.6","python3.6 install","pipenv"]
---

## Prerequisite

__OS:__ Ubuntu 14.04 LTS  
__Processor:__ 64 Bit  
__RAM:__ 2 GB  

## 1. Install python3.6 From source

### Step 1.1: Compile

```sh
$ wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz
$ tar -xvf Python-3.6.3.tgz
$ cd Python-3.6.3
$ sudo ./configure --enable-optimizations
```

### Step 1.2: Check

```sh
$ python3.6 --version
```

## 2. Install `pip3.6`

### Step 2.1: Download pip

```sh
$ wget https://bootstrap.pypa.io/get-pip.py
```

### Step 2.2: Execute

```sh
$ sudo python3.6 get-pip.py
```

### Step 2.3: If you Got below error

#### Error  `zlib not available`

```sh
Traceback (most recent call last):
  File "get-pip.py", line 22711, in <module>
    main()
  File "get-pip.py", line 198, in main
    bootstrap(tmpdir=tmpdir)
  File "get-pip.py", line 82, in bootstrap
    from pip._internal.cli.main import main as pip_entry_point
zipimport.ZipImportError: can't decompress data; zlib not available
```

#### Step 2.3.1: Install `zlib`:

#### Install with some other dependency

```sh
$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev
```

OR

#### You can install just `zlib1g-dev`

```sh
$ sudo apt-get install zlib1g-dev
```

#### Chances to get error

```sh
$ zlib1g-dev is already the newest version.
```

#### Just upgrade it if not new

```sh
$ sudo apt-get upgrade zlib1g-dev
```

### Step 2.3.2: Now again try

```sh
$ sudo python3.6 get-pip.py
```

#### If you get same error again, Then go for below steps:

Open `Modules/Setup` from folder 'Python-3.6.3' which we extracted on Step 1.1 

```sh
$ sudo vim Modules/Setup
```

#### Uncomment the below line or jump on line no. 366
```sh
zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz
```

#### Now we need to again perform few operation from __steps 1.1__

```sh
$ sudo ./configure
$ sudo make install
```

#### Now run again & It should be work
```sh
$ sudo python3.6 get-pip.py
```

### Step 2.4: Let's Install `pipenv` for testing purpose:

```sh
$ sudo pip3.6 install pipenv
```


