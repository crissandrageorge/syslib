# Install WordPress
> WordPress is an open-source content management system that is known for being a general-purpose CMS and website builder. Two sites exist, WordPress.com and WordPress.org, to make WordPress accessible. WordPress.org is maintained by the WordPress Foundation, which oversees the development of and provides access to the WordPress software. When we download the WordPress software, we download it from WordPress.org. Unlike the hosted solution, when we install and set up WordPress on our own servers, we become responsible for administrating its installation and for keeping the software updated.
> Since it is widely used, it is a focus of attack, so reading the security and installation information. 
# Usage of WordPress within libraries
### Example Website
> Example is the Reading Public Library (RPL) in Massachusetts. These library websites coordinate with additional solutions that provide integrated library systems and other electronic resource services. RPL, for instance, connects their WordPress installation, which serves as their main website page, with the open-source Evergreen ILS, which serves their OPAC. Check this by clicking on RPL's Library Catalog link, and you will see that it takes you to a different URL.
> RPL's WordPress launch at: Reading Public Library Launches New WordPress Site. The above announcement page also describes how various plugins were used to offer patrons additional functionality. These include plugins to display business hours and to manage events and event attendees.
### Bringing it all together with WordPress in Libraries
> Coordinating these many services across all these websites drives the need to develop standards for data exchange and workflow processes.
> Many library websites are partitioned like this, having a main page such as libraries.uky.edu that is connected to other sites like the OPAC, Discovery System, and more. 
> When installing WordPress, it will be as if we are only installing the front entrance to the library.
> Libraries are generally like this, which is why there is confusion around how libraries provide electronic resources. There are efforts to make all these components connect more seamlessly (e.g., through discovery systems), but if we were to model this to the walking around world, it would be like having a library that has multiple buildings, where each building provides one thing: one building for books, one building for journals, another building for other journals, another building for another set of journals, another building for looking up where to find journals, another building for special collections, and so on.

# Plugins
-	Plugins are often used with WordPress sites to offer all sorts of additional capabilities. 
-	Currently, there are over 60 thousand plugins available for WordPress, but some are of higher quality and utility than others. In addition to the thousands of available plugins, there are over 10 thousand free themes for WordPress sites. 
-	Plus, many businesses offer paid themes or can offer customized themes based on customer needs. 
-	These themes can drastically alter the appearance and usability of a WordPress site.
# Installation
> In this lesson, we are going to install WordPress by downloading the most recent version from WordPress.org and installing it manually. The WordPress application is available via the apt command, but the apt process makes it a bit more confusing than it should be, oddly.
-	We are going to kind of follow the documentation provided by WordPress.org. You should read through the documentation before following my instructions, but then follow the process I outline here instead because the documentation uses some different tools than we'll use.
> Another reason we do this manually is because it builds on what we have learned by building our bare bones ILS. That is, the two processes are similar. In both cases, we create a specific database for our platform, we create a specific user for that database, and we provide login credentials in a specific file.
> Do not follow, but read these instructions <a href = “ https://wordpress.org/documentation/article/how-to-install-wordpress/” > Instructions from WordPress</a>

## Step One
1.	Requirements
> * All major software has dependencies and the more complicated a software is, the stricter the dependencies are. *
- Check that our system has all of the requirements. 
```
php --version
mysql --version
```
> The output from php --version shows that our systems have PHP 7.4.3, which is greater than PHP 7.4. 
> The output from mysql --version show that our systems have MySQL 8.0, which is greater than MySQL 5.7. This means we can proceed.
1.	Next, we need to add some additional PHP modules to our system to let WordPress operate at full functionality.
```
sudo apt install php-curl php-xml php-imagick php-mbstring php-zip php-intl
```
1.	Now, we need to restart Apache and MySQL
```
sudo systemctl restart apache2
sudo systemctl restart mysql
```
## Step Two
1.	The next step is to download and extract the WordPress software, which is downloaded as a tar.gz file.
> This is very much like a compressed zip file. Although we only download one file, when we extract it with the tar command, the extraction will result in a new directory that contains multiple files and subdirectories. 
-	The general instructions include:
1. Change to the /var/www/html directory.
1.	Download the latest version of WordPress using the wget program.
1.	Extract the package using the tar program.
```
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzvf latest.tar.gz
```
> As noted in the WordPress documentation, this will create a directory called wordpress in the same directory. Therefore, the full path of your installation will be located at ** /var/www/html/wordpress **
## Step Three
1.	Create a Database and a User
-	The WordPress documentation describes how to use phpMyAdmin to create the database and a user for WordPress. 
-	phpMyAdmin is a graphical front end to the MySQL relational database that you would access through the browser. 
-	But you can minimize software to reduce the server's security exposure. Therefore, we are going to create the WordPress database and a database user using the same process we used to create a database and user for our bare bones OPAC.  
*Therefore, we need to: *
1. Switch to the root Linux user
1.	Login as the MySQL root user
```
sudo su
mysql -u root
```
1.	The mysql -u root command places us in the MySQL command prompt. The next general instructions are to:
1.	Create a new user for the WordPress database
1.	Be sure to replace the Xs with a strong password
1.	Create a new database for WordPress
1.	Grant all privileges to the new user for the new database
1.	Examine the output
1.	Exit the MySQL prompt
> Replaces Xs with a unique and strong password of your own
```
create user 'wordpress'@'localhost' identified by 'XXXXXXXXX';
create database wordpress;
grant all privileges on wordpress.* to 'wordpress'@'localhost';
show databases;
\q
```
## Step Four
 Now we need to Set up wp-config.php
> When we created our bare bones OPAC, we created a file called login.php that contained the name of the database (e.g., opacdb), the name of the database user (e.g., opacuser), and the user's password. WordPress follows a similar process, but instead of login.php, it uses a file called wp-config.php.
Follow these general steps:
1.	Change to the wordpress directory, if you haven't already.
1.	Copy and rename the wp-config-sample.php file to wp-config.php.
1.	Edit the file and add your WordPress database name, user name, and password in the fields for DB_NAME, DB_USER, and DB_PASSWORD.
```
cd /var/www/html/wordpress
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```
-	In nano, add your database name, user, and password in the appropriate fields, just like we did with our login.php file for our bare bones OPAC.
-	Additionally, we want to disable FTP uploads to the site. To do that, navigate to the end of the file and add the following line:
```
define('FS_METHOD','direct');
```
## Step Five (optional)
-	The WordPress files were installed at /var/www/html/wordpress. This means that your site would be located at:
```
http://11.111.111.11/wordpress
```
-	If you want to, you can rename your wordpress directory to something else. 
-	The WordPress documentation uses blog as an example. But it could be something else, like the name of a fictional library that you might be using WordPress to build a site.
-	If you decide to change it, be sure to keep the name lowercase and one word (no spaces and only alphabetic characters). For example, if I want to change mine to blog, then:
```
cd /var/www/html
sudo mv wordpress blog
```

## Step Six
Change File Ownership
-	WordPress will need to write to files in the base directory. Assuming your still in your base directory, whether that is /var/www/html/wordpress or /var/www/html/blog or like, run the following command:
```
sudo chown -R www-data:www-data *
```
## Step Seven
Run and Install the Script
-	The next part of the process takes place in the browser. The location (URL) that you visit in the browser depends on your specific IP address and also includes the directory in /var/www/html that we extracted WordPress to or that you renamed if you followed Step 5. 
-	Thus, if my IP address is 11.111.111.11 and I renamed by directory to blog, then I need to visit the following URL:
```
http://11.111.111.11/blog/wp-admin/install.php
```
-	If I kept the directory named wordpress, then this is the URL that I use:
```
http://11.111.111.11/wordpress/wp-admin/install.php
```

## Rest of the Steps
-	From this point forward, the steps to complete the installation are exactly the steps you follow using WordPress's documentation.
-	Most importantly, you should see a Welcome screen where you enter your site's information. The site Username and Password should not be the same as the username and password you used to create your WordPress database in MySQL. Rather, the username and password you enter here are for WordPress users; i.e., those who will add content and manage the website.
-	Two things to note:
o	We have not setup Email on our servers. It's actually quite complicated to setup an email server correctly and securely, but it wouldn't work well without having a domain name setup anyway. So know that you probably should enter an email when setting up the user account, but it won't work.
o	Second, when visiting your site, your browser may throw an error. Make sure that the URL is set to http and that it's not trying to access https. Setting up an https site also generally requires a domain name, but we are not doing that here. So if there are any problems accessing your site in the browser, be sure to check that the URL starts off with http.
# Congrats on setting up your WordPress library site! 
-	It's now time to explore and build a website. Use free themes and free plugins to alter the look of the site, its usability, and its functionality. 
-	Try to create a nice-looking website. 
> Generally, your goal is to create an attractive, yet fictional, front entrance for a library website. It's also a break from the command line!

# Reflection
1.	As software is more complicated, it will have stricter dependencies. During the cataloging module, we had to have PHP and MySQL between the database and the HTML. For WordPress, a much more complicated software, we need to make sure that our system has the requirements needed for installation. This is a good tip to always ensure efficiency and not have to back track, especially when downloading software outside of the –apt ecosystem.
2.	Here, we begin to see greater connections across software and systems. Additionally, we can see the combination of using more complicated software and how it interacts with the other software that we have already begun using. 
3.	Security issues came up within this lecture, which further affirms the importance of keeping these things in mind throughout the planning, management, and curation process. 
4.	As we continue to make this website, aspects of internet technology, systems, information architecture, and software will continue to arise, which will challenge us to continue discussing and visualizing these great connections that occur to have a functional library website. 
5.	Since I am also in LIS 668 and LIS 638 right now, it is making inquire on how I can connect the material within all these classes to have good stylistic and aesthetic approaches along with organized information within the database that is easier retrievable in addition to all of the other working needs for a repository. 
6.	I am eager to see how this can interact with an ILS and how this will all come together into one connected network and system. Additionally, if a library changes their ILS and wants to have their digitized archival materials compatible with the ILS (due to past bad experiences with another system), how will this process look? Is it like the chicken and the egg question? Does one need to come first or is there a way to ensure interoperability and compatibility across these technologies? 
