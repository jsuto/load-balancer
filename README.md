# Setup a load balancer with 2 nginx backends

## Start the containers (and show their logs)

```
$ docker-compose up
Starting nginx2 ... done
Starting nginx1 ... done
Starting traefik ... done
Attaching to nginx2, nginx1, traefik
nginx1     | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
nginx1     | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
nginx1     | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
nginx2     | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
nginx2     | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
nginx1     | 10-listen-on-ipv6-by-default.sh: info: can not modify /etc/nginx/conf.d/default.conf (read-only file system?)
nginx1     | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
nginx2     | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
nginx2     | 10-listen-on-ipv6-by-default.sh: info: can not modify /etc/nginx/conf.d/default.conf (read-only file system?)
nginx1     | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
nginx1     | /docker-entrypoint.sh: Configuration complete; ready for start up
nginx2     | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
nginx2     | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
nginx2     | /docker-entrypoint.sh: Configuration complete; ready for start up
traefik    | time="2021-05-19T14:49:48Z" level=info msg="Configuration loaded from file: /traefik.yaml"
nginx1     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx2     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx1     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx2     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx1     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx2     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx1     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx2     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx1     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
nginx2     | 172.21.0.4 - - [19/May/2021:14:49:50 +0000] "GET / HTTP/1.1" 200 42 "-" "curl/7.68.0" "172.21.0.1"
```

## Test the loadbalancer

```
$ for i in {1..10}; do curl localhost; done
8d243346e20d
7747589531ce
8d243346e20d
7747589531ce
8d243346e20d
7747589531ce
8d243346e20d
7747589531ce
8d243346e20d
7747589531ce
```

## How not to run as root

Use the user remapping feature of docker

/etc/docker/daemon.json:

```
{
   "userns-remap": "default"
}

```
/etc/subuid:

```
dockremap:100000:65536
```

/etc/subgid:

```
dockremap:100000:65536
```

Even though some processes run as root inside the container,
they are non-privileged processes on the host:

```
$ ps uaxw|grep -E 'nginx|traefik'|grep -v grep
100000      8341  0.1  0.1  10648  6176 ?        Ss   12:04   0:00 nginx: master process nginx -g daemon off;
100000      8342  0.1  0.1  10648  6128 ?        Ss   12:04   0:00 nginx: master process nginx -g daemon off;
100101      8460  0.0  0.0  11036  2636 ?        S    12:04   0:00 nginx: worker process
100101      8493  0.0  0.0  11036  2572 ?        S    12:04   0:00 nginx: worker process
100000      8539  2.3  1.3 788052 52728 ?        Ssl  12:04   0:00 traefik traefik
```
