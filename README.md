# SadServers
Sad Servers Walkthrough

## "Saskatoon": counting IPs

Description: There's a web server access log file at /home/admin/access.log. The file consists of one line per HTTP request, with the requester's IP address at the beginning of each line.

Find what's the IP address that has the most requests in this file (there's no tie; the IP is unique). Write the solution into a file /home/admin/highestip.txt. For example, if your solution is "1.2.3.4", you can do echo "1.2.3.4" > /home/admin/highestip.txt

Test: The SHA1 checksum of the IP address sha1sum /home/admin/highestip.txt is 6ef426c40652babc0d081d438b9f353709008e93 (just a way to verify the solution without giving it away.)

Time to Solve: 15 minutes

```
user@user:~$ cat /home/admin/access.log

23.30.147.145 - - [20/May/2015:16:05:39 +0000] "GET /presentations/logstash-metrics-sf-2012.10/lib/css/zenburn.css HTTP/1.1" 200 1791 "http://semicomplete.com/presentations/logstash-metrics-sf-2012.10/" "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1700.107 Safari/537.36"
---continuous---

user@user:~$ cat access.log | awk '{print $1}' | sort | uniq -c | sort

.
.
99 68.180.224.225
102 209.85.238.199
113 50.16.19.13
273 75.97.9.59
357 130.237.218.86
364 46.105.14.53
482 66.249.73.135

user@user:~$ echo "66.249.73.135" > /home/admin/highestip.txt
user@user:~$ sha1sum /home/admin/highestip.txt
6ef426c40652babc0d081d438b9f353709008e93 highestip.txt

```
---