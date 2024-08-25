# Setting Up Multiple Static Websites on a Single Server Using Nginx Virtual Hosts

As a continuation from the first project of Setting up a static server using Nginx [www.teachteam.help](https://www.Techteam.help)

This project succeeded in setting up 

[Fitfam](https://fitfam.techteam.help) and [Eatwell](https://eatwell.techteam.help) websites on a single server  with the following processes:



## Installation of  Nginx and Website Setup 

- Execute the following commands.

    ```
    sudo apt update && sudo apt upgrade && sudo apt install nginx
    ````

    This runs the Update, upgrade and Nginx installation concurrently upon successful completion of each task.


- Start the Nginx server using ``sudo systemctl start nginx ``; enable start boot with ``sudo systemctl enable nginx``and confirm its running status with ``sudo systemctl status nginx```

    Open the IP address on a web browser to view the default startup page
![nginx](/Project2/Image/Nginx.png)

- run ``sudo apt install unzip``


**For the first website template**
- run ``sudo curl -o /var/www/html/2119_gymso_fitness.zip https://www.tooplate.com/zip-templates/2119_gymso_fitness.zip && sudo unzip -d /var/www/html/ /var/www/html/2119_gymso_fitness.zip && sudo rm -f /var/www/html/2119_gymso_fitness.zip``

The above command will do 3 things, download the template from the specified website, unzips and extracts the content into the var/www/html directory and deletes the zip file to clean up the directory 

Repeat same process for the second website template 

``sudo curl -o /var/www/html/2129_krispy_kitchen.zip https://www.tooplate.com/zip-templates/2129_krispy_kitchen.zip && sudo unzip -d /var/www/html/ /var/www/html/2129_krispy_kitchen.zip && sudo rm -f /var/www/html/2129_krispy_kitchen.zip``



**Setup website configuration**

- create new file in the Nginx sites-available directory using ``sudo nano /etc/nginx/sites-available/fitfam``

- edit root directory to website downloaded server 
``` server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html/2119_gymso_fitness;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
} 
```

- Repeat same for the second website  ``sudo nano /etc/nginx/sites-available/eatwell``

``` server {
    listen 80;
    server_name placeholder.com www.placeholder.com;

    root /var/www/html/2129_krispy_kitchen;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

````

- Create symbolic link for both websites using 

```sudo ln -s /etc/nginx/sites-available/fitfam /etc/nginx/sites-enabled/ && sudo ln -s /etc/nginx/sites-available/eatwell /etc/nginx/sites-enabled/ ```

- then run ```sudo nginx -t``` to check syntax  of configuration file

- delete ```sudo rm /etc/nginx/sites-available/default &&
sudo rm /etc/nginx/sites-enabled/default```


- ```sudo systemctl restart nginx``` to restart the Nginx server


## Create An "A Record"
Creating a Host Record in [Project1](https://github.com/otuansa/FINALDEVOPS/blob/main/project1.md) refers 
Created an A record by selecting the Domain name, attaching the IP address and clicking **create records** 
- Create sub domain records by inputing **fitfam** under Records name and the same IP Address as above and click **create records**
- Repeat same process for **eatwell** subdomain 
Confirm the both records created


    - Run ```` sudo nano /etc/nginx/sites-available/fitfam````

then edit server_name to domain name

``` server {
    listen 80;
    server_name fitfam.techteam.help www.fitfam.techteam.help;

    root /var/www/html/2119_gymso_fitness;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
} 
```
ctrl X and save

Repeat same for second website using ```sudo nano  /etc/nginx/sites-available/eatwell``` 
Edit server name to domain name 


``` server {
    listen 80;
    server_name eatwell.techteam.help www.eatwell.techteam.help;

    root /var/www/html/2129_krispy_kitchen;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

````


Restart nginx server ```sudo systemctl restart nginx```

Go to domain name on web browser to confirm site accessibility

### Certbot installation 
- Install certbot with the command ````sudo apt update && sudo apt install python3-certbot-nginx && sudo certbot --nginx```` 

- Run ````sudo certbot --nginx```` to request certificate and follow instructions to activate HTTPS for ````fitfam.techteam.help```` and ````eatwell.techteam.help````

- Verify the website's SSL using the OpenSSL utility with the command: ````openssl s_client -connect fitfam.tedchteam.help:443```` 

and

    openssl s_client  -connect eatwell.techteam.help:443

Open the domain names on a web browser!

![1](/Project2/Image/SecureFitfam.png)
![1](/Project2/Image/secure%20eatwell.png)