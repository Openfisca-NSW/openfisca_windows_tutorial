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
The author of this guide spent a full workday on this task and made no progress. 

