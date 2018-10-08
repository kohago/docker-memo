# to easily launch and bootstrap Docker hosts targeting various platform
```
//has to install virtualbox in advance
//Error with pre-create check: "VBoxManage not found. Make sure
VirtualBox is installed and VBoxManage is in the path"
//check->create virtulaMachine (Boot2Docker?)->
docker-machine create --driver virtualbox docker-local
docker-machine ls
docker-machine env docker-local
// export DOCKER_TLS_VERIFY="1"
// export DOCKER_HOST="tcp://x.x.x.x:2345"
// export DOCKER_CERT_PATH="xxx/.docker/machine/machines/docker-local"
// export DOCKER_MACHINE_NAME="docker-local"

eval $(docker-machine env docker-local)

docker-machine active

docker-machine ssh docker-local
```
