```
docker image pull nginx
docker container run -d --name nginx-test -p 9000:80 nginx

//-----interactive exec-----
docker container exec -i -t nginx-test /bin/bash

//------logs------
//attach to see log,like tail -f
docker container attach nginx-test
//attach get the process handler
//control +c stop attach will kill the docker container process.  restart it
docker container start nginx-test
//Proxy all received signals to the process
docker container attach --sig-proxy=false nginx-test
docker container logs --tail 5 nginx-test
//-f --follow ,the same to tail -f
docker container logs -f nginx-test
//since the time recorded by Docker and not the time within the container
docker container logs --since 2017-06-24T15:00 nginx-test

//------top----
//list processes running within the container
docker container top nginx-test

//----stats---
//show the realtime information of the container
docker container stats nginx-test

//----limits----
//by default,the container can use all the available resources on the host
docker container run -d --name nigix-test --cpu-shares 512 --memory 128M -p 9000:80 nigix
//when update,used memory-swap should bigger than memory want to set
docker container update --cpu-shares 512 --memory 128M nginx-test

//---pause,kill(SIGKILL),stop(SIGTERM),start,restart---
//suspened the container process using the cgroups freezer,the process is unaware it has been suspended,
so it can be resumed
docker container pause nginx-test
docker container unpause nginx-test

//---prune,rm----
//rm the exited container
docker container prune

//create,just create ,not start or run
docker container create --name nigix-test -p 9000:80 nginx
//the status is created
docker container ls -a  
docker container port nigix-test
//show the diffrences from started
docker container diff nigix-test



```
