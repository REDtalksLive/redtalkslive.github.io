---
title: 'REDtalks #18 - Enabling the docker TCP API in AWS'
date: Fri, 26 May 2017 23:41:29 +0000
draft: false
tags: ['API', 'AWS', 'docker', 'words']
---

Not a traditional REDtalks post today (no interview/demo), but this took me a while to work out so I thought I'd share.

What's this about?
------------------

It all started with me building REST extensibility solutions for F5 Networks in AWS. I created (Launched) a new AWS AMI Linux instance - yep, the very first one on the list: "_Amazon Linux AMI 2017.03.0 (HVM), SSD Volume Type_". Next, I followed the AWS instructions to install docker:```
sudo yum update -y

sudo yum install -y docker

sudo service docker start

sudo usermod -a -G docker ec2-user

docker info
```NOTE: Full docs here: http://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html

This is where I got stuck!
--------------------------

As part of the solution I needed to issue a `docker` command on the docker host, from inside a container... Ok, Batman, to the Google-copter... There's loads of suggestions out there to map `/var/run/docker.sock` into the container using -v. For example:```
$ docker run -it -v /var/run/docker.sock:/var/run/docker.sock my\_container sh
```With this you can execute:```
$ curl --unix-socket /var/run/docker.sock http:/containers/json
```**HOWEVER**, there are loads of forum posts saying to be real careful about mapping /var/run/docker.sock to all you containers...

What to do?
-----------

_**Enable the API over TCP! **_ Back to the Google-copter, there are a few posts out there about getting it running on Ubuntu but none for the Linux AMI distro...

A solution (hours later...)
---------------------------

1\. Change some startup options: The default 'OPTIONS' in `/etc/init.d/docker` is:```
OPTIONS="${OPTIONS:-${other\_args}}"
```we need to change this to:```
OPTIONS="${OPTIONS:-${other\_args}} -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
```So, you need to go ahead and edit that to something link this:```
$ sudo vi /etc/init.d/docker

#OPTIONS="${OPTIONS:-${other\_args}}"
OPTIONS="${OPTIONS:-${other\_args}} -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
```2\. Restart `docker` for these options to take effect: `sudo service docker restart` Now you have enabled the docker API over TCP! #w00t

Test the API
------------

Lets get the API version:```
curl http://<ip\_address>:2375/version
```NOTE: Replace <ip\_address> with the IP address of the docker host, or its hostname! The response will look something like this:```
{"Version":"1.12.6","ApiVersion":"1.24","GitCommit":"7392c3b/1.12.6","GoVersion":"go1.6.3","Os":"linux","Arch":"amd64","KernelVersion":"4.9.20-11.31.amzn1.x86\_64","BuildTime":"2017-03-07T20:34:04.601909006+00:00"}
```Note the:```
"ApiVersion":"1.24"
```Now add that version number to the beginning of the URI, slap `json` on the end of it, and presto:```
curl http://<ip\_address>:2375/v1.24/images/json
```Returns:```
\[{"Id":"sha256:80ee3a0f225e543668eca9922bf5642bce0e484403df665f3ac9b107d2895d40","ParentId":"","RepoTags":\["npearce/ilxe-festivus:latest"\],"RepoDigests":\["npearce/ilxe-festivus@sha256:973dbf813f6a7f07929b8fd86da4fa9b79f613228e3942fb35d9d525fcfa61b0"\],"Created":1495771390,"Size":85001082,"VirtualSize":85001082,"Labels":{}}\]
```  Now you can go read this: [https://docs.docker.com/engine/api/v1.24/](https://docs.docker.com/engine/api/v1.24/) Enjoy! **CAUTION**: One last step, and this is REALLY important! Don't leave your Docker API open on the Internet!