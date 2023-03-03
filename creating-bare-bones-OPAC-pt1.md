#Creating Bare Bones OPAC pt.1

## Creating the HTML Page and a PHP Search Page
-HTML Page wil be called **opacbb.html**
-PHP script that is activated will be called **search.php** which will establish the connection to the OPAC Database that was previously created
-MySQL still used and SQL queries will be used/conducted

```
sudo nano opaccb.html
sudo nano search.php

>Do not forget to add 'sudo' or else you will not be able to make the files within the directories!
```
<html>
<head>
<title>MySQL Server Example</title>
</head>
<body>

<h1>A Basic OPAC</h1>

<p>In the form below,
<b>optionally</b> enter text in the search field.
You can search by author, title, or publisher.
Capitalization is not necessary.
It's okay to enter partial information,
like part of an author's, title's, or publisher's name.</p>

<p>The date fields are <b>required</b>.
You can use the date fields to limit results.
I added some extra records,
which you can view to know what you can query:</p>

<p><a href="http://11.111.222.222/opac.php">http://11.111.222.222/opac.php</a></p>

<p>This is very much a toy, stripped down OPAC.
The records are basic.
Not only do they not conform to MARC,
but they don't even conform to something
as simple as Dublin Core.
I also don't provide options
to select different fields,
like author, title, or publisher fields.
Instead the search field below searches
all the fields in our <b>books</b> table.
The key idea is to get a sense,
an intuition, of how an OPAC works, though.</p>

<h2>My Basic Library OPAC</h2>
<form method="post" action="search.php">
    <label for="search">Search:</label>
    <input type="text" name="search" id="search">
    <br>
    <label for="start_date">Start Date:</label>
    <input type="date" name="start_date" id="start_date">
    <br>
    <label for="end_date">End Date:</label>
    <input type="date" name="end_date" id="end_date">
    <br>
    <input type="submit" value="Search">
</form>


</body>
</html>



## PHP Search Script

```
<?php
// Load MySQL credentials
require_once 'login.php';

// Establish connection
$conn = mysqli_connect($db_hostname, $db_username, $db_password) or
  die("Unable to connect");

// Open database
mysqli_select_db($conn, $db_database) or
  die("Could not open database '$db_database'");

// Check if search query was submitted
if (isset($_POST['search'])) {
    // Sanitize user input to prevent SQL injection attacks
    $search = mysqli_real_escape_string($conn, $_POST['search']);

    // Get the start and end dates for the date range
    $start_date = mysqli_real_escape_string($conn, $_POST['start_date']);
    $end_date = mysqli_real_escape_string($conn, $_POST['end_date']);

    // Build the MySQL query with a WHERE
    // clause that includes the date range filter
    $query = "SELECT * FROM books WHERE
        (author LIKE '%$search%' OR
        title LIKE '%$search%' OR
        publisher LIKE '%$search%') AND
        copyright BETWEEN '$start_date' AND '$end_date'";

    // Execute the query
    $result = mysqli_query($conn, $query);

    // Check if any results were returned
    if (mysqli_num_rows($result) > 0) {
        // Loop through the results and output them
        while ($row = mysqli_fetch_assoc($result)) {
            echo "ID: " . $row["id"] . "<br>";
            echo "Author: " . $row["author"] . "<br>";
            echo "Title: " . $row["title"] . "<br>";
            echo "Publisher: " . $row["publisher"] . "<br>";
            echo "Copyright: " . $row["copyright"] . "<br><br>";
        }
    } else {
        echo "No results found.";
    }

    // Free up memory by closing the MySQL result set
    mysqli_free_result($result);
}

// Close the MySQL connection
mysqli_close($conn);

echo "<p>Return to search page: <a href='http://11.111.222.222/opacbb.php'>http://11.111.222.222/opacbb.php</a></p>";

?>

##### Be sure to change the above and throughout this document the IP Address to my own IP Address

## Modifications

### Connect to the MySQL Server
```
mysql -u opacuser -p


### Run the INSERT command
```

insert into books
(author, title, publisher, copyright) values
('Emma Donoghue', 'Room', 'Little, Brown \& Company', '2010-08-06'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000-01-27');


## Reflection:
1. Much larger OPAC would not use these methods because creating a search form for thousands and millions of results would not be useful.
1. Additionally Discovery System like InfoKAT would not use this because it is much more advanced and complex (but similar in theory). 
1. ILS rely on modules that rely on relational databases with complex and interconnected tables *think of LIS 668 DB Management* This only relies on one table, but this is much larger in actual working libraries
1.Gave a better visual into how this would apply on a much larger scale
1. 

##Fundamental Ideas from this process
-Previously created a HTML Page titled 'A Basic OPAC'
-Established a connection and opened the database after loading our MySQL credentials
-Tested out MySQL queries, stored results, then closed the connections
-Now, we are going to use a different PHP code to create a search system that will allow us to retrieve results based on the MySQL Query, which is closely mimicking an OPAC
-ultimately, we are creating a means to search this Books Table

