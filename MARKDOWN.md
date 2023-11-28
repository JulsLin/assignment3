TUTORIAL

Starting from a Fresh Debian 12 server on DigitalOcean

1. Create a new regular user:
    - User can perform administrative tasks
    - User has bash as login shell
    - User can access the server via SSH
2. Prevent the root user from connecting to the server via SSH
3. Install nginx
4. Configure nginx to serve a sample website


*********************************************************************
1. Create a regular user

General syntax: 

        useradd [options] <user-name>

Give the user a password:

        passwd <user-name> <user-password>

Now you can change from root to user: 

        su -l <user-name>

Allow user to perform administrative tasks: 

        usermod -aG sudo <user-name>

FROM ROOT USER - User has bash as login shell: 

        useradd -ms /bin/bash <user-name>

*********************************************************************


2. Prevent the root user from connecting to the server via SSH

        sudo cp -r /root/.ssh /home/<user-name>

        sudo chown -R <user-name>:<user-group> /home/<user-name>/.ssh

Now test this out using:

        ssh -i path-to-your-key <user-name>@<ip-address>

*********************************************************************


3. Install nginx <br>

        install apt nginx

********************************************************************




