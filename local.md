work with confd in same machine
```
# run consul
docker run --name consul -p 8400:8400 -p 8500:8500 -p 8600:53/udp -h node1 progrium/consul -server -bootstrap

# run registrator
# use consul ip from consul container output, in may case it was
# CONSUL_IP=172.17.0.2
docker run -d --name=registrator --volume=/var/run/docker.sock:/tmp/docker.sock gliderlabs/registrator:latest consul://$CONSUL_IP:8500

# run api, may have more than one to see confd magic happen
# don't set container name or map host ip your self
docker run -d -P --hostname api waleedsamy/hello-world-expressjs-docker


# run nginx (LB) with confd
# HOST_IP=0.0.0.0
docker run -d --link="consul:consul" -e "CONSUL_NODE=consul:8500" -p $HOST_IP:80:80 waleedsamy/nginx-confd-consul-docker

```
