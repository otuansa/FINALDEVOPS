# Setup a Static Website Using Nginx

I am Otu Ansa, A DevOps Engineering and Cloud Computing enthusiast and this is my first project!!.

### **`Warning`** This had a lot of drawbacks but most importantly, lots of lessons



#### The Project was broken into the tasks below:

 - [x] Task 1: Buy a domain name from a domain Registrar.
- [x] Task 2: Spin up a Ubuntu server & assign an elastic IP to it.
- [x] Task 3: SSH into the server and install Nginx.
- [x] Task 4: Find freely available HTML website files.
- [x] Task 5: Download and unzip the website files to the Nginx website directory.
- [x] Task 6: Validate the website using the server IP address.
- [x] Task 7: In Route53, create an A record and add the Elastic IP.
- [x] Task 8: Using DNS verify the website setup.
- [x] Task 9: Install certbot and Request For an SSL/TLS Certificate.
- [x] Task 10: Validate the website SSL using the OpenSSL utility.

## Domain Acquisition
I got a domain name from Namecheap.com and called it !![Techteam.help]

## Creating Ubuntu Server
### EC2 Launch
On my AWS Management Console I created an Instance using Ubuntu AMI; Created and saved a New key pair `cube.pem`  and the Instance was named *'Project1'*
![1](/image/img1.png)


### Creation and Assigning of Elastic IP

Backt to the AWS Console on the Network and Security menu, I selected Elastic IP, Allocate Elastic IP, leaving settings unchanged, I assigned the generated elastic IP to my running instance
![2](/image/ElasticIP.png)

 

Given the Instance IP has been updated, SSH into the Instance on Terminal again to execute.

## NGINX Installation and Website Setup

On executing the following commands
`sudo apt update` ; `sudo apt upgrade` and `sudo apt install nginx`
Nginx was installed and started using `sudo systemctl start nginx`

On the EC2 dashboard, the **Public IPv4 address** was copied and pasted in the web browser to confirm 
![3](/image/Nginx.png)


## Obtain Website Template for URL

This was got from www.tooplate.com where the zip file of the desired template was obtained using Inspect protocol and Network tab and saved in downloads folder while also copying the URL of the template to be `curl` into the machine. 

This was done with `sudo curl -o /var/www/html/2126_antique_cafe.zip https://www.tooplate.com/zip-templates/2126_antique_cafe.zip`** to download the websites file to the html directory.

![4](/image/antique%20cafe.png)


Next I installed Unzip Tool using **`sudo apt install unzip`**
- Unzipped the website template `sudo unzip 2126_antique_cafe.zip`
- then Update my nginx configuration using **`sudo nano /etc/nginx/sites-available/default`** and edited the **`root`** directive  to where the website content is located 

- I restarted my Nginx with **`sudo systemctl restart nginx`**.
- I opened my web browser to **Public IPv4 address/Elastic IP address**

![5](/image/Techteam.png)



 ## Create An A Record

 This was done to make the website accessible through domain name as well as IP Address by moving hosting to AWS Route 53 
- I created Hosted Zone on ROute 53 and copied the generated values 

end