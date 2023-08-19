# Setup

I have ngrok in front of everything, the balancer afterwards and 4 nodes behind it. 
Due to some issues on my side, all the containers are up in host network, and I'm not 
sure why, but in this setup ngrok is listening only on ipv6 interface.
Nevertheless, the rest of stuff works fine.
Also in my code there might be discrepancy between "UK" and "EU", since
ngrok does not support UK per se, but some names are still "UK".

## Balancer
Balancer is a custom docker container, now available on docker hub, that
includes two extra modules: upstream health checks and geoip.

## Nodes
All nodes are a regular nginx server returning the name of the "region" they are deployed in.

## Test Results
Let's simply access "US" server by connection via VPN:
![Accessing from the US region](https://i.imgur.com/3ETxmaQ.png)

Now let's check the "EU" region, by connecting from Netherlands:
![Accessing from the EU region](https://i.imgur.com/www8rIf.png)

Now let's try to access "US" region again, but before that we stop the only "US" node:

```
docker stop node-1
```

![Accessing from the US region, backup server](https://i.imgur.com/iWPwVkp.png)

So, we got the response from teh "Japan" server, which is the backup server for the US upstream.
