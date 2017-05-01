# Continuous Integration for POWER8

## Purpose

Many developers do not have access to IBM's POWER8 infrastructure for building and testing their codebases. Our goal is to expand the availability of POWER8 to developers so that they can ensure their software can run on a wider variety of platforms.

## Description

The OSL hosts a Jenkins server on a OpenStack cluster that runs on POWER8 servers. Users can request access to register one or more GitHub repositories on the Jenkins server, where they can configure the build process and the environment as needed. Builds will run in a Docker container by default, but can also be ran in a virtual machine if need be. They can also configure the system to run their tests, package any necessary files and binaries after running the build, and archive the build artifacts on the Jenkins server for later access. The service also supports providing e-mail notifications on build status and embedded build-notification for webpages. 

## Requirements

* POWER8 OpenStack Cluster  
* One VM on the OpenStack cluster to act as the Jenkins master
  * This is the primary target for the Ansible playbook
  * Tested with Ubuntu 16.04 ppc64le  
* (At least) One Docker host VM on the OpenStack cluster


