```
//validate docker-compose.yaml
docker-compose config

//pull all images found
docker-compose pull

//build images
docker-compose build

//build,pull command only dothings for images
//they don't config the container
//docker-compose will create the container.not launch
docker-compose create

docker-compose top
docker-compose logs -f

docker-compose up -d --scale xx=3

//stop the container immediately
docker-compose kill

//remove any containers with the state of exited
docker-compose rm

// remove all of the containers, networks,by default
// -rmi option will delete images
docker-compose down
docker-compose down --rmi=all

```
