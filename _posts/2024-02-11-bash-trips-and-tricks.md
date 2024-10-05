---
title: Bash&#58 Trips & Tricks
author: edc0
categories: [Bash, Snippets]
tags: [bash, scripts, snippets]
render_with_liquid: false
---


### Function: Do or die Script:

```bash
function do_or_die {
 $@ || { echo "Command failed: $@" && exit 1; }
}

do_or_die test -f /important/file

echo "Phew, everything's fine"

# => "Command failed: test -f /important/file"
```

### Lock file or restart app with flock:
```bash
if [ `flock -xn /tmp/script.lock -c 'echo 1'` ]; then 
   echo 'it's not running'
   restart.
else
   echo -n 'it's already running with PID '
   cat /tmp/script.lock
fi
```

Reference: http://stackoverflow.com/questions/696839/how-do-i-write-a-bash-script-to-restart-a-process-if-it-dies


### Kill process by port:

```bash

sudo netstat -anp --numeric-ports | grep ":${PORT}\>.*:" | awk -F " " '{print $7}' | head -n1 | cut -f1 -d "/"


Remove comments (#) from file:


grep -o '^[^#]*' file > new_file


sed 's/#.*$//g' filename
```



### Disk-usage by folder:

```bash
du -h --max-depth=1 | sort -hr

# Check folder size with du
du -hs /var/log/journal/
```



### FIND Examples:

```bash

#Recursively find in directory
find . -name "foo.log*"


cd /var/log/grnc_hub/$(date +%Y)/$(date +%m)/ && find . -name "hotelbeds_si_new.*"


#Delete with pattern
find . -name '217.*' -exec rm -rf {} \;




#Remove older (< 3 months) build files
find . -type f -mtime +90 -name '*.gz' -execdir rm -rf -- '{}' +



# find files modified greater than 24 hours ago
find . -mtime +0

# find files modified between now and 1 day ago
find . -mtime 0 

# (i.e., in the past 24 hours only)

# find files modified less than 1 day ago
find . -mtime -1  #(SAME AS -mtime 0)

# find files modified between 24 and 48 hours ago
find . -mtime 1 

# find files modified more than 48 hours ago
find . -mtime +1 



find . -mmin +5 -mmin -10 # find files modified between


# 6 and 9 minutes ago
find / -mmin -10 # modified less than 10 minutes ago
```



### Bash-Regex-Tutorial:
[https://www.cyberciti.biz/faq/grep-regular-expressions/](https://www.cyberciti.biz/faq/grep-regular-expressions/)


### Run Command With SSH-Tunnel:

```bash
ssh -f server1 -tt "cat /etc/environment | grep word” && \
ssh -f server2 -tt "cat /etc/haproxy/haproxy.json | grep something-else && \
echo "all done \n"
```


### AWK:

```bash
#In-file replacement
echo "one potato two potato" | awk '{gsub(/potato/,"banana")}1'


awk '{sub(/blue/,"azure")}1' input.txt
```


### GREP & Egrep:

```bash
#Examples
egrep "foo|bar" *.txt
grep -E -- 'foo|bar' *.txt
grep -e foo -e bar -- *.txt


#grep-and match in log files
tail -f /var/log/haproxy.log | grep -E -- ' 401 .*dbed284fccf84992983d91e00807b6f5'


#grep or match
tail -f /var/log/haproxy.log | grep -E -- ' 401 |*dbed284fccf84992983d91e00807b6f5'


#Find all 500 related error from haproxy log
cat /var/log/haproxy.log | grep -P ' 5\d{1}\d{1} '


cat /var/log/haproxy.log | grep -P ' \d{3} \d{3} - - sH--'
cat /var/log/haproxy.log | grep -P ' \d{3} \d{3} - - CL--'


#Search for files under root
grep -s . /*/tmpfiles.d/*.conf /*/*/tmpfiles.d/*.conf
```


### Log-Between-timestamps:

```bash
sed -rne '/2019-12-27T15:50:00/,/2019-12-27T16:11:58/ p' /var/log/syslog
awk -v from="10:30:33" -v to="10:30:33" '$1>=from && $1<=to' /var/log/syslog
egrep -B 50 -A 50 '2020-02-01T15:[4-5][0-1]:[0-9][0-9]' /var/log/syslog
```


### SED:

```bash

#listen = 127.0.0.1:9000
sudo sed -i "s/listen =.*/listen = 127.0.0.1:9000/" /etc/php/7.2/fpm/pool.d/www.conf
```



### PS Memory usage (Human readable format):

```bash

# Print outputs in specified column
ps -eo pid,cmd,size,%mem,%cpu --sort=-rss | head

# Print output with sorting
ps -eo size,mem,pid,user,command --sort -size | awk '{ hr=$1/1024 ; printf("%13.2f Mb ",hr) } { for ( x=4 ; x<=NF ; x++ ) { printf("%s ",$x) } print "" }'

# Another example
ps -eo size,pid,user,cmd --sort -size | awk '{ hr=$1/1024 ; printf("%13.2f Mb ",hr) } { for ( x=4 ; x<=NF ; x++ ) { printf("%s ",$x) } print "" }'


# command-> cmd (sort form), comm -> command name



# Some more examples
ps -eo pmem,comm,pid,maj_flt,min_flt,rss,vsz --sort -rss | numfmt --header --to=iec --field 4-5 | numfmt --header --from-unit=1024 --to=iec --field 6-7 | column -t | head


# This is shorter version
ps -eo pid,pmem,rss,vsz,comm --sort -rss | numfmt --header --from-unit=1024 --to=iec --field 3-4 | column -t | awk '$3 != 0'
```


### PS list all process names:

```bash
ps -ejH
ps -ejH | grep fail2ban
```


### Check-if-port-in-use:

```bash
# With nc
nc -z 127.0.0.1 80 && echo "IN USE" || echo "FREE"

#For remote machines
nc -w 2 -z 216.58.196.174 81
```

Reference: [https://stackoverflow.com/questions/14001825/how-to-check-if-a-certain-port-is-open-and-unused](https://stackoverflow.com/questions/14001825/how-to-check-if-a-certain-port-is-open-and-unused)


### Check-File-Existence:

```
File Testing
-b filename - Block special file
-c filename - Special character file
-d directoryname - Check for directory Existence
-e filename - Check for file existence, regardless of type (node, directory, socket, etc.)
-f filename - Check for regular file existence not a directory
-G filename - Check if file exists and is owned by effective group ID
-G filename set-group-id - True if file exists and is set-group-id
-k filename - Sticky bit
-L filename - Symbolic link
-O filename - True if file exists and is owned by the effective user id
-r filename - Check if file is a readable
-S filename - Check if file is socket
-s filename - Check if file is nonzero size
-u filename - Check if file set-user-id bit is set
-w filename - Check if file is writable
-x filename - Check if file is executable
```

***How to use:***

```bash
file=./file
if [ -e "$file" ]; then
 echo "File exists"
else
 echo "File does not exist"
fi

#A test expression can be negated by using the ! operator
file=./file
if [ ! -e "$file" ]; then
 echo "File does not exist"
else
 echo "File exists"
fi
```



### Crash-Dump Backup With Datestamp:

```bash
sudo aws s3 cp /var/log/syslog.dump s3://crash-dump/$(hostname)/$(date -r /var/log/syslog.dump "+%Y/%m/%d/%H-%M")/
```

