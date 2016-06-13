---
published: true
layout: post
date: {}
tags:
  - ruby
  - docker
comments: true
signature: true
---
Last [blog post](http://blog.sandydy.com/2016/06/08/new-experience-docker-on-windows/), we have shown how to host a jekyll web server and update the web page on hosted machine (e.g. Mac or Windows). This article illustrates how to fetch different versions of ruby docker images. 

# Problem statement
As a developer, I've encountered the following scenarios many times. 

- I need Ruby version 2.1, I don't have ruby at all. Installation requires a list of prerequisites
- I need J2EE version 1.6, but I already upgraded to version 1.7 and there are dependencies and JAVA_HOME...
- I found a open source tool which relies on python 1.x, I only run it once. Can I skipp the installation? 

Docker on desktop could be an answer. Docker image is a standardised runtime images with different pre-installed packages. Docker container is more or less the same across local machine, and on the cloud. As a result, the nice abstraction offers painless solution to the problem statements above.

# Ruby image on the glance
What I want was to migrate a blogger post to jekyll post. jekyll provides a [tool](http://import.jekyllrb.com/docs/blogger/) in ruby to convert the post. I don't have ruby installed on my desktop. 

In the old days... I'll need to get ruby and install on my windows with all pre-conditions matched...

With docker, I can simply execute 
`docker run ruby`

Ruby image is fetched from docker hub, and I am ready to go... Hm... we missed some details.

# Entrypoint
If we execute `docker run ruby` straight, we are not able to get what we want. If you  look at the docker process `docker ps -a`, you will find that ruby image Exited(0). The reason is because all docker image comes with an entrypoint. An entrypoint is a default command to execute when the image is run. 

![Ruby Image Entrypoint]({{site.url}}/public/images/2016/06/13/ruby_on_demand/ruby_interactive.jpg)

`ird` is interactive ruby. The entrypoint is correct and the docker image is correct. What is missing? By default, docker image runs in daemon mode. However, if we expect inputs, we shall switch to interactive mode with switch (-i) and instructs docker to allocate a presudo-TTY to the container stdin with switch (-t). As a result, the command should be 

`docker run -it ruby`
![Ruby irb]({{site.url}}/public/images/2016/06/13/ruby_on_demand/ruby_irb.jpg)

Here we can execute anything we'd like. We might curious to know why do we exit irb and the docker container exits automatically. Let's run the ruby image with `bash`. 
*Note: for newbies, docker allows overriding entrypoint at execution. That is we can run `bash` instead of ruby docker image pre-defined `irb` as below*

`docker run -it ruby bash`
![Entrypoint command pid]({{site.url}}/public/images/2016/06/13/ruby_on_demand/ruby_pid.jpg)

As you can see, `bash` is executing with PID=1. That means if `bash` finishes, the linux machine shutdowns. That's why in docker container when entrypoint command exits, the docker container exits. At the same screenshot, we can find that docker image in fact is not a complete function OS, it is a trimmed version only to execute specific process. 

The other area to keep an eye on. Docker on ARM for low power device e.g. IoT, you can see people are [porting docker to raspberry pi](http://blog.hypriot.com/post/port_dockerfiles_to_arm/)) The idea would make sense when it comes to router and network device. Many vendors (e.g. synology, mi, cisco) are building apps on their routers. If apps are delivered via docker...)

# Volume
After we understand how to execute the ruby image. Next is to think of storage. Docker offers flexiblity *volume mount* mechanisms. There are volume driver to mount variuos fs including local fs ext4, to cloud storage AWS EBS, etc.

The simpliest and desktop friendly volume mount must be host mount. That means, docker container mounts to a host directory. As a result, changes can be made both side. (right, the mount offers read/write access). The docker run switch is 
`-v [hosted path]:[container path]`

Our command becomes
`docker run -v /home/docker/rubycommand:/rubycommand -it ruby`

I am lazy and I've prepared a sh script file to run what I want... My command becomes
`docker run -v /home/docker/rubycommand:/rubycommand -it ruby /rubycommand/execute.sh`

    `#!/bin/bash
    gem install jekyll-import
    ruby -rubygems -e 'require "jekyll-import";
    JekyllImport::Importers::Blogger.run({
    "source"                = "blog-06-12-2016.xml",
    "no-blogger-info"       = false, # not to leave blogger-URL info (id and old URL) in the front matter
    "replace-internal-link" = false, # replace internal links using the post_url liquid tag.
    })'`

And the ruby program is executed correctly, **Mission Accomplished**. 

# Versions
While we've run ruby docker image, we can first navigate to the [docker image page](https://hub.docker.com/_/ruby/). If we have specific ruby application only works on specific version, we could actually specify the version on docker run command.

`docker run -it ruby:2.3.1`

The above command runs ruby 2.3.1. The available version can be found at docker image page on docker hub. 

![Ruby docker image version]({{site.url}}/public/images/2016/06/13/ruby_on_demand/ruby_version.jpg)

As you can see, various version on different platforms are available. You could just choose the version you'd like to execute the command. *(I regard this feature particularly useful for J2EE as compiler and as runtime.)*

# Attach and Detach
Sometimes we'd want to start a container, but at the same time yet to connect to the bash or sh. Docker allows such feature as attach and detach which allows host console to attach to the docker in/out.

By default, `docker run` attach to the docker container once the initialization is done. But user can specify docker container to run in detached via switch (-d).

`docker run -itd --name=r1 ruby:2.3.1`

The above command starts a docker ruby version 2.3.1 image in a container named *r1* in detached mode. 

![ruby run detach]({{site.url}}/public/images/2016/06/13/ruby_on_demand/ruby_detach.jpg)

If we want to attached to the ruby image `ird`, executes 
`docker attach r1`
![ruby attach]({{site.url}}/public/images/2016/06/13/ruby_on_demand/ruby_attach.jpg)

In an attached machine, `ctrl+c` won't allow you to detach. It terminates the program. `ctrl+p,ctrl+q` will do the work for you. 

Detach, attach mode enables you to work on one terminal and switch between. The command is handy and easy to be used.
