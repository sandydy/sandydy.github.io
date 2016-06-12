---
layout: post
title: New Experience - Docker on Windows
date: 2016-06-08 10:42:39.000000000 +00:00
tags       : 
  - jekyll
  - docker
comments   : true
signature  : true
---
If we want to leverage linux features but at the same time we are on windows. In the past, we'll need to either work on a linux machine or work on a virtual machine. This articles describes an alternative for you.

We are going to use [Docker Toolbox](https://www.docker.com/products/docker-toolbox) version 1.11.1 on Windows 7. Please refer to [tutorials](https://www.docker.com/products/docker-toolbox#/tutorials) for beginners. If you already have experience on docker, do take a look at this [article](http://blog.pavelsklenar.com/5-useful-docker-tip-and-tricks-on-windows/) by [Pavel Sklenar](http://blog.pavelsklenar.com/about/) for the tips to use docker on windows. 

I was originally looking at GitHub/GitLab Page to host my blog. I then spotted [Jekyll](https://jekyllrb.com/). I was later looking at [Jekyll on windows](https://jekyllrb.com/docs/windows/). My head was spinning when I read the instructions. Why not give a try to docker?

First of all, check if jekyll already has a docker image from community via [docker hub](https://hub.docker.com) Yes, most common linux software already has docker image created. 

![Docker Hub Jekyll](/public/images/2016/06/docker_hub.jpg)

The [`Jekyll/Jekyll`](https://github.com/jekyll/docker/blob/master/README.md) looks promising, let's give a try. Typically the author provides instruction how to use the image. But no luck this time. 

Let's try

`docker run jekyll/jekyll`, see what will happen

![Jekyll first run](/public/images/2016/06/jekyll_image.jpg)

Aha, I see how it works now. I tweaked my `docker run` command a bit and make it works. Before I re-run the docker image...

I didn't want to start from scratch, instead, I start with [Jekyll Bootstrap](http://jekyllbootstrap.com/usage/jekyll-quick-start.html) which looks promising.

`git clone https://github.com/plusjade/jekyll-bootstrap.git tryJekyll`

I am getting Jekyll to my local filesystem via virtual box shared folder which mounted at `$(pwd)`

![VirtualBox shared folder mount](/public/images/2016/06/data_mount.jpg)

`docker run -v $(pwd):/srv/jekyll -p 4000:4000 --name=tryJekyll jekyll/jekyll`

`jekyll serve` automatically executed from the newly create docker image

![jekyll serve in progress](/public/images/2016/06/jekyll_serve-1.jpg)

Good, I don't need to care about how to get jekyll. I don't even need to `apt-get`

I am just dumb, please hide all these things for me. Thanks Docker, Thanks Jekyll. Jekyll is up at port 4000. If you are new to docker, you can execute `docker-machine ip` to get the docker machine ip from host. Since I've used `-p 4000:4000` switch, I can navigate `http://<docker host ip>:4000/` to check the jekyll bootstrap created page.

![Jekyll Bootstrap initialized](/public/images/2016/06/jekyll_bootstrap_up.png)

it seems that CSS is missing. I surfed from the issue list and found [issue 302](https://github.com/plusjade/jekyll-bootstrap/issues/302)

The css path is incorrect, I was then create the folder (in windows) and style comes back now. 

![Jekyll bootstrap with style](/public/images/2016/06/jekyll_bootstrap_up_with_style.png)

what I need to do is ...

Once I change the folder structure, instantly Jekyll detects my change and re-generated the _site as I can see from console...

![Jekyll regenerate](/public/images/2016/06/regenerate.png)

Thanks Jekyll docker image, it just works well :-)

If in case filesytem watcher does not exist, you can re-create the whole thing without bothering to re-configure any tools.

1. `Ctrl+C` to the Jekyll execution console
2. `docker rm -f tryJekyll`
3. `docker run -v $(pwd):/srv/jekyll -p 4000:4000 --name=tryJekyll jekyll/jekyll`

This is obviously not the smartest way in the planet as the docker image re-load all dependencies again. But it will work. :-)

Alternatively, you can stop and start the docker container
1. `Ctrl+C` to the Jekyll execution console
2. `docker start -a tryJekyll`

As you can see, docker works like cygwin (if you are on my age, you probably have heard of this). Docker image could save you a lot of time on software or tools configuration. The data are persisted either in container or host filesystem or the docker image container can be restart from scratch. Docker is flexible and enables hybrid mode working. I don't bother to remember all the commands in VIM now. I can now continue to develop my blog page locally and once ready commit it to github :-)

**Updates**
The way docker delivers software has been widely adopted and will be popular in coming years. Though Docker for Toolbox relies on virtual box which is a bit complicated, next generation you will see a lot of support from windows. 

For windows 10 (Professional) users, there are several options 

1. Native Docker support for windows, it is now on private [beta]((Beta for docker). Don't expect too much yet, it is just a replacement of virtual box to hyper-v however, you don't have access to host VM anymore (at least for the moment the article is written)
2. [Container as Windows Feature](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/about/about_overview), the feature is available as insider build and will be included in windows server 2016
3. [Bash on ubuntu on windows](https://blogs.msdn.microsoft.com/commandline/2016/04/06/bash-on-ubuntu-on-windows-download-now-3/) Another windows insider feature, but you should expect `apt-get docker-engine` available on the powershell soon
