- build a image
```
docker image build file Dockerfile --tag local/dockerfile-example:0.1
#or
docker image build --tag local/dockerfile-example:0.1 .

docker image ls
docker image tag imageId local/dockerfile-example:0.2
docker image inspect imageId
```
- entrypoint
```
When you set and entry point in a docker container.
It is the only thing it will run. 
It's the one and only process that matters (PID 1). 
Once your entry_point.sh script finishes running and returns and exit code, 
docker thinks the container has done what it needed to do and exits, since the only process inside it exits.
```
- run it -> login -> change ->save change
```
docker container run --it --name test-image1 imageId /bin/sh

apk update
apk upgrade

//save it at another terminal
//sha256:xxxx
docker container commit test-image1 local:after-edited

//exit and shutdown the container
exit

//image taged [after-edited] was created!
docker image ls

//save image file to tar files
docker image save -o after-edited.tar local:after-edited

```

- login to dockerhub and push the image1
```
docker login
docker image push xx/xx:latest
```
