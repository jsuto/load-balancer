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
Welcome to nginx running on 2ac7e3e496c8
Welcome to nginx running on 7bf62bb06722
Welcome to nginx running on 2ac7e3e496c8
Welcome to nginx running on 7bf62bb06722
Welcome to nginx running on 2ac7e3e496c8
Welcome to nginx running on 7bf62bb06722
Welcome to nginx running on 2ac7e3e496c8
Welcome to nginx running on 7bf62bb06722
Welcome to nginx running on 2ac7e3e496c8
Welcome to nginx running on 7bf62bb06722
```
