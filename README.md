# Docker Image for Atlassian Bitbucket Server 4.1.0

*this documentation isn't fully done yet - we're still working on major and minor issues corresponding to this repository base!*

this repository provide the currently latest version of Atlassians sourcecode repository/review software [Bitbucket](https://de.atlassian.com/software/bitbucket) including the recommended [MySQL java connector](http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.36.tar.gz) for an easy and painless docker based Bitbucket installation. Take note that this repository will be used inside our docker atlassian application workbench sources, which are also available on [Github](https://github.com/dunkelfrosch/docker-atlassian-wb) as soon as documentation is completed. *In this workbench we've combined several Atlassian products (JIRA, Confluence and Bitbucket) using advanced docker features like docker-compose based service management, data-container and links*

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)
[![System Version](https://img.shields.io/badge/version-0.9.7-blue.svg)](VERSION)
[![Build Status](https://travis-ci.org/dunkelfrosch/docker-bitbucket.svg?branch=master)](STATUS)

## Preparation
We recommend the [latest Docker version](https://github.com/docker/docker/blob/master/CHANGELOG.md). For simple system integration and supervision we suggest [Docker Compose](https://docs.docker.com/compose/install/). If you're using MacOS or Windows as host operating system, you may take the advantage of [Docker Machine](https://www.docker.com/docker-machine) for Docker's VM management. Bitbucket requires a relational database like MySQL or PostgreSQL, so we'll provide a specific Docker Compose configuration file to showcase both a Bitbucket-MySQL link and a data-container feature configuration. Use the installation guides of provided links down below to comply your Docker preparation process.

[docker installation guide](https://docs.docker.com/engine/installation/)</br>
[docker-compose installation guide](https://docs.docker.com/compose/install/)</br>
[docker machine installation guide](https://docs.docker.com/machine/install-machine/)</br>


## Installation-Method 1, the classic Docker way
As long as our image isn't available via docker.io hub repository, you will need to build it by yourself using this Github repository. These steps will show you the generic, pure Docker based installation of our Bitbucket image container, without any database container linked or data-container feature.  *We also will provide a Docker Compose based installation in this documentation (Method 2)*.

1. checkout this repository

```bash
git clone https://github.com/dunkelfrosch/docker-bitbucket.git .
```

2. build Bitbucket (version 4.1.0) image on your local docker host, naming image "dunkelfrosch/bitbucket:4.1.0"

```bash
docker build -t dunkelfrosch/bitbucket:4.1.0
```

3. start your new Bitbucket application container

```bash
docker run -d -p 7990:7990 dunkelfrosch/bitbucket 
```
	
4. finish your installation using atlassian's browser based configuration 
just navigate to `http://[dockerhost]:7990` 


## Installation-Method 2, docker-compose (simple)
The following steps will show you an alternative way of your Bitbucket service container installation using Docker Compose


1. checkout this repository

```bash
git clone https://github.com/dunkelfrosch/docker-bitbucket.git .
```

2. create a docker-compose.yml file in your target directory (or using the existing one), insert the following lines (docker-compose.yml in ./sample-configs/ directory). 

![](https://dl.dropbox.com/s/rj8zsfmkor4ynj5/dc_setup_001.png)

3. start your bitbucket container by docker-compose

```bash
docker-compose up -d bitbucket
```

4. (*optional*) rename the resulting image after successful build (we'll use our image auto-name result here)

```bash
docker tag dfdockerbitbucket_bitbucket dunkelfrosch/bitbucket:4.1.0
```

5. the result should by a running container and an available local bitbucket image

![](https://dl.dropbox.com/s/iwbxdix94tw1wmj/dc_result_001.png)

## Installation-Method 3, Docker Compose using DB (advanced)
Bitbucket needs a relational DB and for safety reasons we suggest using data-only container features. Take a look inside your *./sample-config* path, we've provided a few sample Docker Compose yaml config files below to show you those feature implementations.

./sample-configs/**docker-compose-dc.yml**
> sample configuration for data-container feature

./sample-configs/**docker-compose-linkdb.yml**
> sample configuration for linking mysql container directly

## container access and maintenance
You can check container health by accessing logs of inner tomcat/bitbucket processes directly as long as the container is still running. As you can see in this screenshot, Atlassian Bitbucket was starting successfully (*Let's ignore some minor warnings ;)* )

```bash
docker logs df-atls-bitbucket
```

![](https://dl.dropbox.com/s/betzx0n620v94ae/dc_logs_001.png)

You can log in easily to your running Bitbucket container to take a deeper look in your Bitbucket service process. *This Bitbucket build provides midnight-commander as terminal extension accessible typing `mc` in your container session shell*.

```bash
docker exec -it --user root df-atls-bitbucket /bin/bash
```

![](https://dl.dropbox.com/s/hznyhy877366p14/dc_term_001.png)


## Contribute

This project is still under development and contributors are always welcome! Please refer to [CONTRIBUTING.md](https://github.com/dunkelfrosch/docker-bitbucket/blob/master/CONTRIBUTING.md) and find out how to contribute to this project.


## License-Term

Copyright (c) 2015 Patrick Paechnatz <patrick.paechnatz@gmail.com>
                                                                           
Permission is hereby granted,  free of charge,  to any  person obtaining a 
copy of this software and associated documentation files (the "Software"),
to deal in the Software without restriction,  including without limitation
the rights to use,  copy, modify, merge, publish,  distribute, sublicense,
and/or sell copies  of the  Software,  and to permit  persons to whom  the
Software is furnished to do so, subject to the following conditions:       
                                                                           
The above copyright notice and this permission notice shall be included in 
all copies or substantial portions of the Software.
                                                                           
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING  BUT NOT  LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR  PURPOSE AND  NONINFRINGEMENT.  IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,  WHETHER IN AN ACTION OF CONTRACT,  TORT OR OTHERWISE,  ARISING
FROM,  OUT OF  OR IN CONNECTION  WITH THE  SOFTWARE  OR THE  USE OR  OTHER DEALINGS IN THE SOFTWARE.