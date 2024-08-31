# Setting up Load Balancing for Static Website Using Nignx

As a progression from previous projects, this project shall entail setting up Load Balancing for [www.teachteam.help](https://www.Techteam.help) using Nginx.

Load balancing is a technique commonly used for optimizing resource utilization, maximizing throughput, reducing latency, and ensuring fault-tolerant configurations.

The process is summarized below
|S/N | Project Tasks                                                                                              |
|----|------------------------------------------------------------------------------------------------------------|
| 1  |Deploy three servers                                                                                        |
| 2  |Set up static websites on two servers using Nginx.                                                          |
| 3  |Use two separate HTML files with distinct content. Deploy one file to each server's index.html location.    |
| 4  |Set up Nginx on the third server. It will act as a load balancer.                                           |
| 5  |Configure Nginx to load and balance traffic between two static websites.                                    |
| 6  |Add the Nginx Load balancer IP to the DNS A record.                                                         |
| 7  |Try accessing the website. Every time you reload the website you should see a different index.html.         |

## The Process

- Spun up 3 ubuntu servers with distinct nomenclature.
![1](/Project3/Images3/Setup3Instances.png)

- Downloaded 2 website html templates matching the websites above

### For Server 1 
- Run ```` sudo apt update && sudo apt upgrade && sudo apt install nginx````

- Then ````sudo systemctl start nginx && sudo systemctl enable nginx && sudo systemctl status nginx```` to Start, execute and certify the status of Server 1

![2](/Project3/Images3/Server1.png)

Repeat Process above for Server 2

![3](/Project3/Images3/Server2.png)



- For Server 1 on Terminal, 
run ``sudo apt install unzip``


**For the first website template**
- run ``sudo curl -o /var/www/html/2125_artxibition.zip https://www.tooplate.com/zip-templates/2125_artxibition.zip && sudo unzip -d /var/www/html/ /var/www/html/2125_artxibition.zip && sudo rm -f /var/www/html/2125_artxibition.zip``

The above command will do 3 things, download the template from the specified website, unzips and extracts the content into the var/www/html directory and deletes the zip file to clean up the directory 

Repeat same process for the second website template on the second terminal

    sudo curl -o /var/www/html/2098_health.zip https://www.tooplate.com/zip-templates/2098_health.zip && sudo unzip -d /var/www/html/ /var/www/html/2098_health.zip && sudo rm -f /var/www/html/2098_health.zip

**For Server 1**
    
- Create new file in the Nginx sites-available directory using ``sudo nano /etc/nginx/sites-available/Arts``

- edit root directory to website downloaded server 
``` server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/html/2125_artxibition;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
} 
```

- Create symbolic link for both websites using 

```sudo ln -s /etc/nginx/sites-available/Arts  /etc/nginx/sites-enabled/ ```

- then run ```sudo nginx -t``` to check syntax  of configuration file

- delete using ```
sudo rm /etc/nginx/sites-enabled/default```


- ```sudo systemctl restart nginx``` to restart the Nginx server

**For Server 2**
**For Server 1**
    
- Create new file in the Nginx sites-available directory using ``sudo nano /etc/nginx/sites-available/Healthcare``

- edit root directory to website downloaded server 
``` server {
    listen 80;
    server_name placeholder.com www.placeholder.com;

    root /var/www/html/2098_health;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
} 
```

- Create symbolic link for both websites using 

```sudo ln -s /etc/nginx/sites-available/Healthcare  /etc/nginx/sites-enabled/ ```

- then run ```sudo nginx -t``` to check syntax  of configuration file

- delete using ```
sudo rm /etc/nginx/sites-enabled/default```


- ```sudo systemctl restart nginx``` to restart the Nginx server

* Confirm both websites running 

![4](/Project3/Images3/Arts.png)


![5](/Project3/Images3/Healthcare.png)

## Load Balancer Configuration
On Load Balancing Ubuntu server;

 Install, Run and Confirm status ```` sudo apt update && sudo apt upgrade && sudo apt install nginx && sudo systemctl status nginx````

- Execute  ````sudo nano /etc/nginx/nginx.conf````

Add to the Http Block
````
    upstream techteam {
    server 1;
    server 2;
    # Add more servers as needed
}

server {
    listen 80;
    server_name <techteam.help> www.<techteam.help>;

    location / {
        proxy_pass http://techteam;
    }
}
```` 

![6](/Project3/Images3/NginxConfig.png)

Run ````sudo nginx -t```` and if successful, run the server restart using ````sudo systemctl restart nginx````

### Creation of `A Record`
Refer to [Project1](https://github.com/otuansa/FINALDEVOPS/blob/main/project1.md) 

- Point `teachteam.help` A records to the Load balancer server IP Address
and create A Records for the subdomain

- On Server 1 Terminal, Run ````sudo nano /etc/nginx/sites-available/Arts```` and edit server-name to **techhteam.help www.techteam.help**
- Run server restart ````sudo systemctl restart nginx````

- On Server 2 Terminal Run ````sudo nano /etc/nginx/sites-available/Healthcare```` and edit server-name to **techhteam.help www.techteam.help**
- Run server restart ````sudo systemctl restart nginx```

Go to browser and load website to ensure accessibility and reload to ensure load balancing traffic distribution between both servers 

![7](/Project3/Images3/LoadBalanceResult%20copy.gif)


## Install certbot and Request For an SSL/TLS Certificate

- Run **````sudo apt update && sudo apt install python3-certbot-nginx && sudo certbot --nginx````**

Authorize HTTPS activation for techteam.help and www.techteam.help

Access website to verify HTTPS succefully enabled 

![8](/Project3/Images3/LoadBalancingSecureSite.mov)



## Congratulations, you outdid yourself again, Very Demure.
# Now, Do Better!! 
