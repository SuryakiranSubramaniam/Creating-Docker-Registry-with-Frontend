# Creating-Docker-Registry-with-Frontend
Creating Docker Registry with Frontend

## Create a Hosted zone in aws Rout53

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/rout53.png)

## Copy name server values from there:

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/ns-aws.png)

## Go to hostinger and change the name server values.

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/ns-hostinger.png)

## Create an EC2 Instance AIM- (IP – 65.2.38.182)

![alt text]()

## Create an A record in rout 53

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/a-record.png)

## ssh into that EC2

```sudo amazon-linux-extras install epel -y
sudo yum install -y certbot python2-certbot-apache
certbot certonly -d site.suryakiran.online --manual --preferred-challenges dns
```

![alt text](https://github.com/SuryakiranSubramaniam/Creating-Docker-Registry-with-Frontend/blob/main/img/Screenshot%20from%202022-02-25%2023-58-22.png)

![](img/Screenshot from 2022-02-25 23-58-22.png)
