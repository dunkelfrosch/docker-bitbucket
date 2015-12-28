# Docker Image for Atlassian Bitbucket Server 4.1.0

*this documentation isn't finally done yet - we still working on major and minor issues corresponding to this repository base!*

this repository provide the currently latest version of Atlassians sourcecode repository/review software [bitbucket](https://de.atlassian.com/software/bitbucket) including the recommended [MySQL java connector](http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.36.tar.gz) for an easy and painless docker based bitbucket installation. Take note, that this repository will be used inside our docker atlassian application workbench sources (also available on Github as soon as our documentation stands stable and "readable"). *In this workbench we've combine additional atlassian products (bitbucket, bitbucket and bitbucket) using advanced docker features like docker-compose based service management, data-container and links*

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)
[![System Version](https://img.shields.io/badge/version-0.9.7-blue.svg)](VERSION)
[![Build Status](https://travis-ci.org/dunkelfrosch/docker-bitbucket.svg?branch=master)](https://travis-ci.org/dunkelfrosch/docker-bitbucket)

## Preparation
we recommended the usage of the latest version docker and for simple system integration/supervision docker's "in-house" application docker-compose.
If you're using MacOS or Windows as host operating system, you may take the advantage of docker-machine for docker's vm management. Jira needs
a relational database like MySQL or PostgreSQL linked to. We'll provide a spec.
MySQL container configuration inside this documentation beside a docker-compose sample yaml config file, to show container linking feature. Use the installation guides of provided links down below to comply your docker pre-peration process.

[docker installation guide](https://docs.docker.com/engine/installation/)</br>
[docker-compose installation guide](https://docs.docker.com/compose/install/)</br>
[docker machine installation guide](https://docs.docker.com/machine/install-machine/)</br>


## Installation-Method 1, docker direct mode
as long as our image isn't available via docker.io hub repository, you've to build it by yourself using this github repository. this steps will show you the generic, pure docker based installation of our bitbucket image container, without any database container linked or data-container feature.  *We also will provide a docker-compose based installation in this documentation*.

1. checkout this repository

```bash
git clone https://github.com/dunkelfrosch/docker-bitbucket.git .
```

2. build bitbucket (version 4.1.0) image on your local docker host, naming image "df/bitbucket:4.1.0"

```bash
docker build -t df/bitbucket:4.1.0
```

3. start your new bitbucket application container

```bash
docker run -d -p 7990:7990 df/bitbucket 
```
	
4. finish your installation using atlassian's browser based configuration 
just navigate to `http://[dockerhost]:7990` 


## Installation-Method 2, docker-compose (simple)
this steps will show you an alternative way of bitbucket service container installation using docker-compose

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

4. (optional) rename the resulting image after successful build (we'll use our image auto-name result here)
```bash
docker tag dfdockerbitbucket_bitbucket df/bitbucket:4.1.0
```
5. the result should by a running container and an available local bitbucket image

![](https://dl.dropbox.com/s/iwbxdix94tw1wmj/dc_result_001.png)

## Installation-Method 3, docker-compose (using db)
bitbucket needs a relational db and may feel more self-confident using data-only container features. Take a look inside your *./sample-config* path, we've provide a few sample docker-compose yaml config files to show you those feature implementations.

./sample-configs/**docker-compose-dc.yml**
> sample configuration for data-container feature

./sample-configs/**docker-compose-linkdb.yml**
> sample configuration for linking mysql container directly

## container access and maintenance
you can check container health by accessing logs of inner tomcat/bitbucket processes directly as long as the container is still running. as you can see in this screenshot, atlassian bitbucket was starting successfully (*we've to ignore some by-side warnings ;)* )

```bash
docker logs df-atls-bitbucket
```

![](https://dl.dropbox.com/s/betzx0n620v94ae/dc_logs_001.png)

you can login easily to your running bitbucket container to take a deeper look in your bitbucket service process. *this bitbucket build provide midnight-commander as terminal extension accessable typing "mc" in your container session shell*.

```bash
docker exec -it --user root df-atls-bitbucket /bin/bash
```

![](https://dl.dropbox.com/s/hznyhy877366p14/dc_term_001.png)


## Contribute

DockerConfluence is still under development and contributors are always welcome! Feel free to join our docker-bitbucket distributor team. Please refer to [CONTRIBUTING.md](https://github.com/dunkelfrosch/dfdockerbitbucket/blob/master/CONTRIBUTING.md) and find out how to contribute to this Project.


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