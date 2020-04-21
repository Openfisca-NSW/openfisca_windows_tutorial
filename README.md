# A tutorial to getting OpenFISCA working on Windows.

Good morning.

This is a guide for getting OpenFISCA working on a Windows machine, written specifically from a non-tech, policymaker background.

This guide is specific to the OpenFISCA NSW implementation (but most of the principles should work for other jurisdictions, too.)

And more specifically this is written for DPIE SPB, who are going to be implementing Rules as Code for the ESS, using OpenFISCA.

(Forking of this tutorial for other jurisdictions is welcome.)

This guide assumes you have:
- some knowledge of coding, specifically Python, but probably not an expert
- knowledge of the specific legislation/ruleset you're looking to code
- a Windows machine with Windows 10 Professional

This guides assumes you do not have:
- knowledge of what Docker does
- a MacOS machine
- strong coding skills

If you're coming at this from a strong programming/coding background, this guide might be a bit simple to you - I'm going to explain a bunch of core code concepts for the policy/program maker who may not have them. 
Apologies, but it's needed to get these skills to a foundational level. Skip the stuff you already know. :P

Let's get started.

## Prerequisites

You'll need: 

#### Hardware
- a machine with Windows 10 Professional on it
- a 64 bit processor [this should be standard on most Windows machines these days...]
- 4GB RAM [as above]
- administrative privileges

#### Software
- Docker Desktop - available at https://www.docker.com/products/docker-desktop
- a Github account
- Git - available at https://git-scm.com/. Note that Git comes with a Python image. 
- [RECOMMENDED] - Atom - available at atom.io
- NEED TO CHECK - install Python 3.7 prior? 

Note - it is theoretically possible to get OpenFISCA working on a Windows 7, Windows 8 or Windows 10 Home machine - but this is a labour intensive process that is not guaranteed to work.

The author of this guide spent a full workday on this task and made no progress. Whoops.

*TO DO - DETERMINE WAY TO PULL git config -- global user.email & git config -- global user.name into Docker universal details (note current user has to input user name, password etc for every container when they git commit etc)*

## Installing Docker Desktop

Here's the simple way of putting it - Docker is a way of building an virtual image of a Unix application, in a self-contained "container".

The idea with OpenFISCA is you build these different repositories that act as an application of a specific ruleset - and then pull from different repositories to ensure consistency of terms across legislation.

So you work in a different Docker container for each piece of legislation (or in the case of large pieces of legislation, each section) and then pull it together with the wider NSW base of legislation. 

(And the idea is, whatever you do in a Docker container doesn't impact anything outside of that container, unless it's specifically written to pull from something else.)

You can only install Docker Desktop on Windows 10 Professional because Windows 10 Professional provides Virtual Machine support as native to the platform, which previous Windows versions doesn't.

To install Docker Desktop, go to the above URL, pull the Stable version (unless you're very sure of what you're doing) and then install the .exe. 

The default settings are sufficient for this task - the only question that might confuse you is whether to run containers based on Linux containers or Windows containers - you'll want to run it on Linux containers. (This can be changed later if required.)

You'll need to authorise Docker with your system password at some point in the process - this is safe and required to install networking components and manage the Hyper-V VMs. 

You'll then need to launch Docker Desktop - it'll throw some suggested next steps like providing read access to your C:/ drive (safe, do this unless you'd like some nightmares) and to get a Docker ID (I've done this but haven't delved into the usefulness of it yet.)

## Some Core Docker Concepts

#### Container

#### Image

## Testing Docker Desktop

Here we're going to test that Docker works, build a container and then remove it.

Here we go. (Below steps sourced from https://docs.docker.com/docker-for-windows/.) 

1. Open a terminal window - Command Prompt (typically found in Windows Systems in your start menu) or PowerShell. It'll give you the blinking black window asking for some code.

2. Let's check what version of Docker you have installed. Type "docker --version" and then run the code (hit enter). You'll get something like this:

> docker --version

Docker version 19.**.*

This shows you've installed Docker version 19, whatever sub-version number. Great, works as intended. 

3. Let's now run the foundational command for coders - "hello world", and put it in a Docker container. 
run "docker run hello-world". You should get something like this. 

> docker run hello-world

docker : Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:c3b4ada4687bbaa170745b3e4dd8ac3f194ca95b2d0518b417fb47e5879d9b5f
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

Great. Docker is telling us the installation is working correctly. Good good. It should exit out of this container once it runs, but you'll still have your Terminal window. Let's see what happened there.

4. Let's look at the image that was downloaded from Docker, that it pulled from the Docker Hub. Run "docker image ls".

You'll get something like this: 

C:\****\****>docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        3 months ago        13.3kB

Great, it tells you where it's from, what the ID of the relevant image is, when it was created and its size. Perfect. 

5. Let's now look at the container that this image was in. Run "docker container ls --all". 

You'll get something like this. 

C:\****\****>docker container ls --all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
da8c6dc82739        hello-world         "/hello"            6 minutes ago       Exited (0) 6 minutes ago                       goofy_wing

Cool, so it shows you the Container's ID, the image inside it, how we pulled the relevant image, when it was created, what it's current status is, what port it's running on (if any), and a colloquial name for it.

6. Go read the Docker help pages by running some help commands, like docker --help or docker container --help. Do this as you please!

