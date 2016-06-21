---
published: true
layout: post
date: {}
tags:
  - network
  - docker
comments: true
signature: true
---
## Docker network host
Many docker users are familar with `-p` switch in `docker run`. There is actually a network mode named `host`. 

[Jon Langemak](http://www.dasblinkenlichten.com/test/) has a good [article](http://www.dasblinkenlichten.com/docker-networking-101-host-mode/) on this topic specifically on docker network `host` behavior. Here I try to further explain the ideas behind.

The network mode `host` enables docker container to access ethernet interfaces directly from inside docker container. (networking throughtput should perform better) If there are conflicts, actually the FIFO rules applies. 

for instance, 
**n1**<-- port 80 httpd
**n2**<-- port 80 httpd

both of which runs with
`docker run -itd --name=n1 --net=host nginx`
`docker run -itd --name=n2 --net=host nginx`

firstly n1 will listen to port 80, n2 listens will fail (you won't know unless you check n2 nginx logs explicitly, it will show cannot bind to port 80)
![bind_failure.jpg](/public/images/2016/06/13/docker_network_host/bind_failure.jpg)

however, if we modify the command
`docker run -itd --name=n1 --net=host --restart=always nginx`
`docker run -itd --name=n2 --net=host --restart=always nginx`

after docker host is restarted, you won't know n1 or n2 will bind port 80. Docker itself has a container dependencies, however, n1 and n2 are not in linked, volume mount or [dependson](https://docs.docker.com/compose/compose-file/#depends-on) relationship. the restart sequence is simply FIFO. and as a result n1 always takes precedence.

In another scenario, if we have
`docker run -itd --name=n1 nginx`
`docker run -itd --name=g1 ghost`

both container starts without problem as n1 listens port 80 and g1 listens port 2368. That means g1 and n1 are working as 2 different applications and independently binds to 80 and 2368.

In that case, network mode "host" work for both g1 and n1 at the same time. Network mode `host` does not mandate only one container to allow host mode. Instead, it leaves to docker host administrator to assign which containers to have host ethernet access.

in the [Jon Langemak](http://www.dasblinkenlichten.com/test/)'s article, the other possible way is to bind different host ethernet. That will also work as the listen will succeed, same as we've done without docker.

in conclusion, network mode **host** enables container to operate host ethernet address same as problem on docker host. However, conflicts might occur. by design, docker host administrator should avoid the conflicts situation.
