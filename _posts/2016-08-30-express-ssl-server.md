---
layout: post
title:  "set up a SSL server with express and letsencrypt"
date:   2016-08-30 19:20:00
categories: Dev-Misc
tags: express, ssl
#image:
published: true
---

This is a quick and dirty way to set up SSL server for development. 
I'm not a security expert, and will not be held responsible for any damages it may causes. Please let me know
if any of these poses security risks. Thanks

#Request SSL Certs from Let's Encrypt

(Long tutorials)[https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-16-04]

###Install Let's Encrypt Client

```bash
sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
cd /opt/letsencrypt
./letsencrypt-auto --apache -d example.com -d www.example.com
```

The `.pem` files will be stored `/etc/letsencrypt/live/example.com`

###Auto Renew The Cert
The certs are valid upto 90 days. Set up crontab for root user:

```bash
sudo crontab -e
```

Specify any random time, for example Every Monday at 2:30am:
```crontab
30 2 * * 1 /opt/letsencrypt/letsencrypt-auto renew >> /var/log/le-renew.log
```

#Setup SSL with Express

source: <https://github.com/lancevo/express_ssl_server>  

This setup include CORS, helmet plug-ins. As for the HTTP and HTTPS, don't use 80 and 443, so it doesn't need `sudo` to
run the code, and we will re-route the ports with iptables later.

```javascript
var express = require('express');
var helmet = require('helmet'); //https://expressjs.com/en/advanced/best-practice-security.html
var cors = require('cors'); // https://github.com/expressjs/cors
var app = express();
var https = require('https');
var http = require('http');
var fs = require('fs');

var HTTP_PORT = 7777, HTTPS_PORT = 4443;

var key = '/etc/letsencrypt/live/example.com/privkey.pem';
var cert = '/etc/letsencrypt/live/example.com/fullchain.pem'; 

var sslOptions = {
    key: fs.readFileSync(key),
    cert: fs.readFileSync(cert)
}

var whitelist = ['http://localhost:8000','https://www.mywebsite.com'];
var corsOptions = {
    origin: function(origin, callback){
        var originIsWhitelisted = whitelist.indexOf(origin) !== -1;
        callback(originIsWhitelisted ? null : 'Bad Request', originIsWhitelisted);
    },
    optionsSuccessStatus: 200
}

app.use(helmet());
app.use(cors(corsOptions));

app.get('/', function(req,res){
    var ip = req.headers['x-forwarded-for'] || req.connection.remoteAddress;
    res.send('Hello ',ip);
  
});

console.log('Running HTTP', HTTP_PORT, 'HTTPS', HTTPS_PORT)
http.createServer(app).listen(HTTP_PORT);
https.createServer(sslOptions,app).listen(HTTPS_PORT);
```


#Re-route iptables to your server

<http://askubuntu.com/questions/427600/persist-port-routing-from-80-to-8080>

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to 7777
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to 4443
sudo iptables-save
```

In case you need to delete a PREROUTING, for longer instruction <http://www.cyberciti.biz/faq/how-to-iptables-delete-postrouting-rule/>

List the ROUTING with LINE NUMBER
```bash
sudo iptables -t nat -v -L -n --line-number
```

Delete a PREROUTING
```bash
sudo iptables -t nat -v -L PREROUTING -n --line-number {LINE-NUMBER}
```










