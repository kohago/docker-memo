```
//by default,all containers are running on a flat networm,
//means that they can access each another
//create a new network and let containers to run in the network,
//then they can only access others containers in that network

docker network create test-network
docker container run -d --name xxx --network test-network xxxx

//the hostname of the local container is in /etc/hosts,that is bridge??
docker container exec xxxx cat /etc/hosts

//using a local nameserver
/etc/resolv.conf
--> nameserver 127.0.0.11

docker container exec container1 nslookup container2 127.0.0.11


docker network ls
docker network inspect test-network
docker network prune
```
