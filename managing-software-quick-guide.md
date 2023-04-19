# Managing Software

> Most Linux distributions use what's called a package manager to handle the installation, upgrades, and uninstalls of the software on a system. The Ubuntu distribution uses a package manager called dpkg and a front-end called apt (advanced package tool). We will use apt to install, update, and remove software from our servers.

# sudo and apt
> sudo execute a command as another user, but main use to is to execite command as the root user
> It is the *super user*
> For security purposes, regular accounts may not add, remove, or update software on a system, nor may they modify most files or directories outside their home directories. Using sudo allows regular users to perform maintenance tasks on our systems by executing commands as the root user. Some consider this safer than logging in as the root user.
> Not all regular users can use the sudo command. On regular Ubuntu distributions, users must belong to the sudo group in order to run the sudo command. The groups command will return a list of groups that your account belongs to. On the Ubuntu version used by the Google Cloud Platform (GCP), the group of interest is called google-sudoers. The difference between the sudo group on regular Ubuntu distributions and the GCP version is that regular users in the google-sudoers group are not prompted for their password.

### Outside our home directory, for example, in the directory /usr/local/bin. We need to use sudo to do such things.
```
cd /usr/local/bin
sudo mkdir data
```
*or*
```
sudo mkdir /usr/local/bin/data
```
#### Create a file in another directory
```
sudo touch data.csv
```


### Sudo Apt Commands

> Commands Updates
```
sudo apt update
```

> Upgrades
```
sudo apt upgrade
```

> Apt Search
> sudo is not needed because there is no modification
```
apt search tldr
```

> Apt Show
> to see more specific information about the package
```
apt show tldr
```

> sudo apt install software
```
sudo apt install tldr
```

> sudo apt remove to remove a package
```
sudo apt remove
```
> To remove system configurations that are not needed
```
sudo apt --purge remove tldr
```

> Few computer applications are self-contained, and they often require other technology. When we uninstall (or remove) applications, the package manager does not auto uninstall those dependencies that were installed with it. 
- We use the autoremove command to uninstall those:
```
sudo apt autoremove
```

> Remove extra files and free up disk space
```
sudo apt clean
```

