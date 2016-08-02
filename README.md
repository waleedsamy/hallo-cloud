# hallo-cloud
Play with consul, etcd, confd. check [cluster.md](https://github.com/waleedsamy/hallo-consul/blob/master/cluster.md) for more details about infrastructure used to deliver this cluster.

* every time your run api container it will be automatically added to nginx upstream servers, using [etcd](https://github.com/coreos/etcd), [registrator](http://gliderlabs.com/registrator/latest/) and [confd](http://www.confd.io/).
