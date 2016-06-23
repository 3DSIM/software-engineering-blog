+++
author = "ryanwalls"
comments = true
date = "2016-06-21T10:03:36-06:00"
draft = false
share = true
tags = ["conferences", "docker"]
title = "DockerCon 2016 Highlights Day 1"
+++

At 3DSIM we believe in continuing education and investing in our developers.  We put that into practice in many ways and one of those ways is to encourage developers to attend a conference.  This year I'm attending DockerCon in Seattle.  Here are some running higlights...

# Day 1
## Keynote highlights
![Success](/images/posts/DockerCon-2016-Highlights-Day-1/dockercon-day1-keynote.jpg)  

* Docker for Mac allows you to debug code running in a container and live reload it.  (Need to figure out details)
* Docker for Mac Beta is now open to anyone at https://www.docker.com/products/docker#/mac
* Docker 1.12 will have orchestration features built-in.  https://blog.docker.com/2016/06/docker-1-12-built-in-orchestration/  
  * Swarm mode.  Self forming, self-healing.  No external data store required (e.g. Consul or etcd)
  * Secure node to node communication.
  * There is now a docker service API that handles scaling, rolling updates, scheduling, application specific health checks, rescheduling on node failure.  This look amazing...
  * Built in routing.  Load balancing, DNS based service discovery, no separate cluster to setup.  

Example commands with new built-in orchestration:

```
docker service create --name vote -p 8080:80 instavote/vote
docker service ls
docker service tasks vote
docker service scale vote=6
docker service update vote --image instavote/vote:movies
docker service update vote --image instavote/vote:indent --update-parallelism 2 --update-delay 10s
```

* Docker for AWS and Docker for Azure.  (Interesting.)  
  * "Seamless" (Not sure we will give up Ansible for deploying...)
  *  Will have to find more links to details... sounds like specialized cloudformation templates.
* (Experimental) Distributed Application Bundle (DAB)... a format for multi-container applications.  

## Talk Highlights: "The Golden Ticket: Docker and High Security Microservices"
http://dockercon2016.sched.org/event/70Ni

* Use TLS
* Least trust/Least access
* Security starts with the base OS - does the base OS manage security in a "good" way?  Start with a "minimal" base
* Minimal kernel
*  Should also have minimal containers.  Larger containers = more patching, disk space, attack surface and post exploitation utilities.  
* He's a fan of Go... only put a single binary in the Docker container.  (Shot out to Richard Bolt who gave a talk on this at our last SLC Docker meetup.)
* Use Mandatory Access Control (MAC) - which is enabled by default in Docker.
  * Use aa-genprof to generate an apparmor profile, or use Bane
  * Profile generators are not perfect, keep an eye after switching to "mandatory" mode                       
  *  Default seccomp filter permits 304 calls.
  * Can use sysdig to figure out what calls are open on the system (requires kernel module)
  * Pitfalls of seccomp:
   * Fragile
  * Libseccomp - go library


High security docker microservices

* Enable user namespace
* Use specific apparmor if possible
* Seccomp whitelist
* harden host system

Handling secrets

* Avoid environment variables and flat files
* Use something like Vault https://github.com/hashicorp/vault

Networking

* Use TLS.  All network traffic should be encrypted and authenticated.

Security in general

* Develop threat models for each application
* Log everything and all access.  Keep logs centrally.


## Docker for Developers 1 and 2
Various demos of how native Docker for Mac/Windows and Docker Cloud make development easier.  Both show promise.  

Docker Cloud demos involved too much clicking and UI work... will have to investigate what APIs are available.

Installed Docker for Mac yesterday and so far so good.  

## Immutable Infrastructure
* Limit number of dependencies and libraries in projects
* Shorten lead time
* High performing organizations:
  * Deploy more often
  * Lead time is short
  * High change success rate
  * Low MTTR

Overall, entertaining talk without a lot of "new" content.  

## Summary
Good first day.  Highlight for me is Docker 1.12 orchestration features... will be using those in our pipelines as soon as they become available.  

See my day 2 notes here: [DockerCon 2016 Highlights Day 2]({{< relref "post/DockerCon-2016-Highlights-Day-2.md" >}})
