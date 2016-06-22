+++
author = "ryanwalls"
comments = true
date = "2016-06-22T10:03:36-06:00"
draft = false
share = true
tags = ["conferences", "docker"]
title = "DockerCon 2016 Highlights Day 2"
+++

Continuing from [my post from yesterday]({{< relref "post/DockerCon-2016-Highlights-Day-1.md" >}})... Here are the Day 2 Notes and Highlights.  

# Day 2
## Keynote
Keynote summary.... monetizing docker.  (This is a good thing because that means new features funded will continue flowing down to the open source offerings.)  

* Docker Data Center
* Docker Trusted Registry
* Docker Universal Control Plane
* HP Datacenter Enterprise

Demo

* Showed SQL Server running in a docker container on linux.  Interesting.
* Debugging a .net app running in a container from VS Code

ADP CTO short talk... he's very nervous.  Highlight was when he compared microservices to chicken nuggets and monoliths to chickens.  You had to be there.  

## Docker for Ops: Operationalize your Docker Built Apps in Production

Emphasis on Containers as a Service (CaaS) with a focus on agility, portability, and control.

Considerations for a production docker app

* Scale
* Security
* Monitoring
* Ecosystem

Docker Data Center handles security in a few different ways

* Provides fine grained access control
* Integrated content trust.  Pushed images are signed by individuals.

Demo from Evan Hazlett.  (Evan originally wrote Shipyard, which was a Docker Container UI, deployment tool, etc.  Shipyard probably became Docker UCP if I had to guess.)

UCP

* Can create teams, labels, and permissions for each
* Supports the new "services" feature in Docker 1.12
* Declare state of the services and the cluster takes care of matching the state
* Integrates with built in load balancing in Docker 1.12
* Handles rolling deploys
* Monitoring uses stats Docker API to aggregate data


Summary

Basically they have created a full ecosystem management tool with a UI.  Looks nice, but it is rather expensive... to the tune of $150/node/month.  Wayyyyy too expensive when you consider it doesn't include the compute resources.


## Friendly Microservices
* Autogenerate documentation
* Embed monitoring
* Service should not go down
* Make your service easy to deploy and scale
* Consumers should be able to hit the API directly in a non-prod environment
* Do not require a development environment to troubleshoot -- i.e.  use containers
* One base url for everything
  * Use an api gateway, such as Zuul
* Don't use wildcards and set correct domain in cookies
* Always use HTTPS
* Separate certs for api vs domain, so if one is compromised, the other still works.


## Security tips from Twistlock
* Compliance policies, adjust per application
* Monitor early in production
* Use active threat control (identify unusual behavior)

## Securing the Container Pipeline

![Securing all the steps](/images/posts/DockerCon-2016-Highlights-Day-2/security1.jpg)

Threats

* Run-time
  * Container exploits
  * Breaking out of container
  * Cross container attacks
  * DDoS
* At rest or during transport
  * Tampering of images
  * Unpatched OS or applications

Mitigations

* Platform security
* Monitoring and Response
* Access Controls
* Content Security

Access Control

* LDAP over SSL for Docker image transaction or use mutual TLS authentication for registry replication

Container Integrity

* Use signed images

Host hardening:

* Frequent patching
* Install only needed components and libraries
* Grsecurity/PaX for the kernel
* File system integrity monitoring
* Leverage Linux isolation capabilities

Container Hardening:

* Base image and app with latest updates/patches
* Leverage User namespaces (run as low privilege user on host)
* Install only needed components and libraries (i.e. no gcc, bash, or ssh)
* Avoid using Docker with --privileged flag
* Use --read-only when running containers
* Avoid providing access to the docker user and group
* Limit and/or separate host and kernel device access

Vulnerability management:

* Image scans with tools, such as docker security scanning.  
* Operating system
* Application source code and libraries
* Network Scans with traditional vulnerability scanners.
  * Discovery
  * Exposed services
* Auto and manual source code edits.  
* Remediation - have prioritization and SLAs for patching

Network Infrastructure

* Use an Intrusion Detection System (IDS)

Monitoring hosts:

* All host logs are saved
* Use machine learning to analyze logs

Monitoring containers and Apps

* Monitor all logs, similar to host
* Network activity monitoring
* Disk activity monitoring
* Memory monitoring - docker and container process activity

Digital Forensics

* Have incident response plan/policies in place
* Memory, disk, network forensics
* Build a super timeline of events using various tools like:
  * Sleuth Kit
  * Plaso
  * dd: Raw disk image

Memory forensics

* Useful because everything runs in memory
* Faster discovery vs disk forensics

![Security Summary](/images/posts/DockerCon-2016-Highlights-Day-2/security-summary.jpg)

Conclusion

Lots of good information to incorporate.  Definitely could dedicate a full time position to this.   

## Docker networking deep dive

* libnetwork - not just a driver interface
  * handles all docker container networking
  * ip address management
  * multi-host networking
  * service discovery
  * load balancing
  * allows for extensions/plugins

New features in 1.12
![New networking features](/images/posts/DockerCon-2016-Highlights-Day-2/networking.jpg)

* Cluster aware
* De-centralized control plane
* Highly scalable
* Routing mesh
* Load balancing
* Service discovery

Conclusion

Docker 1.12 is going to make a lot of engine-to-engine communication seamless.  I left early on this talk though because the deep dive was deeper than I needed.

## Project Tesson Demo

![@kobolog](/images/posts/DockerCon-2016-Highlights-Day-2/tesson.jpg)

[@kobolog](https://twitter.com/kobolog) shared his open source project called "Tesson" that maximizes resource usage by analyzing a machine's hardware topology and handles spawning/pinning instances of a Go app to utilize all the hardware capability.  https://github.com/kobolog/tesson

Caught the tail end of this talk... very impressive.  Will have to try it out on some of our CPU intensive Go apps.

## Summary
DockerCon was great.  Day 1 was the cool announcements.  Day 2 was the sales pitch... at least for the keynote.  Both days were informative.  The conference was well organized.  Food was good.  Good set of speakers.  Great location.  And the weather was epic.  

Overall, can't complain.... I got out mountain biking both days.  Highly recommend Duthie Hill and Tiger Mountain.  And if you need a bike rental check out http://compassoutdooradventures.com.  They deliver/pickup at both destinations.

![Mountain Biking at Tiger Mountain](/images/posts/DockerCon-2016-Highlights-Day-2/mountain-biking.jpg)
