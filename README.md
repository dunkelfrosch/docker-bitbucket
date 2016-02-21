# Docker Image for Atlassian Bitbucket Server 4.2.0

this repository provide the currently latest version of Atlassians sourcecode repository/review software [Bitbucket](https://de.atlassian.com/software/bitbucket) including the recommended [MySQL java connector](http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.36.tar.gz) for an easy and painless docker based Bitbucket installation. Take note that this repository will be used inside our docker atlassian application workbench sources, which are also available on [Github](https://github.com/dunkelfrosch/docker-atlassian-wb) as soon as documentation is completed. *In this workbench we've combined several Atlassian products (JIRA, Confluence and Bitbucket) using advanced docker features like docker-compose based service management, data-container and links*

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)
[![System Version](https://img.shields.io/badge/version-0.9.9-blue.svg)](VERSION)
[![Build Status](https://travis-ci.org/dunkelfrosch/docker-bitbucket.svg?branch=master)](STATUS)


## Preparation
We recommend the [latest Docker version](https://github.com/docker/docker/blob/master/CHANGELOG.md). For simple system integration and supervision we suggest [Docker Compose](https://docs.docker.com/compose/install/). If you're using MacOS or Windows as host operating system, you may take the advantage of [Docker Machine](https://www.docker.com/docker-machine) for Docker's VM management. Bitbucket requires a relational database like MySQL or PostgreSQL, so we'll provide a specific Docker Compose configuration file to showcase both a Bitbucket-MySQL link and a data-container feature configuration. Use the installation guides of provided links down below to comply your Docker preparation process.

[docker installation guide](https://docs.docker.com/engine/installation/)</br>
[docker-compose installation guide](https://docs.docker.com/compose/install/)</br>
[docker-machine installation guide](https://docs.docker.com/machine/install-machine/)</br>


## Installation-Method 1, using docker
These steps will show you the generic, pure docker-based installation of our Atlassian-Bitbucket image/container (the classic approach).  *We also will provide (and recommend) a docker-compose based installation in this documentation (↗ Installation-Method 2)*.

1. checkout this repository

```bash
git clone https://github.com/dunkelfrosch/docker-bitbucket.git .
```

2. build Bitbucket (version 4.2.0) image on your local docker host, naming image "dunkelfrosch/bitbucket:4.1.0"

```bash
docker build -t dunkelfrosch/bitbucket:4.2.0
```

3. start your new Bitbucket application container (opening application port 7990 and internal-ssh port 7999)

```bash
docker run -d -p 7990:7990 -p 7999:7999 dunkelfrosch/bitbucket 
```
	
4. finish your installation using atlassian's browser based configuration
just navigate to `http://[dockerhost]:7990`. Please take note, that your dockerhost will depend on your host system. If you're using docker on your mac, you'll properly using docker-machine or the deprecated native boot2docker vbox image. In this case your 'dockerhost' ip will be the vbox ones (just enter `docker-machine ls` and grab your named machine's ip from there. In my case, (image down below) the resulting setup-ip will be <strong>192.168.99.100:7990</strong> on any other "real" linux system the setup-ip should be 127.0.0.1/localhost (port will be the tomcat application used port 7990)

![](https://dl.dropbox.com/s/jnmr3ejkzm12hdc/dm_start_003.png)

4.1 The following steps will help you to finalize your bitbucket server web-based configuration

4.1.1 Select "external" database from configuration options and choose MySQL as type

![](https://dl.dropbox.com/s/qrp94qfwtsqh4if/bitbucket_setup_001.png)

4.1.2 Enter the following credentials for your database (take note, that the mysql host will be available as your in docker-compose defined service key 'bitbucket_mysql' or by container-name 'df-atls-bitbucket-mysql' directly):
_the initialization and base import of the db will take a while (may be up to 3 minutes)_

![](https://dl.dropbox.com/s/wxc3sc6pnvlg2pd/bitbucket_setup_003.png)

| MySQL Host               | username                   | password            | database            |
|:------------------------ |:-------------------------- |:------------------- |:------------------- |
| df-atls-bitbucket-mysql  | root                       | secretpassword      | bitbucket           |
| (or) bitbucket_mysql     |                            |                     |                     |

4.1.3 Enter your license key

![](https://dl.dropbox.com/s/fe5sqpnshha81ck/bitbucket_setup_004.png)

4.1.4 Create your bitbucket administrator user account and proceed to login (do not connect with JIRA!)

![](https://dl.dropbox.com/s/ta1eyhqyj9ic6nn/bitbucket_setup_005.png)

4.1.5 Welcome to your bitbucket server :)

![](https://dl.dropbox.com/s/fphuadsmh2y2s5n/bitbucket_setup_007.png)


## Installation-Method 2, docker-compose
The following steps will show you an alternative way of your Bitbucket service container installation using Docker Compose (the recommended approach).

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
docker tag dfdockerbitbucket_bitbucket dunkelfrosch/bitbucket:4.2.0
```

5. the result should by a running container and an available local bitbucket image

![](https://dl.dropbox.com/s/iwbxdix94tw1wmj/dc_result_001.png)


## Installation-Method 3, Docker Compose using DB (advanced)
Bitbucket needs a relational DB and for safety reasons we suggest using data-only container features. Take a look inside your *./sample-config* path, we've provided a few sample Docker Compose yaml config files below to show you those feature implementations.

./sample-configs/**docker-compose-dc.yml**
> sample configuration for data-container feature

./sample-configs/**docker-compose-linkdb.yml**
> sample configuration for linking mysql container directly

./sample-configs/**docker-compose-v2.yml**
> sample configuration for the new docker-compose format (valid since version 1.6.n of docker-compose)

I've extend the sample configuration files with corresponding parts of new docker-compose v2 formatted ones (this files will be suffixed by 'v2')


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

Copyright (c) 2015-2016 Patrick Paechnatz <patrick.paechnatz@gmail.com>
                                                                           
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