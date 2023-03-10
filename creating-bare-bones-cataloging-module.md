# Creating Bare Bones Cataloging Module
> SQL interface was used first to understand the foundation of these technologies, but it is not realistic that this would be the interface for always importing data. A graphical interface would be created for this, which in terms of electronic resources, you would need a cataloging module as one of the modules to make up the ILS (integrated library system). Newer systems have different LSPs (library service programs) that would contain more modules. 

# Creating an HTML Page and a PHP Cataloging Page
> These pages will not be fully realistic, but this will give a path to work from in terms of technical standpoint!
- Form needs to mimic the Books Table in data structure
- Structure will be *author, title, publisher, copyright*
# HTML Page Creation
1. Need to create a new directory for **index.html**
2. Use nano to create the html file
```
cd /var/www/html
sudo mkdir cataloging
cd cataloging
sudo nano index.html
```
3. Input content into HTML file
```
<!DOCTYPE html>
<html>
<head>
    <title>Enter Records</title>
</head>
<body>
    <h1>OPAC Library Administration</h1>
 
    <p>This is the library administration page for entering records into the OPAC.</p>
    <p>Please do not use this page unless you are an authorized cataloger.</p>
 
    <form action="insert.php" method="post">
        <label for="author">Author:</label>
        <input type="text" name="author" id="author" required><br><br>
 
        <label for="title">Book Title:</label>
        <input type="text" name="title" id="title" required><br><br>
 
        <label for="publisher">Publisher:</label>
        <input type="text" name="publisher" id="publisher" required><br><br>
 
        <label for="copyright">Copyright:</label>
        <input type="date" id="copyright" name="copyright">
 
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```
# PHP Insert Script
> The user interface will input data into the relation through MySQL, but a form will be used instead of using the MySQL interface. The form provides the manner in which to insert the data. This bibliographic data then needs to be input through the communication of the PHP Script to the MySQL database and Books table. *Notice this path and connections made from start to finish to execute these actions.* 
> **Just as the HTML form has to match the data structure of the books table, the PHP script also needs to match the form from the HTML page and the data structure in the books table.**
1.	Use nano to create insert.php
2.	Input php script
```
<?php
 
// Load MySQL credentials
require_once '../login.php';
 
// Establish connection
$conn = mysqli_connect($db_hostname, $db_username, $db_password) or
  die("Unable to connect");
 
// Open database
mysqli_select_db($conn, $db_database) or
  die("Could not open database '$db_database'");
 
// Prepare and bind SQL statement
$stmt = $conn->prepare("INSERT INTO books (author, title, publisher, copyright) VALUES (?, ?, ?, ?)");
$stmt->bind_param("ssss", $author, $title, $publisher, $copyright);
 
// Set parameters and execute statement
$author = $_POST["author"];
$title = $_POST["title"];
$publisher = $_POST["publisher"];
$copyright = $_POST["copyright"];
 
if ($stmt->execute() === TRUE) {
    echo "New record created successfully";
} else {
    echo "Error: " . $stmt->error;
}
 
// Close statement and connection
$stmt->close();
$conn->close();
 
echo "<p>Return to the cataloging page: <a href='http://11.111.111.111/cataloging/'>http://11.111.111.111/cataloging/</a></p>";
?>
```
# Security Measures
## Since this would be a module that limited access to it would be preferred, security measures need to be taken. 
> Again, in a real-world situation, modules like these would have a variety of security measures in place to prevent wrongful data entry. In our case, we will rely on a simple authorization mechanism provided by the Apache2 server called htpasswd. 

### Steps
-	First, we create an authentication file in our /etc/apache2 directory, which is where the Apache2 web server stores its configuration files. The file will contain a hashed password and a username we give it. 
-	In the following command, I set the username to libcat,
-	Next we need to tell the Apache2 web server that we will use the htpasswd to control access to our cataloging module. To do that, we use nano to open the apache2.conf file. We need:
```
sudo htpasswd -c /etc/apache2/.htpasswd libcat
sudo nano /etc/apache2/apache2.conf
```

-	Next, In the apache2.conf file, look for the following code block / stanza. We are interested in the third line in the stanza, which is line 173 for me, and probably is for you, too.
```
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride None
  Require all granted
</Directory>
```
-	Carefully, we need to change the word None to the word All:
```
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride All
  Require all granted
</Directory>
```

-	Next, change to the cataloging directory and use nano to create a file called .htaccess (note the leading period):
```
cd /var/www/html/cataloging
sudo nano .htaccess
```
-	Add the following content to .htaccess:
```
AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```
-	Check!!!
```
apachectl configtest
```
-	If you get a Syntax OK message, then restart Apache2 and check its status:
```
sudo systemctl restart apache2
systemctl status apache2
```

# Conclusion
> In the last lesson, we created a very bare bones OPAC that would allow patrons to search our catalog. In this lesson, we learned how to create a very bare bones cataloging module that would allow librarians to add bibliographic data and records to our OPAC.
> Add some records using the above form, and then return to your OPAC and conduct some queries to confirm that the new records have been added.
> You can also use the MySQL command line interface to view the new records, just like we did a couple of lessons ago.
> In a production level environment, we would add quite a bit more. Our MySQL database would contain many more tables that allow storing data related to the modules listed above. We would also like to make our modules graphically attractive and provide more content. That would mean we would add Cascading Style Sheets (CSS) and JavaScript to create an attractive and usable interface. But that would be a whole other class.

 # Reflection
1.	


