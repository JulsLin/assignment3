# ﻿TUTORIAL

Starting from a Fresh Debian 12 server on DigitalOcean

1. Create a new regular user:
    - User can perform administrative tasks
    - User has bash as login shell
    - User can access the server via SSH
2. Prevent the root user from connecting to the server via SSH
3. Install nginx
4. Configure nginx to serve a sample website
    
   
---
    
    
## 1. How to create a regular user

To create a regular using here is the general syntax:

        useradd [options] <user-name>

Please refer to `man useradd` to get more information

Now give your User a password, make sure to enter a strong password! 

*Note: Nothing will appear on the screen when you are entering the password but it is still being recorded. 

        passwd <user-name> <user-password>


Using the command below, it will allow you to perform administrative tasks from your User account. Meaning your User will be added to the sudo gorup to allow you to use sudo in Debian.

        usermod -aG sudo <user-name>

Now enter this command so your User has bash as login shell: 

        useradd -ms /bin/bash <user-name>
        
Now you can change from the root User to your User with this command: 

        su -l <user-name>


---


## 2. Prevent the root user from connecting to the server via SSH

       sudo cp -r /root/.ssh /home/<user-name>

This command below changes the permissions from root to your User and User group:

       sudo chown -R <user-name>:<user-group> /home/<user-name>/.ssh

Now edit the SSH configuration so that the root user can no longer connect to the server via SSH

First `cd` into the folder holding the sshd_config file: `cd /etc/ssh`

Then enter this command:

        sudo vim sshd_config

The vim file will open and find the line `#PermitRootLogin yes`.

To edit the file, press `i` and get rid of the `#` and change yes to no.

To save, press `esc` and `:wq` to save and quit vim.

Now it is time to restart SSH with this command:

        sudo systemctl restart ssh.service
   

Now test this out using:

        ssh -i path-to-your-key <user-name>@<ip-address>

Example: `ssh -i path-to-your-key charles@165.232.131.22`
    

---



## 3. Install nginx

Make sure everything is updated before installing nginx

        sudo apt update
        sudo apt install nginx

To make sure the service is running, type in:

        systemctl status nginx

You should see something like this:

```
Output
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-04-20 16:08:19 UTC; 3 days ago
     Docs: man:nginx(8)
 Main PID: 2369 (nginx)
    Tasks: 2 (limit: 1153)
   Memory: 3.5M
   CGroup: /system.slice/nginx.service
           ├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─2380 nginx: worker process
```

If the service is disabled, enable it with `systemctl`

        sudo systemctl enable --now nginx

>Now you are ready to move on to the next step!


---


## 4. Configure nginx to serve a sample website

The nginx configuration files can be found in `/etc/nginx`. Today you will be using `/etc/nginx/sites-available` and `/etc/nginx/sites-enabled`. The configuration files for our servers are in these two directories.

There are different directories used to hold the documents served by nginx, the default location on the Debian server we are using is `/var/www`.

### Step one:

Create a new `my-site` directory in `/var/www`. 

Then create an `index.html` document that hold your code in the `my-site` directory.

Here is an example of index.html:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>Hello, World</h1>
</body>
</html>
```

>Remember to change permissions using chmod to edit the files in vim

### Step two:

Creating your server block.

Create a new file in `/etc/nginx/sites-available` and past this nginx config into the file:

```
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	
	root /var/www/index.html;
	
	index index.html index.htm index.nginx-debian.html;
	
	server_name _;
	
	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}
```

Now create a symbolic link to your new config file in `/etc/nginx/sites-enabled`

Now run these commands below:


        sudo ln -s /etc/nginx/sites-available/my-site.conf /etc/nginx/sites-enabled

>The `ln` command line utility is used for creating links between files. By default, the `ln` command creates hard links. To create a symbolic link, use the `-s` option.
>You can also run `sudo ln -s /etc/nginx/sites-available/my-site.conf` if you are currently in the `/etc/nginx/sites-enabled` directory.

Now to test your nginx configurations:

        sudo nginx -t

Restart the service:

        systemctl restart nginx

If no errors appear after restarting the nginx service, you can try the curl command. This will display your sample HTML instead of the original debian nginx page.

        curl <ip-address>

>Now you are all done! Congratulations for completing this tutorial!





