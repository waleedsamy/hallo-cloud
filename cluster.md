* This is coreos cluster built with vagrant form `coreos-vagrant`

node1 :
 ```bash
 # run registrator
 docker run -d \
   --name=registrator \
   -h $HOSTNAME \
   --net=host \
   --volume=/var/run/docker.sock:/tmp/docker.sock \
   gliderlabs/registrator:latest \
   -ip $COREOS_PUBLIC_IPV4 \
   etcd://$COREOS_PRIVATE_IPV4:2379/traffics.de


 docker run -d -e "ETCD_NODE=$COREOS_PRIVATE_IPV4:2379" -p $COREOS_PUBLIC_IPV4:80:80 waleedsamy/nginx-confd-consul-docker:etcd
 ```

node2, node3, node4 :
 ```bash
 # run registrator
 docker run -d \
   --name=registrator \
   -h $HOSTNAME \
   --net=host \
   --volume=/var/run/docker.sock:/tmp/docker.sock \
   gliderlabs/registrator:latest \
   -ip $COREOS_PUBLIC_IPV4 \
   etcd://$COREOS_PRIVATE_IPV4:2379/traffics.de

 # list all etcd keys
 # etcdctl ls /traffics.de


 # run 3 container of our application
 docker run -d -P --hostname api waleedsamy/hello-world-expressjs-docker
 docker run -d -P --hostname api waleedsamy/hello-world-expressjs-docker
 docker run -d -P --hostname api waleedsamy/hello-world-expressjs-docker
 ```
