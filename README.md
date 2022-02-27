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

**At this point go to Rout 53 and add a TXT record:**

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




