---
layout: post
title: Meteor + Atom at a first glance
date: 2016-06-06 11:08:30.000000000 +00:00
tags       : 
  - meteor
  - atom
  - javascript
  - web application development
  - javascript framework
  - HTML Editor
  - docker
comments   : true
signature  : true
---
This article is about configuration of IDE [Atom](https://atom.io/) and the web framework [Meteor](https://www.meteor.com). I've been working on web many years, but still new to both [Meteor](https://www.meteor.com) and [Atom](https://atom.io/). I used to use [Yeoman](http://yeoman.io/) since they started. I thought [Meteor](https://www.meteor.com) is similar...

I started looking at the [Meteor tutorials](https://www.meteor.com/tutorials/). At the first point, I realized [Meteor](https://www.meteor.com) is different from [Yeoman](http://yeoman.io/). [Yeoman](http://yeoman.io/) is just a templating library. It asks a list of questions and come up with a ready to develop repository. [Meteor](https://www.meteor.com) is an end-to-end tool which enables

1. Different UI framework [AngularJS](https://angularjs.org/), [ReactJS](https://facebook.github.io/react/), etc.
2. Different test framework Mocha, Jasmine, etc.
3. Different target platforms supported (e.g. Android, iOS, web)

"What?". I used to spend a some efforts to integrate grunt, angularjs, uglifyjs, phantomjs, jslint, configuration settings etc. [Meteor](https://www.meteor.com) has done everything for us. Let's leave the in-depth topics to later blogs. Here let's prepare the environment.

[Meteor](https://www.meteor.com) suggests [WebStorm](http://www.meteorpedia.com/read/Category:Development_Environments#Webstorm). I do agree [WebStorm](https://www.jetbrains.com/webstorm/) is the best IDE for single page application. However, the license is about $5.9 per month... Unless you are teachers, students, active open source project users.

Sublime is another popular IDE for single page applications. However, only text editing (no debugging) to [Meteor](https://www.meteor.com) available.

#Meteor Installation
![Meteor official site](/public/images/2016/06/meteor_install.jpg)

After installation, meteor was not set to the windows environment variable %PATH%. I searched the executable a while and end up found at 

`%LOCALADDDATA%\.meteor\meteor.bat`

I further drill into the batch file and eventually found the [nodejs](https://nodejs.org) executable as expected. 

*%LOCALADDDATA%\.meteor\packages\meteor-tool\1.3.2_4\mt-os.windows.x86_32\meteor.bat*

`...
"%~dp0\dev_bundle\bin\node.exe" "%~dp0\tools\index.js" %*
...`

and there is a list of nodejs module installed at 
`%LOCALADDDATA%\.meteor\packages\`

![Subset of nodejs module meteor](/public/images/2016/06/meteor_nodejs_module.jpg)

Obviously, the modules are downloaded via NPM. (to be studied)

At the end, meteor can be run in any path.

`meteor --version`

![Meteor version](/public/images/2016/06/meteor_version.jpg)

[Meteor](https://www.meteor.com) commands are like [docker](https://docker.io). To create a project, ...

`meteor create <project name>`

To run a [Meteor](https://www.meteor.com) application.

`meteor run [target...] [options]`

There are list of other commands ...

![Meteor Commands](/public/images/2016/06/meteor_command_2.jpg)

Up to this point, if you've ever used [Grunt](http://gruntjs.com) or [Gulp](http://gulpjs.com/). Do you think either task runner is needed?

Tutorials can be found at 

`meteor create --list`

![Meteor examples](/public/images/2016/06/meteor_example_list.jpg)

Both [AngularJS](https://angularjs.org/) and [ReactJS](https://facebook.github.io/react/) example can be found in the list.

`meteor create --example clock`

*Meteor returns a git command suggesting to run git directly. Just follow the instruction...*

In the clock directory, execute

`meteor run`

[Meteor](https://www.meteor.com) downloads necessary package, builds the solution via isobuild and run the application locally. By default, web page is located at 

`http://localhost:3000`

![Meteor started](/public/images/2016/06/meteor_started.jpg)

Navigate to the web page
![Example Clock](/public/images/2016/06/meteor_client.jpg)

Client side debugging is immediately available via developer tools [F12]. By design, any change to the source code in clock\client triggers client to restart to reflect the changes. For instance, I've added a header.

![Insert header](/public/images/2016/06/meteor_client_added_header.jpg)

On the other hand, Server side debug mode can be started at

`meteor debug`

A [node inspector](https://github.com/node-inspector/node-inspector) and the client will be prepared.

![Meteor debug](/public/images/2016/06/meteor_debug.jpg)

Node inspector server side debugging is then available at

`http://localhost:8080/debug?port=5858`
![Node inspector](/public/images/2016/06/node_inspector.jpg)

We are ready to develop single page application. 
Like many other [nodejs](https://nodejs.org) framework, [Meteor](https://www.meteor.com) works well in command prompt. IDE is in fact only a productivity tool to offer features like auto-complete, snippets and list of other development tools.

If you are already familiar with [AngularJS](https://angularjs.org/) and [ReactJS](https://facebook.github.io/react/). You would be able to follow the example and start to work. First impression [Meteor](https://www.meteor.com) is just a surprise to the scope it covers. However, I am also worrying the scope it covers. So far we don't need to know much what is the underlying libraries used, I am afraid learning curve could go steeper when development goes on. I've probably cover more on the development experience to [Meteor](https://www.meteor.com) in coming blog posts. Next section we are going to explore IDE support to Meteor [ATOM](http://atom.io).

#ATOM Installation

[ATOM](http://atom.io) is absolutely free. It is created by [GitHub](https://github.com). Download [ATOM](http://atom.io) from their official site.

![Atom official site](/public/images/2016/06/atom-1.jpg)

After installation is done, first launch experience is like
![Atom first launch](/public/images/2016/06/firstlaunch.jpg)

Impressive GUI, the tool is folder based. 

* **Open a Project** open a folder and start editing.
* **Install a Package**, these are plugins to [ATOM](http://atom.io)

I started with **Install a Package**. [ATOM](http://atom.io) by default ships with for instance GitHub, Git, Snippets, themes, styling related features. Many other development tools are available via packages.

I'll need "terminal". I hate shift+tab between IDE and my command prompt. I would love they both integrated. 

![Terminal Panel](/public/images/2016/06/terminal-panel.jpg)

I've chosen [terminal panel](https://atom.io/packages/terminal-panel). There are other packages, but I didn't bother to find another as long as terminal panel one works.

click "Install", and a while later the package will be installed. *NO RESTART* is needed :-)

Next, I'd **Open a Project** the meteor example downloaded earlier.

Alt+Shift+T, terminal started and it works as expected

![Terminal Panel GUI](/public/images/2016/06/terminal-panel-console.jpg)

Now, I can definitely start meteor application by 

`meteor run`

If I did that, that makes [ATOM](http://atom.io) only a text editor. IDE should be able to do more... 

Back to **Install a Package**, we need [meteor-helper](https://atom.io/packages/meteor-helper)

![Meteor Helper](/public/images/2016/06/meteor_package.jpg)

After [meteor-helper](https://atom.io/packages/meteor-helper) is installed, some setting is needed. Go to package setting page. *Make sure meteor Path in meteor helper is correct*

![Meteor Helper setting](/public/images/2016/06/meteor_helper_setting_1.jpg)

On the other hand, [meteor-helper](https://atom.io/packages/meteor-helper) by default run `meteor run`. Debug can be done by toggling the setting

![Meteor helper run in debug](/public/images/2016/06/meteor_helper_runindebug.jpg)

Next we can try to run meteor application ...

1. Alt+Ctrl+M or 
2. right click on your project file tree, "Launch Meteor"
![Launch Meteor](/public/images/2016/06/launchMeteor.jpg)

This will launch [Meteor](https://www.meteor.com) via package [meteor-helper](https://atom.io/packages/meteor-helper)

![Meteor Helper](/public/images/2016/06/meteor_helper.jpg)

It has no difference than running `meteor run` in command console.

The debugging experience is still relying on browser and node inspector. Hm... both build and debug features are not integrated to [Atom](https://atom.io/) yet.

![Meteor debug on ATOM](/public/images/2016/06/atom_debug.jpg)

I further tried [node-debug](https://atom.io/packages/node-debug) package in [Atom](https://atom.io/) but the behavior is weird which debugger can't locate source file. 

[Atom](https://atom.io/) is at most providing auto-complete and the coding level assistance to developer. Build and Debug still immature.

# Conclusion 
I couldn't wait to further explore capability Meteor offers. I found lint, uglyify, build options in command. But I was a little disappointed to ATOM support to Meteor though especially when I move around, the response wasn't too smooth.
