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

# Attach and Detach

