# Using Grep
- Grep offers some advanced methods for searching the contents of text files.
- Basically, grep "prints lines that match patterns" (see man grep). In other words, it's search, and it's super powerful.
- grep works line by line. So when we use it to search a file for a string of text, it will return the whole line that matches the string. This line by line idea is part of the history of Unix-like operating systems, and it's important to remember that most utilities and programs that we use on the commandline are line oriented.

# Visualizing how grep is used

> Using Operating-systems.csv
```
OS, License, Year
Chrome OS, Proprietary, 2009
FreeBSD, BSD, 1993
Linux, GPL, 1991
macOS, Proprietary, 2001
Windows NT, Proprietary, 1993
Android, Apache, 2008
```
> Use grep to search
```
grep "Chrome" operating-systems.csv
```
* then it will show the output *

> Be aware that, by default, grep is case-sensitive, which means a search for the string chrome, with a lower case c, would return no results. Fortunately, grep has an -i option, which means to ignore the case of the search string. In the following examples, grep returns nothing in the first search since we do not capitalize the string chrome. 

> Add -i option to to combat case-sensitivity
```
grep -i "chrome" operating-systems.csv
```

# Other Functions to try
> regex can be used with this (remember these commands from Corpus Linguistics classes)
```
-vi...lines that do not match
-vi "^[insert character]"...does not match the string on the first line
-vi "year$"...does not match string on last line
-ic "[insert character]"... count
-io "[insert character]"...print only match and not whole line
-Ei"([insert]|[insert])"...simulate boolean OR search
-i "\<os\>"...only return results with a complete word
-wi "insert"...only return results with a whole word
-i "linux" -A2...print matching line plus two lines after 
-i "linux" -B2...print matching line plus two before
-iw -C1 "bsd"... combination of whole word, case insensitive, print line before and line after
-i -m1 "insert"...return string and stop after first line
-in "insert"...says line number
-Eiw "[a-z]{5}"...character classes and repetition with 5 letter words
-Eiw "[0-9]{4}"... four letter numbers
-Eiw "m.{3}s"...search for words that begin with some letter and end with some letter and with a specified number of letters between
```

# Practice with the /var/log
```
cd /var/log
less auth.log
grep -E "session opened for user (sean|root)" auth.log | less
grep "invalid user" auth.log | grep -Eo "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" | sort | uniq -c | sort
grep "Connection closed by invalid user" auth.log | cut -d" " -f11 | sort | uniq -c | sort |less
grep "Connection closed by invalid user" auth.log | cut -d" " -f11 | sort | uniq -c | sort -r |less
```
