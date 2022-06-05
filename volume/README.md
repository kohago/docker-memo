```
//when start container, a volume will be created and mounted to the path definded in image's Dockerfile

docker volume ls

docker volume create test_volume
docker container run -d --name xxx -v test_volume:/data
docker volume inspect test_volume

docker volume rm some-volume
 ```


###create volume when upping container 
Creating volume "pj-name_volume-name" with local driver
Creating container ... done
Attaching to container
 ```
