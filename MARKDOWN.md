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

Now give your User a password, make sure to enter a strong password! 
*Note: Nothing will appear no the screen when you are entering the password but it is still being recorded. 

        passwd <user-name> <user-password>


Using the command below, it will allow you to perform administrative tasks from your User account 

        usermod -aG sudo <user-name>

Now enter this command so your User has bash as login shell: 

        useradd -ms /bin/bash <user-name>
        
Now you can change from the root User to your User: 

        su -l <user-name>


---


## 2. Prevent the root user from connecting to the server via SSH

       sudo cp -r /root/.ssh /home/<user-name>


       sudo chown -R <user-name>:<user-group> /home/<user-name>/.ssh
   

Now test this out using:

        ssh -i path-to-your-key <user-name>@<ip-address>
    

---



## 3. Install nginx

        install apt nginx



---


## 4. Configure nginx serve a sample website


