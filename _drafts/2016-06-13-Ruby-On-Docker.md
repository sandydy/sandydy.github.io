---
published: false
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

![Ruby Image Entrypoint]({{site.baseurl}}/public/2016/06/13/ruby_on_demand/ruby_interactive.jpg)

`ird` is interactive ruby. The entrypoint is correct and the docker image is correct. What is missing? By default, docker image runs in daemon mode. However, if we expect inputs, we shall switch to interactive mode with switch (-i) and instructs docker to allocate a presudo-TTY to the container stdin with switch (-t). As a result, the command should be 

`docker run -it ruby`
![Ruby irb]({{site.baseurl}}/public/2016/06/13/ruby_on_demand/ruby_irb.jpg)

Here we can execute anything we'd like. We might curious to know why do we exit irb and the docker container exits automatically. Let's run the ruby image with `bash`. 
*Note: for newbies, docker allows overriding entrypoint at execution. That is we can run `bash` instead of ruby docker image pre-defined `irb` as below*

`docker run -it ruby bash`
![Entrypoint command pid]({{site.baseurl}}/public/2013/06/13/ruby_on_demand/ruby_pid.jpg)

As you can see, `bash` is executing with PID=1. That means if `bash` finishes, the linux machine shutdowns. That's why in docker container when entrypoint command exits, the docker container exits. At the same screenshot, we can find that docker image in fact is not a complete function OS, it is a trimmed version only to execute specific process. 

The other area to keep an eye on. Docker on ARM for low power device e.g. IoT, you can see people are [porting docker to raspberry pi](http://blog.hypriot.com/post/port_dockerfiles_to_arm/)) The idea would make sense when it comes to router and network device. Many vendors (e.g. synology, mi, cisco) are building apps on their routers. If apps are delivered via docker...)

# Versions


# Attach and Detach

