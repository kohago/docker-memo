```
//when start container, a volume will be created and mounted to the path definded in image's Dockerfile

docker volume ls

docker volume create test_volume
docker container run -d --name xxx -v test_volume:/data
docker volume inspect test_volume
 ```
