# Creating-Docker-Registry-with-Frontend
Creating Docker Registry with Frontend

## Create a Hosted zone in aws Route 53

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/rout53.png)

## Copy name server values from there:

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/ns-aws.png)

## Go to hostinger and change the name server values.

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/ns-hostinger.png)

## Create an EC2 Instance AIM- (IP – 65.2.38.182)

![alt text]()

## Create an A record in route 53

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/a-record.png)

## ssh into that EC2

```
sudo amazon-linux-extras install epel -y
sudo yum install -y certbot python2-certbot-apache
certbot certonly -d site.suryakiran.online --manual --preferred-challenges dns
```


> Please deploy a DNS TXT record under the name

> _acme-challenge.site.suryakiran.online with the following value:

> 2BCB4dO86qn2_mdwAAY8eFiIzMsgvsh97P5N9EjDtlk

> Before continuing, verify the record is deployed.

> - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

> Press Enter to Continue

**At this point go to Route 53 and add a TXT record:**

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/txt-record.png)

**After adding wait for some time and then press enter**

> IMPORTANT NOTES:

> Congratulations! Your certificate and chain have been saved at:

> /etc/letsencrypt/live/site.suryakiran.online/fullchain.pem

> Your key file has been saved at:

> /etc/letsencrypt/live/site.suryakiran.online/privkey.pem

> Your certificate will expire on 2022-05-26. To obtain a new or

> tweaked version of this certificate in the future, simply run

> certbot again. To non-interactively renew *all* of your

> certificates, run "certbot renew"

> If you like Certbot, please consider supporting our work by:

> Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate

> Donating to EFF:                    https://eff.org/donate-le
 
**If you see this then your certbot SSL certificate have been created sucessfully**

```
ll /etc/letsencrypt/live/site.suryakiran.online/
```
> total 4

> lrwxrwxrwx 1 root root  46 Feb 25 17:21 cert.pem -> ../../archive/site.suryakiran.online/cert1.pem

> lrwxrwxrwx 1 root root  47 Feb 25 17:21 chain.pem -> ../../archive/site.suryakiran.online/chain1.pem

> lrwxrwxrwx 1 root root  51 Feb 25 17:21 fullchain.pem -> ../../archive/site.suryakiran.online/fullchain1.pem

> lrwxrwxrwx 1 root root  49 Feb 25 17:21 privkey.pem -> ../../archive/site.suryakiran.online/privkey1.pem

> -rw-r--r-- 1 root root 692 Feb 25 17:21 README

```
~]# mkdir certs
~]# cp /etc/letsencrypt/live/site.suryakiran.online/privkey.pem /root/certs/server.key 
```

Copy contents of `/etc/letsencrypt/live/site.suryakiran.online/cert.pem` and `/etc/letsencrypt/live/site.suryakiran.online/chain.pem` to `/root/certs/server.crt`

```
[root@ip-172-31-37-31 certs]# pwd
/root/certs
[root@ip-172-31-37-31 certs]# ls
server.crt  server.key
```


```
# mkdir auth/
# cd auth/
# htpasswd -Bc registry.password username
```
## Docker installation

```
amazon-linux-extras install docker -y
systemctl enable docker
systemctl start docker
```

## Docker-compose installation

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
docker-compose version
```

## Create docker-compose.yml `vim docker-compose.yml` 

```
version: '3'
services:
  registry:

    image: registry:2
    ports:
    - "5000:5000"

    environment:

      - REGISTRY_AUTH=htpasswd
      - REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm
      - REGISTRY_AUTH_HTPASSWD_PATH=/auth/registry.password
      - REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/data
      - REGISTRY_HTTP_ADDR=0.0.0.0:5000
      - REGISTRY_HTTP_TLS_CERTIFICATE=/certs/server.crt
      - REGISTRY_HTTP_TLS_KEY=/certs/server.key

    volumes:
      - data:/data
      - ./certs:/certs
      - ./auth:/auth

    networks:
      - registry_net

  frontend:
    environment:
     - ENV_DOCKER_REGISTRY_HOST=registry
     - ENV_DOCKER_REGISTRY_PORT=5000
     - ENV_DOCKER_REGISTRY_USE_SSL=1
     - ENV_USE_SSL="yes"
    volumes:
     - ./certs/server.crt:/etc/apache2/server.crt:ro
     - ./certs/server.key:/etc/apache2/server.key:ro
    ports:
     -  443:443
    image: konradkleine/docker-registry-frontend:v2
    networks:
     - registry_net

volumes:
  data:
networks:
  registry_net:
```

## File structure

```
# tree
.
├── auth
│   └── registry.password
├── certs
│   ├── server.crt                         
│   └── server.key
└── docker-compose.yml

2 directories, 4 files
```

## run the compose file

```
docker-compose config              # To check the syntax

docker-compose up -d

 ~]# docker container ls
CONTAINER ID   IMAGE                                      COMMAND                  CREATED          STATUS          PORTS                                           NAMES
a09e0b79965d   registry:2                                 "/entrypoint.sh /etc…"   48 minutes ago   Up 48 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp       root_registry_1
525d3c1078bf   konradkleine/docker-registry-frontend:v2   "/bin/sh -c $START_S…"   48 minutes ago   Up 48 minutes   80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp   root_frontend_1
```


```
# docker pull ubuntu:16.04

#  docker tag ubuntu:16.04 site.suryakiran.online:5000/ubuntu:latest

#  docker login site.suryakiran.online:5000

#  docker push site.suryakiran.online:5000/ubuntu:latest
```

Now go to `https://site.suryakiran.online/repositories`

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/site.png)

username: username

password: surya
