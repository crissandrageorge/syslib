# Install Koha ILS
> Koha is an open source "library management system", otherwise called an integrated library system (ILS). These kinds of systems provide modules that perform specific kinds of functionality. 
Koha's modules include:
-	Administration
-	Patron management
-	Cash management
-	Circulation
-	Cataloging
-	Course reserves
-	Serials
-	Acquisitions
-	Reports
-	OPAC
- much more
> According to Library Technology Guides, Koha is installed in "4,040 libraries [around the world], spanning 5,677 facilities or branches". Most installations are in medium sized or small libraries. Koha is well represented in academic libraries, but the majority of installations are in public libraries.
> Although Koha is an open source ILS and free to download, install, and administer without external support, librarians can hire companies that support open source library management solutions, like ByWater Solutions or the Equinox Open Library Initiative These companies support ILS migration, hosting, training, and more. They also provide support for other library software services, such as open source discovery systems and electronic resource management systems.
> In addition to Koha, Evergreen is also an open source integrated library system. According to Library Technology Guides, Evergreen is primarily installed at small and medium size public libraries, and most installations are in the U.S. and Canada.
> There is currently a migration to what has been called library service platforms (LSP) in recent years. The LSP is a next generation ILS that is designed from the start to integrate electronic resources. For example, the ILS has an OPAC which was designed to search a library's print collections. Modern OPACs have been adapted for electronic resources, but they are still limited because of the older design model. LSPs use a discovery service instead of an OPAC. Discovery services are designed to search a library's entire collection, including the content in third party databases and journals. Example LSPs include Ex Libris Primo (used by UK Libraries), OCLC's WorldCat Discovery Service, and open source solutions like Aspen Discovery and VuFind.
> It is probably unnecessary to state that integration of library systems like the ILS and the LSP is a major part of modern libraries. When we visit a library's website, we will first interact with a normal website that might be built on WordPress, Drupal, or some other content management system. But these websites will link to the public facing components of an ILS or LSP, as well as other services, such as bibliographic database, journal publishers, ebook services, and more. It may therefore be the systems librarians job to help build and connect these services. In this demo, we will continue that work by installing, configuring, and setting up the Koha ILS.
# Google Cloud Setup
> Before we begin to install Koha, we need to create a new virtual machine instance and configure the Google firewall to allow HTTP traffic to our Koha install.
## New Virtual Instance
> The virtual instance we have been using does not meet the memory (RAM) needs required by the Koha integrated library system. We therefore need to create a new virtual instance that has more RAM. As a refresher, see the section titled gcloud VM Instance at Using gcloud Virtual Machines. However, we will modify the Series to E2 and set the Machine Type to 2 vCPU, 4 GB memory. All else, including the operating system (Ubuntu 20.04), should remain the same. Note that this is a more expensive setup. Therefore, feel free to delete this instance at the end of the semester.
## Google Cloud Firewall
> Later, after we install Koha, we will need to access the staff interface on a special HTTP port. HTTP (i.e., web) traffic is delivered through what are called ports. The default port for HTTP is 80, and the default port for HTTPS is 443. Since we do not have encryption enabled, this means that the Apache2 web server listens on port 80 by default for all web traffic. Firewalls are used to control incoming and outcoming traffic via ports. 
> When we selected Allow HTTP traffic when we created our virtual instance, we instructed the Google Console firewall to allow traffic through port 80. We need to add a firewall rule to allow web traffic through port 8080. We will use port 8080 to access the Koha staff interface.
### To create a firewall rule to allow traffic to port 8080, go to the Google Cloud Console:
-	Click on the hamburger ☰ icon at the top left.
-	Click on VPN Network
-	Click on Firewall
-	At the top of the page, choose Create a firewall rule (do not choose Create a firewall policy)
o	Add name: koha
o	Add description: open port 8080
-	Next to Targets, click on All instances in the network
-	In the Source IPv4 ranges, add 0.0.0.0/0
-	Click on Specified protocols and ports
o	Click on TCP
o	Add 8080 in the Ports box
-	Click on Create
# Install Koha Repo
> log onto our new server and prepare it for the Koha installation
> update our local repositories
> upgrade our servers
```
Sudo apt update
Sudo apt upgrade
```
> The next two commands help save disk space. The apt autoremove command "is used to remove packages that were automatically installed to satisfy dependencies for other packages and are now no longer needed as dependencies changed or the package(s) needing them were removed in the meantime" (see man apt). The apt clean command "clears out the local repository of retrieved package files" (see man apt-get). In the following example, I combine both commands on one line:
```
sudo apt autoremove -y && sudo apt clean
```
> Next we need to install gnupg2, which is used to create digital signatures, encrypt data, and aid in secure communication.
```
sudo apt install gnupg2
```
> At the time of this demo, the update above downloaded a new Linux kernel. Using the new kernel requires a reboot. The reboot command will disconnect you from the server. Just wait a minute or so and then re-connect.
```
sudo reboot now
```
### Add Koha Repository
> When you run the sudo apt update command, Ubuntu syncs the local repository database with several remote repositories. These remote repositories contain metadata about the packages they contain. The syncing process identifies if any new software updates are available. The remote repositories are also used to retrieve software.
> We can add repositories to sync with and to use to download software, and this includes the Koha ILS. To add the special Koha repository to our system, we use the following command:
```
sudo su
echo 'deb http://debian.koha-community.org/koha stable main' | sudo tee /etc/apt/sources.list.d/koha.list
wget -q -O- https://debian.koha-community.org/koha/gpg.asc | sudo apt-key add -
```
# Install Koha
```
apt update
apt show koha-common
apt install koha-common
```
# Configure Koha
```
nano /etc/koha/koha-sites.conf
```
- Change file line of “ INTRAPORT="80"
- To “ INTRAPORT="8080"
```
apt install mysql-server
mysqladmin -u root password bibliolib1
a2enmod rewrite
a2enmod cgi
systemctl restart apache2
koha-create --create-db bibliolib
nano /etc/apache2/ports.conf
Listen: 8080
systemctl restart apache2
a2dissite 000-default
a2enmod deflate
a2ensite bibliolib
systemctl reload apache2
systemctl restart apache2
```
## Koha Web Installation
> All the backend work is complete, and like we did with WordPress and Omeka, we can complete the installation through a web installer.
> First, get Koha username and password in the following file:
```
nano /etc/koha/sites/bibliolib/koha-conf.xml
```
> Look for the <config> stanza (line number 252) and the line beginning with <user> (line number 257). The password is on the line after (line number 258).
> Make sure your URL begins with http* and not https, and visit the web installer at:
```
http://IP-ADDRESS:8080
```
> The documentation for the web installer is helpful. One thing to do is to add sample libraries and sample patrons. More generally, be sure to follow instructions as you click through each step.
# Public OPAC
> When the install and setup are complete, you will have access to the staff interface. To view the public facing OPAC, you need to make a setting change.
- Click on More in the top drop down box
- Click on Administration
- Click on Global System Preferences
- Click on OPAC in the left hand side bar
- Scroll down to the OPACBaseURL line.
- Enter the IP address of your server: http://IP-ADDRESS
- Click on Save all OPAC Preferences
> Once you save these preferences, you should be able to visit your public facing OPAC at the server IP address.
## Additional Tasks
> Once you've installed and setup Koha, begin to learn the system. Some example tasks:
- Create patron accounts
- Create bibliographic records
- Check out books to patrons
- Delete patron circulation history

# Reflection
1. It was really exciting to see all of these elements come together, especially in such a realistic view within the field	
1. There a lot of add ons within a library website. the first page being word press or drupal and then catalog being different. This further shows the many parts that come together for a library website.
1. Many of the steps were similar, showing consistency, but diffences in specific key areas
1. The web interface of Koha was much different than the others we have used before, which was cool to see. 




