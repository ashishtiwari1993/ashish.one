---
title: "[Part 1] Setup LEMP environment with Docker - Setup Nginx and PHP"
date: 2020-05-16T17:01:47+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["LEMP","PHP","MySQL","Nginx","Linux","docker-compose","docker"]
slug: "part-1-setup-lemp-environment-with-docker-setup-nginx-and-php"
topics: "Misc"
---

Hi guys, In this series, we are going to setup LEMP Stack (Linux, Nginx, MySQL, PHP). Mainly it is used by web developers. I am assuming you have a basic idea about Docker & How it works. 

In this blog, We are going to setup PHP and Nginx.

### Why Docker?

I will not go too much deep, You can find more resources over the internet about the docker. 

Docker makes the installation process very smooth and it gives your isolated environment as the container. 

With the docker, There will be no more excuses like:

> **”It's working on my machine.”  :D**

Because your docker environment remains the same across the platforms, So above excuses will going to turn with:

> **”It's working on every machine” ;)**

In Short, Docker makes tech person's life easy like the developer, tester, etc.

### My Machine configuration:

```sh
Docker Version: 18.09.7
Docker Compose Version: 1.24.1
RAM: 8GB
OS: Ubuntu 18.04
```

Make sure docker and docker-compose is installed on your machine. 

### Step 1: Create folders:

```sh
mkdir LEMP
cd LEMP
mkdir {public_html,nginx_conf}
```

`public_html`: It will contains your PHP code.  
`nginx_conf`: It will contains your Nginx configuration file. 


### Step 2: Create Nginx conf file

Go to `nginx_conf`:

```sh
cd nginx_conf
```

Create file `lemp-docker.conf` (You can give any name according to your requirement) & Paste below configuration block.

```sh
server {
		listen 80;
		server_name _;
		root /public_html;

		location / {
				index index.php index.html;
		}

		location ~* \.php$ {
				fastcgi_pass    php:9000;
				fastcgi_index   index.php;
				include         fastcgi_params;
				fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
				fastcgi_param PATH_INFO $fastcgi_path_info;
		}
}
```

Save and Exit from folder.

### Step 3: Create file `docker-compose.yml`

Create file `docker-compose.yml` in `LEMP` folder & paste the below lines: 

```sh
version: '3'

services:
 web:
  image: nginx
  ports:
   - "80:80" 
  volumes:
   - ./public_html:/public_html
   - ./nginx_conf:/etc/nginx/conf.d
   - /tmp/nginx_logs:/var/log/nginx 
  networks:
   - nginx-php

 php:
  image: php:7.2-fpm
  volumes:
  - ./public_html:/public_html
  networks:
  - nginx-php

networks:
 nginx-php:
```

Here we have created the network with the name `nginx-php`. You can check more about the networks [here](https://docs.docker.com/compose/networking/#specify-custom-networks).

You can set `volumes` path according to your requirement. 

### Step 4: Let's Up the containers

Create container:

```sh
cd LEMP/
docker-compose up
```

**Note:** Make sure no other service is running on port 80.

Output:

![docker compose up](/img/lemp-docker/docker-compose-up.png)


You can hit the `localhost` on your browser.

Once you hit the `localhost` check nginx logs:

```sh
cat /tmp/nginx_logs/access.log 

172.25.0.1 - - [16/May/2020:12:51:04 +0000] "GET / HTTP/1.1" 404 556 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.163 Safari/537.36" "-"
```

Now stop the containers by simply hitting `Ctrl + c`. Now run docker-compose in detached mode. It will run in the background.

```sh
docker-compose up -d
```

Lists containers

```sh
docker-compose ps
```

### Step 5: Let's Create `test.php`

Go to `LEMP/public_html`

```sh
cd LEMP/public_html
```

Create file `test.php` & paste below code:

```php
<?php

echo phpinfo();
```

### Step 6: Run `test.php`

Visit [http://localhost/test.php](http://localhost/test.php).

Output:

![phpinfo](/img/lemp-docker/phpinfo.png)

### At the End

We have successfully created Nginx and PHP Environment. In Part 2, We will check, How we can add MySQL to this environment.  
