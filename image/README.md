# build a image
```
docker image build file Dockerfile --tag local/dockerfile-example:0.1
#or
docker image build --tag local/dockerfile-example:0.1 .

docker image ls
docker image tag imageId local/dockerfile-example:0.2
docker image inspect imageId
```

#run it -> login -> change ->save change
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

#login to dockerhub and push the image1
```
docker login
docker image push xx/xx:latest
```
