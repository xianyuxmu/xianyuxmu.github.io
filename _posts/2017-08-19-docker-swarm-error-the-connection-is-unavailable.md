---
layout: post
title: Docker Swarm error response from daemon rpc error code = 14 desc = grpc the connection is unavailable
date: 2017-08-19 23:47:00 +0800
categories:
- Tech
tags:
- Docker

---

## Got the problem

I was following [Get Started, Part 4: Swarms](https://docs.docker.com/get-started/part4/) and got the problem the same as [Creating a Swarm cluster with Virtualbox nodes on OSX, the connection is unavailable](https://stackoverflow.com/questions/45643096/creating-a-swarm-cluster-with-virtualbox-nodes-on-osx-the-connection-is-unavail).

when I got this error👇, I didn't follow the docs...

```
docker@myvm1:~$ docker swarm init
Error response from daemon: could not choose an IP address to advertise since this system has multiple addresses on different interfaces (10.0.xx.xx on eth0 and 192.168.xx.xx on eth1) - specify one with --advertise-addr
```

I try it with `docker-machine ssh myvm1 "docker swarm init --listen-addr eth0:2377 --advertise-addr eth1"`, and then machine `myvm1` was initialized as `swarm manager`.

Next, I tried to add machine `myvm2` to swarm and failed with:

```
➜  docker-test git:(master) ✗ docker-machine ssh myvm2 "docker swarm join --token <token> <IP of myvm1>:2337"
Error response from daemon: rpc error: code = 14 desc = grpc: the connection is unavailable
```
So, I searched it with Google about two hours, and finally got no answer...

Sh*t, I retried it with [docs](https://docs.docker.com/get-started/part4/#create-a-cluster): 


1. re-init the swarm manager: 
 - take down the swarm with `docker swarm leave --force`
 - re-init with `docker swarm init --advertise-addr [ip of the machine, check it with `docker-machine ls`]:2377`(`2377` is [the port for swarm joins][2])

2. then add your the machine to the swarm with `docker-machine ssh myvm2 "docker swarm join --token <token> <ip>:<port>"`

### Then it worked!

----

How stupid I was! I should have follow the docs's solution first instead of try it myself.

  [2]: https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts

