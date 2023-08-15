# 1. Assignment 2 

## 1.1. Create a new Docker network named "my_network" using the bridge driver.*


faisa@FaisalBhatti MINGW64 ~

**$ docker network create --driver bridge my_network**

	245c7b04755a202c97605152084c17e3a6283dd5b5313765867a4975cfe11308

faisa@FaisalBhatti MINGW64 ~

**$ docker network ls**
```
	NETWORK ID     NAME         DRIVER    SCOPE
	ceedada28e90   bridge       bridge    local
	5002fbf7fc45   host         host      local
	245c7b04755a   my_network   bridge    local
	be106ca511ba   none         null      local
```
## 1.2. Create a new Docker container using the "nginx" image and connect it to the my_network" network. Name the container "nginx_container*


faisa@FaisalBhatti MINGW64 ~

**$ docker images**
```
REPOSITORY                   TAG       IMAGE ID       CREATED       SIZE
faisalriazz/sample_fastapi   v1        f025f201355b   7 days ago    217MB
faisalriazz/sample_fastapi   latest    95a5cd02d5cb   8 days ago    216MB
nginx                        latest    89da1fb6dcb9   2 weeks ago   187MB
```
faisa@FaisalBhatti MINGW64 ~

**$ docker run -d -p 8080:80 --name nginx_container --net my_network nginx**

a8eb2150f106a85766a129943df98c26e3b03f07131834d834738e9e566e85f9

faisa@FaisalBhatti MINGW64 ~

**$ docker ps**
```
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                  NAMES
a8eb2150f106   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp   nginx_container
```
faisa@FaisalBhatti MINGW64 ~

$ **docker inspect --format '{{json .NetworkSettings}}' nginx_container**
```json
{
  "Bridge": "",
  "SandboxID": "45f0e28a6dc34ddb27e68030fc074b63bff6ceb10856315b5283229648fb1094",
  "HairpinMode": false,
  "LinkLocalIPv6Address": "",
  "LinkLocalIPv6PrefixLen": 0,
  "Ports": {
    "80/tcp": [
      {
        "HostIp": "0.0.0.0",
        "HostPort": "8080"
      }
    ]
  },
  "SandboxKey": "/var/run/docker/netns/45f0e28a6dc3",
  "SecondaryIPAddresses": null,
  "SecondaryIPv6Addresses": null,
  "EndpointID": "",
  "Gateway": "",
  "GlobalIPv6Address": "",
  "GlobalIPv6PrefixLen": 0,
  "IPAddress": "",
  "IPPrefixLen": 0,
  "IPv6Gateway": "",
  "MacAddress": "",
  "Networks": {
    "my_network": {
      "IPAMConfig": null,
      "Links": null,
      "Aliases": [
        "a8eb2150f106"
      ],
      "NetworkID": "245c7b04755a202c97605152084c17e3a6283dd5b5313765867a4975cfe11308",
      "EndpointID": "bcd28b1728e5609789d7395a0409b8c0bf669ce93f7d8037b84fc795a4560094",
      "Gateway": "172.18.0.1",
      "IPAddress": "172.18.0.2",
      "IPPrefixLen": 16,
      "IPv6Gateway": "",
      "GlobalIPv6Address": "",
      "GlobalIPv6PrefixLen": 0,
      "MacAddress": "02:42:ac:12:00:02",
      "DriverOpts": null
    }
  }
}
```
## 1.3. Verify that the "nginx" default page is accessible on your host machine at http://localhost:8080.

**curl http://localhost:8080**
```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0  53283      0 --:--:-- --:--:-- --:--:-- 55909
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
## Create a new Docker container using the "httpd" image and connect it to the "my_network" network. Name the container "httpd_container".

faisa@FaisalBhatti MINGW64 ~
**$ docker run -d -p 8081:80 --name httpd_container --net my_network httpd**
```
b822f9b56577f7761e9e05f8130236ae3fe8cfc5f92ae540a6644d03be3c869c

```
faisa@FaisalBhatti MINGW64 ~

**$ docker ps**
```
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                  NAMES
b822f9b56577   httpd     "httpd-foreground"       About a minute ago   Up About a minute   0.0.0.0:8081->80/tcp   httpd_container
a8eb2150f106   nginx     "/docker-entrypoint.…"   53 minutes ago       Up 53 minutes       0.0.0.0:8080->80/tcp   nginx_container
```
faisa@FaisalBhatti MINGW64 ~

**$ docker inspect --format '{{json .Containers}}' my_network**


```json
{
  "a8eb2150f106a85766a129943df98c26e3b03f07131834d834738e9e566e85f9": {
    "Name": "nginx_container",
    "EndpointID": "bcd28b1728e5609789d7395a0409b8c0bf669ce93f7d8037b84fc795a4560094",
    "MacAddress": "02:42:ac:12:00:02",
    "IPv4Address": "172.18.0.2/16",
    "IPv6Address": ""
  },
  "b822f9b56577f7761e9e05f8130236ae3fe8cfc5f92ae540a6644d03be3c869c": {
    "Name": "httpd_container",
    "EndpointID": "a5f356b71abb244ae121cb03278d6f98e743f28c3799c8b301b2ff274e0ab401",
    "MacAddress": "02:42:ac:12:00:03",
    "IPv4Address": "172.18.0.3/16",
    "IPv6Address": ""
  }
}
```

## 1.4. Verify that the "httpd" default page is accessible on your host machine at http://localhost:8081

faisa@FaisalBhatti MINGW64 ~

**$ curl http://localhost:8081**
```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    45  100    45    0     0   4085      0 --:--:-- --:--:-- --:--:--  4500
```
<html><body><h1>It works!</h1></body></html>


## 1.5. Use the "docker network inspect" command to display 

information about the "my_network" network.
  * My network Subnet: "172.18.0.0/16" and Gateway: "172.18.0.1"
  * There are two containers connected to my_network.
  * nginx_container and httpd_container.
  * nginx_container ip is "172.18.0.2/16.
  * httpd_container ip is "172.18.0.3/16".

## 1.6. Stop and remove the "nginx_container" container.
faisa@FaisalBhatti MINGW64 ~

**$ docker stop nginx_container**
```
nginx_container
```
faisa@FaisalBhatti MINGW64 ~

**$ docker ps -a**
```
CONTAINER ID   IMAGE                           COMMAND                  CREATED             STATUS                      PORTS                  NAMES
b822f9b56577   httpd                           "httpd-foreground"       25 minutes ago      Up 25 minutes               0.0.0.0:8081->80/tcp   httpd_container
a8eb2150f106   nginx                           "/docker-entrypoint.…"   About an hour ago   Exited (0) 11 seconds ago                          nginx_container
40083cbba23c   faisalriazz/sample_fastapi:v1   "/bin/sh -c 'python3…"   7 days ago          Exited (137) 7 days ago                            FastAPIServer
```
faisa@FaisalBhatti MINGW64 ~

**$ docker rm nginx_container**
```
nginx_container
```
faisa@FaisalBhatti MINGW64 ~

**$ docker ps -a**
```
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS                    PORTS                  NAMES
b822f9b56577   httpd                           "httpd-foreground"       25 minutes ago   Up 25 minutes             0.0.0.0:8081->80/tcp   httpd_container
40083cbba23c   faisalriazz/sample_fastapi:v1   "/bin/sh -c 'python3…"   7 days ago       Exited (137) 7 days ago                          FastAPIServer
```
## 1.7. Create a new Docker container using the "nginx" image and connect it to the "my_network" network. Name the container "nginx_container_2".

faisa@FaisalBhatti MINGW64 ~

**$ docker run -d -p 8082:80 --name nginx_container2 --net my_network nginx**
```45ebb86308848cbb3745902a0143c8510d523a67ce0408a47a506dbce884c0d0```

faisa@FaisalBhatti MINGW64 ~

**$ docker ps -a**

```
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS                    PORTS                  NAMES
45ebb8630884   nginx                           "/docker-entrypoint.…"   6 seconds ago    Up 5 seconds              0.0.0.0:8082->80/tcp   nginx_container2
b822f9b56577   httpd                           "httpd-foreground"       29 minutes ago   Up 29 minutes             0.0.0.0:8081->80/tcp   httpd_container
40083cbba23c   faisalriazz/sample_fastapi:v1   "/bin/sh -c 'python3…"   7 days ago       Exited (137) 7 days ago                          FastAPIServer
```
## 1.8. Verify that the "nginx" default page is accessible on your host machine at http://localhost:8082.
faisa@FaisalBhatti MINGW64 ~

$ **curl http://localhost:8082**
```
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0  94209      0 --:--:-- --:--:-- --:--:--  100k

```
 

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

## 1.9. Use the "docker container ls" command to display information about all running containers. Document your findings in the README.md file

faisa@FaisalBhatti MINGW64 ~

**$ docker container ls**
```
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
45ebb8630884   nginx     "/docker-entrypoint.…"   10 minutes ago   Up 10 minutes   0.0.0.0:8082->80/tcp   nginx_container2
b822f9b56577   httpd     "httpd-foreground"       39 minutes ago   Up 39 minutes   0.0.0.0:8081->80/tcp   httpd_container
```
  * two containers nginx_container2 and httpd_container are running at localhost port 8082 and 8081 respectively.
  * using images nginx and httpd respectively.

## 1.10. Stop and remove all containers. 
faisa@FaisalBhatti MINGW64 ~

**$ docker stop nginx_container2**

```nginx_container2```


faisa@FaisalBhatti MINGW64 ~
**$ docker stop httpd_container**
```httpd_container```


faisa@FaisalBhatti MINGW64 ~

$ **docker ps -a**
```
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS                      PORTS     NAMES
45ebb8630884   nginx                           "/docker-entrypoint.…"   16 minutes ago   Exited (0) 27 seconds ago             nginx_container2
b822f9b56577   httpd                           "httpd-foreground"       45 minutes ago   Exited (0) 12 seconds ago             httpd_container
40083cbba23c   faisalriazz/sample_fastapi:v1   "/bin/sh -c 'python3…"   7 days ago       Exited (137) 7 days ago               FastAPIServer

```

faisa@FaisalBhatti MINGW64 ~

$ **docker rm nginx_container2 httpd_container**
```
nginx_container2
httpd_container
```

faisa@FaisalBhatti MINGW64 ~

$ **docker container ls**
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## 1.11. Cleanup: Remove the "my_network" network.
faisa@FaisalBhatti MINGW64 ~

**$ docker network rm my_network**

```my_network```

faisa@FaisalBhatti MINGW64 ~

**$ docker network ls**
```
NETWORK ID     NAME      DRIVER    SCOPE
ceedada28e90   bridge    bridge    local
5002fbf7fc45   host      host      local
be106ca511ba   none      null      local
```







 


