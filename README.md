# A tutorial to getting OpenFISCA working on Windows.

Good morning. Evening, afternoon, whatever. Time is a construct.

This is a guide for getting OpenFISCA working on a Windows machine, written specifically from a non-tech, policymaker background.

This guide is written with the intent of working with the OpenFISCA NSW implementation (but most of the principles should work for other jurisdictions, too.)

More specifically, this guide was written for the Sustainability Programs Branch in the Department of Planning, Industry and Environment - with the intent of distributing to other policyamkers without an existing knowledge of OpenFISCA.

Forking of this repository/guide for your specific jurisdiction is welcome - please credit @liamdmccann for the initial guide. :)

## Guide Principles

This guide is written to the concept of, you can give this guide to a policymaker and they can get themselves set up in OpenFISCA without requiring technical assistance.

If you're a policymaker/rulemaker/program delivery staff, you should be able to get from this section of the guide to being fully up and running in OpenFISCA, without requiring assistance from your technical staff. 

However, you'll likely run into issues through this process - there's some level of technical complexity involved with this. Due to this, this guide is perpetually incomplete.

If you run into a problem that isn't covered by this guide, the best way to respond is to submit a pull request to this repository. That way, not only can you get an answer to your question, but you can contribute to the knowledge in this document through resolving your issue!

If you're coming at this from a strong programming/coding background, this guide might be a bit simple to you - I'm going to explain a bunch of core code concepts for the policy/program maker who may not have them. 

Let's get started.

## Prerequisites

This guide assumes you have:
- some knowledge of coding, specifically Python, but probably not an expert
- knowledge of the specific legislation/ruleset you're looking to code
- a Windows machine with Windows 10 Professional

This guides assumes you do not have:
- knowledge of what Docker does
- a MacOS machine
- strong coding skills

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

Note - it is theoretically possible to get OpenFISCA working on a Windows 7, Windows 8 or Windows 10 Home machine - but this is a labour intensive process that is not guaranteed to work, and is not recommended.

*TO DO - DETERMINE WAY TO PULL git config -- global user.email & git config -- global user.name into Docker universal details (note current user has to input user name, password etc for every container when they git commit etc)*

## Installing Docker Desktop

#### What is Docker Desktop?

Here's the simple way of putting it - Docker is a way of building an virtual image of a Unix application, in a self-contained "container". It's a virtual computer on your existing machine. 

#### Great, but why is this important?

The core concept with OpenFISCA is you build these different repositories that act as an application of a specific ruleset - and then pull from different repositories to ensure consistency of terms across legislation.

So, you work in a different Docker container for each piece of legislation (or in the case of large pieces of legislation, each section) and link in with the wider base of NSW legislation - in the long term.

#### Why is having Windows 10 Professional important?

You can only install Docker Desktop on Windows 10 Professional because Windows 10 Professional provides Virtual Machine support as native to the platform, which previous Windows versions doesn't.

You're welcome to try getting Docker Desktop working on Windows 7, 8 or Windows 10 Home, but this is labour intensive and not recommended. If you do this and have success, please make a submission to this repository detailing how you did it!

#### Installing and using Docker Desktop

To install Docker Desktop, go to the above URL, pull the Stable version (unless you're very sure of what you're doing) and then install the .exe. 

The default settings are sufficient for this task - the only question that might confuse you is whether to run containers based on Linux containers or Windows containers - you'll want to run it on Linux containers. (This can be changed later if required.)

You'll need to authorise Docker with your system password at some point in the process - this is safe and required to install networking components and manage the Hyper-V VMs. 

You'll then need to launch Docker Desktop - it'll throw some suggested next steps like providing read access to your C:/ drive (safe, do this unless you'd like some nightmares) and to get a Docker ID (I've done this but haven't delved into the usefulness of it yet.)

## Some Core Docker Concepts

#### Container

#### Image

## Testing Docker Desktop

Here we're going to test that Docker works, by building a container and then removing it.

Here we go. Below steps sourced from https://docs.docker.com/docker-for-windows/

1. Open a terminal window - Command Prompt (typically found in Windows Systems in your start menu) or PowerShell. It'll give you the blinking black window asking for some code.

2. Let's check what version of Docker you have installed. Type 

> docker --version

and then run the code (hit enter). You'll get something like this:

```sh
docker --version
Docker version 19.**.*
```

This shows you've installed Docker version 19, whatever sub-version number. Great, works as intended. 

3. Let's now run the foundational command for coders - "hello world", and put it in a Docker container. 

To do this, run 

> docker run hello-world

You should get something like this. 

```sh

docker : Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:c3b4ada4687bbaa170745b3e4dd8ac3f194ca95b2d0518b417fb47e5879d9b5f
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

Great. Docker is telling us the installation is working correctly. 

It should exit out of this container once it runs, but you'll still have your Terminal window. Let's see what happened there.

4. Let's look at the image that was downloaded from Docker, that it pulled from the Docker Hub. Run 

> docker image ls

You'll get something like this: 

```sh

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        3 months ago        13.3kB

```

It tells you where the image is from, what the unique ID of the relevant image is, when it was created and its size. Perfect. 

5. Let's now look at the container that this image was in. Run 

> docker container ls --all

You'll get something like this. 

```sh

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
da8c6dc82739        hello-world         "/hello"            6 minutes ago       Exited (0) 6 minutes ago                       goofy_wing

```

Cool, so it shows you the Container's ID, the image inside it, how we pulled the relevant image, when it was created, what it's current status is, what port it's running on (if any - you'll know if you need to worry about this), and a colloquial name for it.

6. Go read the Docker help pages by running some help commands, like 

> docker --help 

or 

> docker container --help. 

Do this as you please!

## Getting OpenFISCA running

You're probably reading this OpenFISCA Windows guide with the aim of getting OpenFISCA working, not necessarily learning everything there is to learn about Docker. Let's do that.

In this section we're going to test the test suite that the core OpenFISCA team support, using their methodology.

Before you start this section, figure out a folder to put all of your OpenFISCA stuff in, preferably somewhere easy to find and without spaces in the filepath. For example, I use C:\Users\Liam\Desktop\rules_as_code.

Alright. Let's do this. 

1. We're first going to set our working folder to the folder you're going to be working in. We're going to use the cd command - cd means "change directory" - at the command line level to do this.

To do this, open a terminal or PowerShell window, and run 

> cd your/folder/path

Here's an example:

> C:\Users\Liam>cd Desktop/rules_as_code

So I'm already in C:\Users\Liam, so I'm going to cd into the specific folder that I want to do this work in, which is on my desktop. 

So you can now think of this as whenever you refer to cd, you're referring to this folder. 

2. Now we're going to build a container within this folder. To do this, in the same terminal as above, run 

> docker run --rm -it -v %cd%:/rules_as_code -w /rules_as_code python:3.7 bash

What's happening here is we're telling Docker to build a container, with a couple of flags in top for optimisation. 

**Skip to 3. if you don't care about what's actually happening here.**

The first of these is "--rm" - what this goes is it automatically cleans up the container and removes the relevant file system when you exit the container.

For the purpose of OpenFISCA, because you're not going to have this container running 24/7, including this prevents lots of file systems building up on your machine. It's great.

"-it" does something complicated that I won't try to explain *(Sara/Asghar if you know more about this, throw something in?)* but it essentially makes your container interactive, which is great. We want our code base that we're writing to be interactive.

"-v" defines the Docker volume that the container is going to pull a file from. A volume is essentially a directory or file that exists outside of the container, that can be used to share data between containers. You'll want to set this to the folder you're going to work in. 

running "-v %cd%:/end_folder" means that instead of having to type out the whole file path (which is boring AND labourious) you can just use the path you've cded to in Step 1. 

"-w" defines the folder that Docker is going to work within. Same principle as above (but you don't need to type out the %cd% because you're already in that volume.)

This folder should be the same as whatever you set "-v" to - you want your local files and the files in the container to be the same, so you can edit the local files and have that reflected in what's in the container. (and then run tests in the container, and be able to make changes in the local files to impact the tests. You want this!)

python:3.7 bash means you're going to build this container with a python3.7 and a bash image included. You'll need this to run OpenFISCA commands and generally do work across it. 

**The explanation of what Step 2 does ends here.**

3. Let's check you've done the above step properly. 

We're going to check to see what Python libraries are installed in this container, by running 

> pip list 

You should get a list of packages in this container as the result of running this - something like

```sh

 Package    Version
 ---------- -------
 pip        *.*.*   
 setuptools *.*.* 
 wheel      *.*.* 

```
 
 The version numbers for these don't really matter (at present) but they should be the most updated as possible, unless you're aware of why you'd use a previous version of pip. 
 
 4. Great - now, let's install the OpenFISCA country template. Let's install it so you can modify it if you choose. 
 
To do this, you'll need to first get the source code for this extension. 

Do this by running 

> git clone https://github.com/openfisca/country-template.git 

This searches the openfisca repository on Github for the OpenFISCA country template, and by searching for the .git file, pulls all of the relevant file into the directory you're working in.

You should then be able to see the country-template folder in your current working directory.

Note that you only need to do this the first time you build a container for this OpenFISCA template (or any OpenFISCA codebase) - unlike the container, these files are stored on your hard drive and will stay once you stop work for the day.

If you already have this folder and it has stuff in it, it'll return 

```sh

fatal: destination path 'country-template' already exists and is not an empty directory.

```

5. Next you need to set your working directory to this new repository. run 

> cd country-template

to change into this directory.

6. Now install this directory with "make install". It will run a LOT of stuff, most of which you can ignore. Go make a cup of tea or something. 

(What's happening here is - there's a file in the Github repository called a Makefile. Don't edit it [unless you know what you're doing.] 

When you make install, it runs a script that pulls all of the relevant libraries and tools needed to use OpenFISCA, and then puts them in the right places in the container. 

Running one command beats running 80 commands or whatever is in there.)

If you get something like this, you know you've done it right: 

```sh

root@8acf57ad8444:/rules_as_code/country-template#

```

7. Then run 

> make test 

This runs a script that tests all of the default tests in the test OF repository against the legislation in this repository.

You'll get something like this. 


```sh

================================================= test session starts ==================================================
platform linux -- Python 3.7.7, pytest-5.4.1, py-1.8.1, pluggy-0.13.1
rootdir: /rules_as_code/country-template
collected 35 items

openfisca_country_template/tests/age.yaml ....
openfisca_country_template/tests/basic_income.yaml ......
openfisca_country_template/tests/disposable_income.yaml .........
openfisca_country_template/tests/housing_allowance.yaml ..
openfisca_country_template/tests/housing_tax.yaml ...
openfisca_country_template/tests/income_tax.yaml .
openfisca_country_template/tests/social_security_contribution.yaml ......
openfisca_country_template/tests/reforms/modify_social_security_taxation.yaml ...
openfisca_country_template/tests/situations/income_tax.yaml .

=================================================== warnings summary ===================================================
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245
  /usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:245: PytestDeprecationWarning: direct construction of YamlFile has been deprecated, please use YamlFile.from_parent
    return YamlFile(path, parent, self.tax_benefit_system, self.options)

/usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:102: 35 tests with warnings
  /usr/local/lib/python3.7/site-packages/openfisca_core/tools/test_runner.py:102: PytestDeprecationWarning: direct construction of YamlItem has been deprecated, please use YamlItem.from_parent
    yield YamlItem('', self, self.tax_benefit_system, test, self.options)

-- Docs: https://docs.pytest.org/en/latest/warnings.html
=========================================== 35 passed, 44 warnings in 0.23s ============================================

```


If you get this test session or something similar, you've installed OpenFISCA correctly. If you get XX passed, XX failed and XX warnings, it's installed correctly. 

Congratulations! This is some tricky stuff, and you've nailed it. Give yourself a treat. 

## Let's get OpenFISCA-NSW going

Cool. So this has many of the same steps as above, but also requires using OpenFISCA-NSW as a base system, and then using an ESS repository (let's use NABERS) as an extension of this.

The long-term principle is, we want to build interoperability and compatibility of definitions across all of NSW. By using OpenFISCA-NSW as a base and building legislation off this as an extension, we can ensure that the core entities used in legislation (i.e. people, child, building, etc) are consistent across whole of legislation.

Or, identify where they need to not be identical and then redefine them in turn. 

Let's get on with it. Much of this is the same as the above example, but I'll still go into the same level of detail.

1. We're first going to set our working folder to the folder you're going to be working in. We're going to use the cd command - "change directory" at the command line level to do this.

To do this, open a terminal or PowerShell window, and run cd 

> your/folder/path.

In the below example I'm going to use cd/Desktop/rules_as_code, as in the previous example.

2. Now we're going to build a container within this folder. To do this, in the same terminal as above, run 

> docker run --rm -it -v %cd%:/rules_as_code -w /rules_as_code python:3.7 bash

3. Let's check you've done the above step properly. We're going to check to see what Python libraries are installed in this container, by running 

> pip list 

4. Great, here's where we deviate - now, let's pull the OpenFISCA-NSW base repository. Run 

> git clone https://github.com/Openfisca-NSW/openfisca_nsw_base.git
 
This pulls the openfisca_nsw_base files into your base rules as code folder. 

In my experience you don't need to install this repo, you just need to have it present in your base folder. 

5. Now go and git clone the relevant extension you want to work in, say, run 

> git clone https://github.com/Openfisca-NSW/openfisca_nsw_ess_nabers.git

6. Now cd into the folder that this repo is cloned to, by running 

> cd openfisca_nsw_ess_nabers

7. And now run 

> make extension

and it'll install a whole bunch of stuff again...

8. And now run 

> make test

 and it'll run the 500ish tests that currently exist for this repo.

Great! You're in a NSW government OpenFISCA repository. 

Keep this terminal window open while you're editing files - you'll need it to push your changes.

## Editing files

The easiest way I've found to edit files in an OpenFISCA repository is to use Atom - it's much prettier and easier to use than the command line to edit files.

Open Atom, use File > Open Folder, point it at the folder that contains the repo you want to edit, you'll get a tree view (something like this)

![Atom OpenFISCA tree](/img/atom_openfisca_country_template.png)

Click on the file you want to edit, make your edits, remember to save them and then make test again - whatever changes you've made should be reflected.

## Uploading changes

Great, you've made some changes in a repository. Now you're ready to upload them to Github for others to use. Here's how to do this.

Here's how to do that.

1. run

> git add -A

What this does is it adds the edits, new files, deletion of files, etc., to the current local working repository, and tells the container that there are some changed files.

2. run

> git commit -m "your comment here"

This stages the files, in preparation for upload. Note that you'll have to also include a comment when you change a file, so someone else (or yourself!) can see the change and understand the impact of changes to files.

Short, concise, informative comments are best - as always.

The first time you do this in a container, it'll ask you for your github email and name, like so:

```sh

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

```

This tells Github who uploaded these changes. (We're working on a way for this to be supplied in Docker so you don't need to enter this for each container. Stay tuned.)

So run

>   git config --global user.email "you@example.com"

and

>   git config --global user.name "Your Name"


3. run

> git push

If your files are okay to push, and you have rights to push changes to the relevant repository, you'll see something like this

```sh

root@076acbf036ac:/rules_as_code/your_repository_here# git push
Username for 'https://github.com': your_user_name_here
Password for 'https://your_user_name_here@github.com': # note that you'll need to enter your Github password here
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to 2 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 564 bytes | 141.00 KiB/s, done.
Total 7 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), completed with 5 local objects.
To https://github.com/your_repository/your_repository_here.git
   b56a1c4..3a7aa64  master -> master
   
```

Great! You've uploaded your changes to github. Congratulations!
