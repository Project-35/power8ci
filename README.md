# Continuous Integration for POWER8

## Purpose

Many developers do not have access to IBM's POWER8 infrastructure for building and testing their codebases. Our goal is to expand the availability of POWER8 to developers so that they can ensure their software can run on a wider variety of platforms.

## Description

The OSL hosts a Jenkins server on a OpenStack cluster that runs on POWER8 servers. Users can request access to register one or more GitHub repositories on the Jenkins server, where they can configure the build process and the environment as needed. Builds will run in a Docker container by default, but can also be ran in a virtual machine if need be. They can also configure the system to run their tests, package any necessary files and binaries after running the build, and archive the build artifacts on the Jenkins server for later access. The service also supports providing e-mail notifications on build status and embedded build-notification for webpages. 

## Requirements

* POWER8 OpenStack Cluster  
  * Used both for hosting Jenkins and for launching build VMs
* One VM on the OpenStack cluster to act as the Jenkins master
  * This is the primary target for the Ansible playbook
  * Python installed 
  * Tested with Ubuntu 16.04 ppc64le and Python 2.7.12
* (At least) One Docker host VM on the OpenStack cluster
  * This should have Docker installed and listening on a tcp port
* An OpenStack snapshot of a VM for your builds.
  * This allows you to create multiple VMs with different configurations
* A Docker image
  * We use one modified from ppc64le/ubuntu:16.04 ppc64le/ubuntu:16.04
* A host computer from which to run the Ansible playbook
  * Ansible version >= 2.3
* GitHub -- Used for both authentication, and as the source for code

## Installation 

First, clone our repo. Then you will need to go to GitHub and register a new OAuth application. You
can find this under your account or organization by going to Settings -> OAuth
applications. Select the "Register a new application" button and fill in your
information. We use a `secret.yml` file that is not commited to GitHub to keep these in. Create that
file in `roles/jenkins/vars/` with the following content:
```
oauth:
  clientid: '<your clientid from GitHub>'
  clientsecret: '<your clientsecret from GitHub>'
```

You will also want a to get a GitHub token for your admin user. Go to your users Settins page and click
on Personal access tokens. Add that to the same secret.yml file as above:
```
my_jenkins_params:
  url_username: <your admin user's GitHub username>
  url_password: <your GitHub token>
  url: < Your Jenkins url -- http://jenkins.example:8080>
```
Next, edit the hosts file and group_vars/jenkins to replace power-ci.osuosl.org with your VM's IP address or hostname.

Now run the Ansible playbook. You will need your ssh key for your OpenStack VM.
```
ansible-playbook -i hosts jenkins.yml --private-key=<your ssh key>
```

## Configuration

You should now be able to login to the Jenkins interface and configure the OpenStack Cloud Plugin.
Go to Manage Jenkins -> Configure System. You can also label your Docker Image(s) and Build VMs
so that you can specify the specific platform for individual builds. 
