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
## "Santiago": Find the secret combination

Description: Alice the spy has hidden a secret number combination, find it using these instructions:

1) Find the number of lines with occurrences of the string Alice (case sensitive) in the *.txt files in the /home/admin directory
2) There's a file where Alice appears exactly once. In that file, in the line after that "Alice" occurrence there's a number.
Write both numbers consecutively as one (no new line or spaces) to the solution file. For example if the first number from 1) is 11 and the second 22, you can do echo -n 11 > /home/admin/solution; echo 22 >> /home/admin/solution or echo "1122" > /home/admin/solution.

Test: Running md5sum /home/admin/solution returns d80e026d18a57b56bddf1d99a8a491f9(just a way to verify the solution without giving it away.)

Time to Solve: 15 minutes.

```
user@user:/$ cd /home/admin/
user@user:~$ ls
11-0.txt  1342-0.txt  1661-0.txt  84-0.txt  agent  solution
user@user:~$ cat *.txt|grep Alice|wc
    411    4781   28316
user@user:~$ cat 1342-0.txt|grep -n Alice
33:                                Alice
user@user:~$ cat 1342-0.txt|head -35
The Project Gutenberg eBook of Pride and prejudice, by Jane Austen

This eBook is for the use of anyone anywhere in the United States and
most other parts of the world at no cost and with almost no restrictions
whatsoever. You may copy it, give it away or re-use it under the terms
of the Project Gutenberg License included with this eBook or online at
www.gutenberg.org. If you are not located in the United States, you
will have to check the laws of the country where you are located before
using this eBook.

Title: Pride and prejudice

Author: Jane Austen

Release Date: November 12, 2022 [eBook #1342]

Language: English

Produced by: Chuck Greif and the Online Distributed Proofreading Team at
             http://www.pgdp.net (This file was produced from images
             available at The Internet Archive)

*** START OF THE PROJECT GUTENBERG EBOOK PRIDE AND PREJUDICE ***





                            [Illustration:

                             GEORGE ALLEN
                              PUBLISHER
                                Alice
                        156 CHARING CROSS ROAD
                                LONDON
user@user:~$ echo "411156">solution
```
So basically by using grep and wc command we can grep Alice in all txt files (*.txt) and wc (word count) command we can see the total number of Alice.
"There's a file where Alice appears exactly once" for this one I basically did "cat file_name.txt|grep Alice" and try to catch the file. After I found it I got it.

---
## "Lhasa": Easy Math


Description: There's a file /home/admin/scores.txt with two columns (imagine the first number is a counter and the second one is a test score for example).
Find the average (more precisely; the arithmetic mean: sum of numbers divided by how many numbers are there) of the numbers in the second column (find the average score).

Use exactly two digits to the right of the decimal point. i. e., use exaclty two "decimal digits" without any rounding. Eg: if average = 21.349 , the solution is 21.34. If average = 33.1 , the solution is 33.10.

Save the solution in the /home/admin/solution file, for example: echo "123.45" > ~/solution

Tip: There's bc, Python3, Golang and sqlite3 installed in this VM.
Test: md5sum /home/admin/solution returns 6d4832eb963012f6d8a71a60fac77168 solution
Time to Solve: 15 minutes.
```
user@user:~$ ls
README.txt  agent  scores.txt  solution
user@user:~$ tail scores.txt 
91 3.5
92 4.4
93 3.5
94 4.2
95 8.0
96 5.5
97 7.2
98 4.8
99 4.7
100 9.0
user@user:~$ awk '{ total += $2; count++ } END { printf "%.2f", total/count }' scores.txt
5.20
user@user:~$ echo "5.20" > solution
```
printf "%.2f" basically use two decimal digits. 

---
## "Bilbao": Basic Kubernetes Problems

Description: There's a Kubernetes Deployment with an Nginx pod and a Load Balancer declared in the manifest.yml file. The pod is not coming up. Fix it so that you can access the Nginx container through the Load Balancer.
There's no "sudo" (root) access.
Test: Running curl 10.43.216.196 returns the default Nginx Welcome page.
See /home/admin/agent/check.sh for the test that "Check My Solution" runs.

Let's check the pods and describe the pod to see the events
```
admin@i-0d42ad3f499d7c114:~$ kubectl  get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-67699598cc-zrj6f   0/1     Pending   0          146d
admin@i-0d42ad3f499d7c114:~$ kubectl  describe pod nginx-deployment-67699598cc-zrj6f
Name:             nginx-deployment-67699598cc-zrj6f
Namespace:        default
Priority:         0
Service Account:  default
Node:             <none>
Labels:           app=nginx
                  pod-template-hash=67699598cc
Annotations:      <none>
Status:           Pending
IP:               
IPs:              <none>
Controlled By:    ReplicaSet/nginx-deployment-67699598cc
Containers:
  nginx:
    Image:      localhost:5000/nginx
    Port:       80/TCP
    Host Port:  0/TCP
    Limits:
      cpu:     100m
      memory:  2000Mi
    Requests:
      cpu:        100m
      memory:     2000Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-mslhc (ro)
Conditions:
  Type           Status
  PodScheduled   False 
Volumes:
  kube-api-access-mslhc:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Guaranteed
Node-Selectors:              disk=ssd
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  146d  default-scheduler  0/2 nodes are available: 1 node(s) didn't match Pod's node affinity/selector, 1 node(s) had untolerated taint {node.kubernetes.io/unreachable: }. preemption: 0/2 nodes are available: 2 Preemption is not helpful for scheduling..
  Warning  FailedScheduling  42s   default-scheduler  0/2 nodes are available: 1 node(s) didn't match Pod's node affinity/selector, 1 node(s) had untolerated taint {node.kubernetes.io/unreachable: }. preemption: 0/2 nodes are available: 2 Preemption is not helpful for scheduling..
````
Hmm, here is the keyword **didn't match Pod's node affinity/selector**. Let's see the deployment yaml and let's see nodes' labels.

```
admin@i-0eab1b477684c95df:~$ cat manifest.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: localhost:5000/nginx
        ports:
        - containerPort: 80
        resources:
          limits:
            memory: 2000Mi
            cpu: 100m
          requests:
            cpu: 100m
            memory: 2000Mi
      nodeSelector:
        disk: ssd

---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  clusterIP: 10.43.216.196
  type: LoadBalancer
  ```

For the deployment ***nodeSelector.disk: ssd*** , let's check the labels for nodes. 
```
admin@i-0eab1b477684c95df:~$ kubectl get nodes --show-labels
NAME                  STATUS     ROLES                  AGE    VERSION        LABELS
node1                 Ready      control-plane,master   146d   v1.28.5+k3s1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=k3s,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=node1,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=true,node-role.kubernetes.io/master=true,node.kubernetes.io/instance-type=k3s
i-02f8e6680f7d5e616   NotReady   control-plane,master   146d   v1.28.5+k3s1   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=k3s,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=i-02f8e6680f7d5e616,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=true,node-role.kubernetes.io/master=true,node.kubernetes.io/instance-type=k3s
```
As you can see we do not have disk:ssd labeled node.

We have two solutions: Either we label any node with the given value, or we will remove nodeSelector from deployment yaml and re-apply. I picked the second one.

````
admin@i-0eab1b477684c95df:~$ kubectl apply -f manifest.yml 
deployment.apps/nginx-deployment configured
service/nginx-service unchanged

admin@i-0d42ad3f499d7c114:~$ kubectl  get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-67699598cc-zrj6f   0/1     Pending   0          150d
````
It's still in Pending situtation. Let's check again.
````
admin@i-0eab1b477684c95df:~$ kubectl describe pod nginx-deployment-5744b8dff-vk5nd | tail
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Guaranteed
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  44s   default-scheduler  0/2 nodes are available: 1 Insufficient memory, 1 node(s) had untolerated taint {node.kubernetes.io/unreachable: }. preemption: 0/2 nodes are available: 1 No preemption victims found for incoming pod, 1 Preemption is not helpful for scheduling..
````
**1 Insufficient memory, 1 node(s) had untolerated taint** that's the problem. We can reduce memory requesti to 1000Mi and re-apply

And here we go. Now we can test curl.
```
admin@i-0d42ad3f499d7c114:~$ kubectl  get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-67699598cc-zrj6f   1/1     Running   0          146d
````

