node1 :
 ```bash
  HOST_IP=172.17.8.101
  $(docker run --rm progrium/consul cmd:run 172.17.8.101 -d)
  # curl $HOST_IP:8500/v1/catalog/nodes
  # curl $HOST_IP:8500/v1/catalog/services
 ```

node2 :
 ```bash
  HOST_IP=172.17.8.102
  $(docker run --rm progrium/consul cmd:run 172.17.8.102:172.17.8.101 -d)
  # curl $HOST_IP:8500/v1/catalog/nodes
  # curl $HOST_IP:8500/v1/catalog/services
 ```

node3:
 ```bash
  HOST_IP=172.17.8.103
  $(docker run --rm progrium/consul cmd:run 172.17.8.103:172.17.8.101 -d)
  # curl $HOST_IP:8500/v1/catalog/nodes
  # curl $HOST_IP:8500/v1/catalog/services
 ```

node4:
 ```bash
  HOST_IP=172.17.8.104
  $(docker run --rm progrium/consul cmd:run 172.17.8.104:172.17.8.101:client -d)
  # curl $HOST_IP:8500/v1/catalog/nodes
  # curl $HOST_IP:8500/v1/catalog/services

  # run registrator
  docker run -d \
    --name=registrator \
    --volume=/var/run/docker.sock:/tmp/docker.sock \
    gliderlabs/registrator:latest \
    consulkv://$HOST_IP:8500/prefix

  # run 3 container of our application
  docker run -d -P --hostname api waleedsamy/hello-world-expressjs-docker
  docker run -d -P --hostname api waleedsamy/hello-world-expressjs-docker
  docker run -d -P --hostname api waleedsamy/hello-world-expressjs-docker
 ```

node5:
 ```bash
  HOST_IP=172.17.8.105
  $(docker run --rm progrium/consul cmd:run 172.17.8.105:172.17.8.101:client -d)
  # curl $HOST_IP:8500/v1/catalog/nodes
  # curl $HOST_IP:8500/v1/catalog/services

  docker run -d --link="consul:consul" -e "CONSUL_NODE=consul:8500" -p $HOST_IP:80:80 waleedsamy/nginx-confd-consul-docker

 ```

node6:
 ```bash
  # IP 172.17.8.106
 ```
