# The Lightweight Docker Runtime (TLDR)

# General

The purpose of The Lightweight Docker Runtime is to serve as a platform that is easy to stand up on a variety of providers, to be able  easily demonstrate the key concepts and capabilities that we believe should be part of a container platform such as composition of applications via Docker Compose, clustering via Swarm, and service discovery, among others.

TLDR is not production-ready or enterprise-grade, nor does it intend to be.

# Features

- Docker Machine to provision a cluster, either locally via VirtualBox or on Amazon AWS
- 3-node Docker Swarm cluster (easily expandable to more nodes)
- Dynamic service discovery and registration using Consul and Registrator
- Deployment of applications via Docker Compose and overlay networks
- Transparent application container load balancing using the tldr/alb container, which provides seamless scaling of application containers within the Swarm cluster
- Log aggregation via Logspout, ElasticSearch, Kibana and Logstash 
- Monitoring and metrics via Prometheus and cAdvisor

# Pre-requisites

The following are needed to get this environment running:

- Docker Toolbox 1.9.1
- Docker Machine 0.6.0 or higher
- Bash/Cygwin if running on Windows

When running locally, 8Gb of RAM is the minimum recommended amount of memory in order to comfortably run all components.

# Usage

## Setting up locally

Use script ```start.sh``` to set up the Docker Swarm platform and technical components. The process should take approximately 10-15 minutes, depending on the specs of your host machine as well as the speed of your network connection (several containers will be pulled from the Docker Hub, and cached locally).

Once the process is complete, run ```info.sh``` to provide a list of the available endpoints for inspection.

## Setting up in AWS

TLDR is supported on AWS when running in region *eu-central-1*, and provides a process for provisioning AWS resources as well as deploying the platform components.

The platform is very likely to work in other regions, but please see "Running in a region that is not eu-central-1" below for the time being until the provisioning process is improved to take other regions into account.

### Setting up AWS resources

First, a set of Amazon AWS resources will be provisioned to host the platform: a VPC, with a subnet, routing tables and an internet gateway. This is to keep the platform separated from any other AWS resources that may currently be in use.

Provisioning of AWS resources is handled via Terraform, please see provisioning/aws/README.md for detailed steps.

### Provisioning the components

Please note that Docker Machine 0.5.2 or higher is needed, otherwise the process will not work.

Once the AWS resources have been provisioned, the TLDR platfomr can be deployed.

Set the following environment variables with your AWS secrets before running the start.sh script:

```
export AWS_ACCESS_KEY_ID=<secret key>
export AWS_SECRET_ACCESS_KEY=<secret access key>
export AWS_VPC_ID=<vpc-id>
export AWS_DEFAULT_REGION=eu-central-1
export AWS_DEFAULT_ZONE=<zone>
```

The values for AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY can be obtained from the AWS admin console. 

The value for AWS_VPC_ID and AWS_DEFAULT_ZONE will also be available from the console, as the AWS provisioning process creates a VPC called ```tldr-vpc```. Once the AWS provisioning process via Terraform is complete, use the AWS admin console to obtain the id of the ```tldr-vpc```VPC resource, e.g. ```vpc-fe53be97``` and the respective zone of the VPC subnet e.g. ```b```.

### Overriding EC2 instance sizes

Additionally, the following environment variables can be used to override some of the default values used for EC2 instances:

```
AWS_INSTANCE_TYPE=t2.micro
AWS_ROOT_SIZE=16
```

### Running in a region that is not eu-central-1

At the moment the AMI image for Ubuntu 15.10 is hardcoded as ami-fe001292 and *only works in region eu-central-1*. If you are running in a region that is not eu-west-1, find out the id of the Ubuntu 15.10 image for your region and expose it as an environment variable:

```
export TLDR_DOCKER_MACHINE_AMI=ami-xxxxx
```

# Reference applications

See the [ToDo demo application](apps/todo/) for more information.

# Reporting issues

Use [TLDR's Github issue page](https://github.com/Accenture/tldr/issues) to report issues related to TLDR or any of its components. Do not report issues in subprojects as that would become a nightmare to main and we'd rather keep issues and contributions centralized.

When reporting an issue, please paste the contents of your *log.txt* file into the issue for our reference (this file is created automatically by start.sh). If you used the individual scripts that are under bin/ instead, please paste the output of your screen.

# Contributing

TLDR is licensed under the Apache 2.0 license. Pull requests with contributions are more than welcome.

# Releases

[Release 1.0](https://github.com/accenture/tldr/issues?q=is%3Aissue+milestone%3A1.0+is%3Aclosed)

# TODO, Known issues

Please see the [list of open issues](https://github.com/Accenture/tldr/issues) for more details.
