# Install Omeka

> Omeka is an open-source web publishing platform to share digital collections and create media rich exhibits online. 
> Unlike LIS 602, the goal is not to understand knowledge organization, but rather learn to administer this platform

## Goal
> Just as we have have created our bare bones OPAC/ILS and we have downloaded, installed, and configured WordPress on the server
> We will do a similar process with Omeka.

### Prerequistes
- Need to install ImageMagick: a suite of utilities to work with photo files
```
sudo apt install imagemagick
```

- Need to enable APache mod_rewrite : an apache module to write URLS to create appropriate URLS for Items
```
sudo systemctl restart apache2
```
- Then, restart Apache after enabling the rewrite
```
sudo systemcttl restart apache2
```

## General Instructions
1. create a new user and a new database in MySQL for Omeka (do not reuse WordPress database, user, or credentials
1. Use wget from your server to download Omeka Classic as a zip file and extract it in:
```
/var/html 
```
- https://github.com/omeka/Omeka/releases/download/v3.1/omeka-3.1.zip
- unzip it with the *unzip* command, which you will have to install with the apt command.
- the extracted directory will be named omeka-3.1. You might want to rename it simply omeka.

1. In the extracted directory, find the db.ini file and add your database credentials, and replace the values containing XXX
> This is the same as the login.php file
1. Use the *chown* command on the files directory in the Omeka directory
1. Restart APache2 and MySQL
1. Go to your IP Address and complete the setup via web form

## Helpful Links

- The user manual below is helpful, but it does not provide explicit instructions.

- Be sure to download Omeka Classic and not Omeka S.

> Omeka: https://omeka.org/
> Omeka Classic: https://omeka.org/classic/
> Omeka Classic User Manual: https://omeka.org/classic/docs/

# Reflection
1. Definitely being alone on this was scary, but I did see myself solving the issues well and even when asking questions I did have a feeling about what was wrong
1. no FTP for this and i can only assume I did this because i was trying so hard to follow the instructions from WordPress instead of testing this out on my own, which was frustrating, but it taught me a lot
1. A lot of these general instructions are consistent across, which really made me understand systems better.
1. There is a lot of consistency across all systems with specifics that change, which is why it is so important to learn the foundational processes and steps. 



# Code used within this setup

```
sudo apt install imagemagick
sudo a2enmod rewrite
sudo systemctl restart apache2
cd /var/www/html
sudo wget 
https://github.com/omeka/Omeka/releases/download/v3.1/omeka-3.1.zip
sudo apt install unzip
sudo unzip omeka-3.1.zip
sudo mv omeka-3.1 omeka
sudo su
mysql -u root
create user 'omeka'@'localhost' identified by 'crissandrageorge';
create database omeka;
grant all privileges on omeka.* to 'wordpress'@'localhost';
show databases;
\q

sudo nano db.ini
sudo chown -R www-data:www-data *
sudo systemctl restart apache2
sudo systemctl restart mysql
```
put in IP address/omeka and go through install process on the web interface
