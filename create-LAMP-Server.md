# Create a LAMP Server
## Install Apache2
```
sudo apt install apache2
systemctl status apache2


## Install w3m

```
sudo apt install w3m
w3m 127.0.0.1

## Move Index.html File
```
nano index.html
sudo mv index.html index.html.original
sudo nano index.html
ls 
sudo nano index.html
### *Crucial to Update your System before all of this!*

Apache is a Web Server and w3m is used to create a web page
Note that most browsers nowadays may try to force HTTPS mode, and they also often hide the protocal from the URL.
If your web page is not loading, make sure your URL is http://IP-ADDRESS and not https://IP-ADDRESS.
#### If Apache is installed correctly, then you will see "Apache2 Ubuntu Deafult Page. It Works!"

## Create a Web Page

```
cd /var/www/html/
sudo mv index.html index.html.original
sudo nano index.html


*Use sudo to work on files and directories outside of our home directories*

## Basic HTML Example 1

```
<html>
<head>
<title>My first web page using Apache2</title>
</head>
<body>

<h1>Welcome</h1>

<p>Welcome to my web site.
I created this site using the Apache2 HTTP server.</p>

</body>
</html>



## Basic HTML Example 2 (From LIS 638 Class)

```
<html>
	<head>
		<title> Organization Name</title>
	</head>
	<body>
		<h1>About Us:</h1>
		
		<p>We are an organization. We do organization stuff. </p>
		<ul>
			<li> Organizations are great. </li>
			<li> Organizations are organized. </li>
		</ul>
		<p>We do this, and we do that.</p>
			<ol>
				<li> This is the first thing we do. </li>
				<li> This is the second thing we do. </li>
			</ol>
			<h2> Mission, Vision, and Objectives </h2>
				<ul>
					<li> This is our mission. </li>
					<li> This is our vision. </li>
					<li> These are our objectives and goals. </li>
						<ol> 
							<li> First Goal </li>
							<li> Second Goal </li>
							<li> Third Goal </li>
						</ol>
				</ul>
			<h1>Resume</h1>
			<p><a href="Resume_example.docx"> Click here to download my resume. </a></p>
			<br/>
			<hr/>
		<p> Copyright 2022 Crissandra George. Last updated February 14th, 2023 </p>
	<body bgcolor=#04B45F>
	<a href="malito:name@uky.edu"> Click here to email me </a> </p>
	<p><a href="http://www.uky.edu"> University of Kentucky</a></p>
	</body>
</html>



# Installing and Configuring PHP
## PHP is a server-side programming language (must be installed on the server to be used and has to work with the HTTP server, Apache2)

#Steps
1. Install php and relevant Apache2 Modules
1. Configure PHP and the modules
1. Configure PHP and modules with MYSQL

## Install

```
sudo apt install php libapache2-mod-php
sudo systemctl restart apache2
systemctl status apache2


## Check Install (always important!)

```
cd /var/www/html/
sudo nano info.php

### Add:
```
<?php
phpinfo();
?>


*Now visit the external IP to check all of this (IP/info.php)*

## Configurations

```
cd /etc/apache2/mods-enabled/
sudo cp dir.conf dir.conf.bak
sudo nano dir.conf


```
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm


```
apachectl configtest


## If Sytax okay, then:
```
sudo systemctl reload apache2
sudo systemctl restart apache2

## Create PHP File

```
cd /var/www/html/
sudo nano index.php

## Copy:
```
<html>
<head>
<title>Broswer Detector</title>
</head>
<body>
<p>You are using the following browser to view this site:</p>

<?php
echo $_SERVER['HTTP_USER_AGENT'] . "\n\n";

$browser = get_browser(null, true);
print_r($browser);
?>
</body>
</html>


# Installing and Configuring MY SQL

## remember the basics of relational databases
LAMP starts with Apache2 and the added functionality of PHP, but to complete we need MySQL.

## Install
```
sudo apt install mysql-server
systemctl status mysql 
sudo su


**need to be careful when working with the root directory!**

### Command to exit: \q and Command to run: show databases;
**Do not forget basic SQL syntax and language**

## Connect MySQL to root server:
```
mysql -u root
show databases;


## Set up Regular User Account

```
create user 'opacuser'@'localhost' identified by 'XXXXXXXXX';


## Create Practice Databases

```
create database opacdb;
grant all privileges on opacdb.* to 'opacuser'@'localhost';
show databases;


**to exit out of root user, type 'exit'**

## Logging In and CREATE TABLE

```
mysql -u opacuser -p
show databases;
use opacdb;

## Similar to the Interface in MS Access for "Design View"
### Enter the SQL Statement (example below)

```
create table books (
id int unsigned not null auto_increment,
author varchar(150) not null,
title varchar(150) not null,
copyright date not null,
primary key (id)
);


```
show tables;
describe books;


## MySQL Commands

### To Add:

INSERT into [table name] (column1, column2, column3) values ('value 1', 'value2', 'value3'), (repeated for the rest of values);
**always end with a semi-colon**

### View Records
```
select * from books;


#### This is the SELECT command

## More Command Examples from the Lecture
```
select author from books;
select copyright from books;
select author, title from books;
select author from books where author like '%millet%';
select title from books where author like '%mbue%';
select author, title from books where title not like '%e';
select * from books;
alter table books add publisher varchar(75) after title;
describe books;
update books set publisher='Simon \& Schuster' where id='1';
update books set publisher='Penguin Random House' where id='2';
update books set publisher='W. W. Norton \& Company' where id='3';
update books set publisher='Knopf' where id='4';
select * from books;
delete from books where author='Julia Phillips';
insert into books
(author, title, publisher, copyright) values
('Emma Donoghue', 'Room', 'Little, Brown \& Company', '2010-08-06'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000-01-27');
select * from books;
select author, publisher from books where copyright < '2011-01-01';
select author from books order by copyright;
\q


# Complete the Connection between PHP and MySQL

### PHP Support Installation
```
sudo apt install php-mysql php-mysqli
sudo systemctl restart apache2
sudo systemctl restart mysql


### Create PHP Scripts

```
cd /var/www/html/
sudo touch login.php
sudo chmod 640 login.php
sudo chown :www-data login.php
ls -l login.php
sudo nano login.php


```
<?php // login.php
$db_hostname = "localhost";
$db_database = "opacdb";
$db_username = "opacuser";
$db_password = "XXXXXXXXX";
?>


```
sudo nano opac.php


## PHP Script that is displayed on the page (which was posted on CANVAS)
```
<html>
<head>
<title>MySQL Server Example</title>
</head>
<body>

<h1>A Basic OPAC</h1>

<p>We can retrieve all the data from our database and book table
using a couple of different queries.</p>

<?php

// Load MySQL credentials
require_once 'login.php';

// Establish connection
$conn = mysqli_connect($db_hostname, $db_username, $db_password) or
  die("Unable to connect");

// Open database
mysqli_select_db($conn, $db_database) or
  die("Could not open database '$db_database'");

echo "<h2>Query 1: Retrieving Publisher and Author Data</h2>";

// Query 1
$query1 = "select * from books";
$result1 = mysqli_query($conn, $query1);

while($row = $result1->fetch_assoc()) {
    echo "<p>Publisher " . $row["publisher"] .
        " published a book by " . $row["author"] .
        ".</p>";
}

mysqli_free_result($result1);

echo "<h2>Query 2: Retrieving Author, Title, Date Published Data</h2>";

$result2 = mysqli_query($conn, $query1);
while($row = $result2->fetch_assoc()) {
    echo "<p>A book by " . $row["author"] .
        " titled <em>" . $row["title"] .
        "</em> was released on " . $row["copyright"] .
        ".</p>";
}

// Free result2 set
mysqli_free_result($result2);

/* Close connection */
mysqli_close($conn);

?>

</body>
</html>

## Test Syntax
```
sudo php -f login.php
sudo php -f index.php



# Unit Reflection

>These tasks seemed much more daunting before I started them. I made sure to take my time and look into the applications of how all of these programs work together.
>Checking the installations, syntax, and other aspects are crucial to avoid small and confusing errors. 
>Checking the HTTP Server Interface us crucial to ensure the processes are working properly (example from the lecture video).
> Connecting this material with the information I learned in LIS 638 (Internet Technology and Information Services) and LIS 668 (Database Management) was helpful!


# Questions, Implications, and applications
>Can this help with the revision or editing of Subject Headings within a library catalog? 
>What about a Discovery tool and more? 
>Is this what was going on in reference to the Subject Heading Group I saw present?
>Could this be used in other DEI intiatives?


