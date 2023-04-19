## Guide for Learning the Command Line Interface

```
list files and directories.................. ls
print name of current/working directory..... pwd
create a new directory...................... mkdir
remove or delete an empty directory......... rmdir
change directory............................ cd
create an empty file........................ touch
print characters to output.................. echo
display contents of a text file............. cat
copy a file or directory.................... cp
move or rename a file or directory.......... mv
remove or delete a file or directory........ rm
```

### The Filesystem
> First, modern operating systems tend to hide the filesystem from their users. So even though, for example, macOS is Unix, many macOS users that I have taught are completely unfamiliar with the layout of directories on their system. This is because, per my observations, macOS Finder does not show the filesytem by default these days. Instead it shows its users some common locations for folders. This might make macOS more usable to most users, but it makes learning the system more difficult.
> *filesystem* can have many meanings, but in here it means the directory location on the Linux systems

```
/home ... home directory
```
- path begins at root and ends at home



### Bash: Bourne Again Shell
- A shell is "both an interactive command language and a scripting language"
- Bash is an acronym for the Bourne Again Shell because it's based on the original Unix shell called the Bourne shell, written by Stephen Bourne. Bash itself was written by Brian Fox.


## Install Practice Programs

- Start at Home directory
```
ls
git clone https://github.com/cseanburns/learn-the-commandline.git
ls
sudo cp learn-the-commandline/* /usr/local/bin
learn-the-cli
learn-the-filesystem
flashcards

