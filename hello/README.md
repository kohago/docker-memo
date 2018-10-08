# run docker image as a daemon
```
docker image pull nginx
docker image list //hello-world
docker container run -d --name nginx-test -p 8080:80 nigix
docker ps
docker stop containerId

#error!image is being used by stopped container containerId
docker image rm imageId

docker container ls -a
docker container rm containerId
```
