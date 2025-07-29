# Docker Demo for wsgi-to-zpm

Docker Demo for [https://github.com/jamerfort/wsgi-to-zpm](wsgi-to-zpm).

### Docker Demo
If you would like to try out this process from a Docker image, follow these steps:

```bash
$ git clone git@github.com:jamerfort/wsgi-to-zpm-demo.git
$ cd wsgi-to-zpm-demo
$ docker compose up -d
```
Once the Docker container is up and running, connect to it via bash:

```bash
# Connect to the Docker container to run ZPM
$ docker exec -it wsgi-to-zpm-demo-iris-1 bash
```
Once inside the container, go to `./my-wsgi-app/` and run `wsgi-to-zpm`:

```bash
irisowner@4a28b76363cb:/opt/irisapp$ cd my-wsgi-app/

irisowner@4a28b76363cb:/opt/irisapp/my-wsgi-app$ wsgi-to-zpm 

Creating module.xml
Add to module.xml? my-webapp/myapp.py         [Yn] : y
Is this your WSGI callable? app               [Yn] : y
Would you like to update the project name to "my-webapp"? [Yn] : y

Choose a local path to include in the module

    1. .
    2. my-webapp
    3. my-webapp/myapp.py

Enter a choice                                     : 2

Choose a URL for your WSGI app

    1. /my-webapp
    2. /my-webapp/my-webapp

Enter a choice                                     : 1
Choose a base directory to install this WSGI app in

    1. ${libDir}
    2. ${mgrDir}
    3. ${cspDir}

Enter a choice                                     : 3

Saving module.xml
irisowner@4a28b76363cb:/opt/irisapp/my-wsgi-app$ 
```

A new `module.xml` has been created, allowing this directory to be installed as a ZPM package:

```bash
irisowner@4a28b76363cb:/opt/irisapp/my-wsgi-app$ ls -ltr

total 12
-rw-rw-r-- 1 irisowner irisowner    6 Jul 29 05:47 requirements.txt
drwxrwxr-x 4 irisowner irisowner 4096 Jul 29 05:47 my-webapp
-rw-r--r-- 1 irisowner irisowner  911 Jul 29 05:51 module.xml
```

Start ZPM from an IRIS terminal, and load this WSGI app as a zpm package.


```cls
irisowner@4a28b76363cb:/opt/irisapp/my-wsgi-app$ iris terminal IRIS

Node: 4a28b76363cb, Instance: IRIS

USER>zn "IRISAPP"

IRISAPP>zpm

=============================================================================
|| Welcome to the Package Manager Shell (ZPM). Version:                    ||
|| Enter q/quit to exit the shell. Enter ?/help to view available commands ||
|| Current registry https://pm.community.intersystems.com                  ||
=============================================================================
zpm:IRISAPP>load /opt/irisapp/my-wsgi-app

[IRISAPP|my-webapp]	Reload START (/opt/irisapp/my-wsgi-app/)
[IRISAPP|my-webapp]	requirements.txt START
[IRISAPP|my-webapp]	requirements.txt SUCCESS
[IRISAPP|my-webapp]	Reload SUCCESS
[my-webapp]	Module object refreshed.
[IRISAPP|my-webapp]	Validate START
[IRISAPP|my-webapp]	Validate SUCCESS
[IRISAPP|my-webapp]	Compile START
[IRISAPP|my-webapp]	Compile SUCCESS
[IRISAPP|my-webapp]	Activate START
[IRISAPP|my-webapp]	Configure START
[IRISAPP|my-webapp]	Configure SUCCESS
[IRISAPP|my-webapp]	Activate SUCCESS
zpm:IRISAPP>
```

Visiting the URL **http://[host]:[port]/my-webapp/** will now serve your WSGI app (use SuperUser with "SYS" password for this demo).
