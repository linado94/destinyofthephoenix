Phase 1

Following the tutorial to set up AWS EC2 severs on Amazon.
After setup aws ec2 and direct your domian to the sever, we need to connect to our ubuntu.
Using putty if you have Windows, using ssh command if you have Mac.
-If using Windows

#Download putty

Run putty key generate, load private key which saved from aws ec2, save it as new private ppk.

Run putty, on the login panel, copy the aws ec2 yourservername(ex: ec2-12-223-230-31.us-east-2.compute.amazonaws.com) as Host Name, and click SSH then click Auth, browse the saved ppk, click open.
If ask user name to login, type ubuntu,
and type sudo -i will become root user
Now download nginx by following the tutorial:
sudo wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
cd /etc/apt
sudo nano sources.list

(hold down arrow until to the end of file).

At the bottom of that file, append it with:

#
deb http://nginx.org/packages/ubuntu xenial nginx
deb-src http://nginx.org/packages/ubuntu xenial nginx
#

Click ctrl+X to exit, type yes to confirm save the file.

sudo apt-get update
sudo apt-get install nginx

Then start NGINX:
sudo service nginx start
Typing your ip or domain on your browser to check if it will show up nginx logo.

Next, the github need to push to the ubuntu.

install git on ubuntu by typing the code

sudo apt update
sudo apt install git
git clone https: Your Github Address Name .git
test if can pull the github from the server.

Phase 2

now we need to encrypt our domain in order to get https and A+ on the test of security.

Go to the website https://certbot.eff.org/ to encrypt domain, domain will become https.
After encrypt domain you will get the path of your certification pem, and private key pem, record the path.
Type command on ubuntu to edit the default configur file:
sudo nano /etc/nginx/conf.d/default.conf
at the beggin of file, add domain name after the server_name like below
server_name YOURDOMAIN www.YOURDOMAIN
Check server 443 if certifcation.pem, privatekey.pem and options-ssl-nginx added by Certbot

if not, add few lines from tutroals like: ssl_certificate /YOU CERT PATH/fullchain.pem;

...

And check server 80 if add return 301

if not, need to add few lines from tutroals like:

return 301 https://$server_name$request_uri;

...

Notes: In the tutorial this line:

return 301 https://$server_name$request_uri;

Need to type correctlly:

Is return 301 https://$server_name$request_uri;

And also:

location / {

Is location / {

Has empty space between “/”

After edit the default config file, type:

sudo nginx -s reload

Now test your domain on https://www.ssllabs.com

If get A+, done.
