# docker
Just for memo


# DOCKER ESSENTIAL

Created time: December 28, 2022 3:58 PM
Last edited time: January 5, 2023 10:18 AM
Tags: Guides, docker, memo

# DOCKER RESUM  ( CONTAINER - VIRTUALISATION )

1 Docker machine

2 Docker compose ( Dockerfile - docker-compose.yml )

3 Docker  SWARM

# EssentieL

```bash
# docker compose
	docker-compose up -d
# Copy from Hote TO Container ( boubacar@zii:~/Desktop/Docker_Symfony/project$ )
	docker cp ./ CONTAINER:/var/www/project/
docker cp ./ CONTAINER:/var/www/
# Copy from Container TO Hote 
	docker cp CONTAINER:/var/logs/ /tmp/app_logs
# add CHANGE OWN FOLDERS & FILES
	 sudo chown -R $USER ./

# Login Container
	docker exec -it CONTAINER /bin/bash
							# OR
	docker exec -it CONTAINER bash

# Edit .env for symfony
	DATABASE_URL="mysql://root:password@db:3306/db_test"
	MAILER_DSN=smtp://localhost:1025

# >> **Nettoyage du système avec**
docker system prune
```

# MANAGE IMAGES

![gere-images.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/gere-images.png)

```bash
	docker |grep image
# List images
	docker images
# Search page by page
	docker search ngix|more
# Recupere  image from repo ou registre
	docker pull docker.io/nginx
# SHOW images
	docker images
# TAGGER image
	docker tag docker.io/nginx 4096autos/4096autos:nginx
# Push images
	docker push 4096autos/4096autos:nginx
# SEARCH from repository "4096autos/4096autos"
	docker search 4096autos/4096autos

# Save in tmp for exemple nginx_4096autos.tar
	docker save 4096autos/4096autos:nginx -o /tmp/nginx_4096autos.tar
# Restore(charger image) from /tmp/nginx_4096autos.tar
	docker load -i /tmp/nginx_4096autos.tar

# Delete image
	docker rmi -f 1403e55ab369
```

# REGISTRY LOCAL

![hub-local.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/hub-local.png)

```bash
# Docker-registry-local
	docker search registry
# Make 
	docker run -d -p 5000:5000 --restart=always --name=registry registry:2
# Pull imge ubuntu
	docker pull ubuntu
# Tag image ubuntu
	docker tag docker.io/ubuntu localhost:5000/ubuntu_local
# Push ubuntu_local on Hub-local(registry-local)
	docker push localhost:5000/ubuntu_local
# Show images
	docker images
```

# DOCKER CONTAINER

![CONTAINER.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/CONTAINER.png)

![RUN-CONTAINER.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/RUN-CONTAINER.png)

```bash
# Make container
	docker run centos
# show container
	docker ps
# show hide container
	docker ps --all
# Make container centos with option i=interactive t=terminal
	docker run -it centos bash
# log in container
	docker attach container_id
# start / stop container
	docker start container_id
	docker stop -f container_id
# Install apache on centos
	docker run --name web_server centos /bin/bash -c "yum install -y httpd"
# Install apache on centos -d on background
	docker run -d --name web_server_00 centos /bin/bash -c "yum install -y httpd"
# Log into Container web_server_00
	docker attach
# Delete docker container
	docker rm container_id
```

# MAP PORT

![map-port.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/map-port.png)

```bash
# Map port
	docker run -d -p 8080:80 —name web00 nginx
# CMD
	docker exec web00 /bin/bash -c 'echo "<H1>Mon CT Nginx</H1>" >/usr/share/nginx/html/index.html'
	
	docker run -d -p 8000:80 —name web02 nginx
	docker exec web00 /bin/bash -c 'echo "<H1>Mon CT Nginx Web 02</H1>" >/usr/share/nginx/html/index.html'
```

# MANAGE DATA SIZE

![manage-data-size.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/manage-data-size.png)

MAP docker-container-directory AT host--directory

```bash
# Run docker
	# hote-IP 192.168.1.109:8080  |
																#  /data/web/
	# hote-IP 192.168.1.109:8000  |
	
	docker run -d --name web -p 8000:80 -v /data/web:usr/Share/nginx/html/ nginx

	
```

# LIAISON INTER CONTAINERS (deprecied)

## Link-legacy

![link-legacy.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/link-legacy.png)

### Link training/webapp to training/postgres

```bash
#  
	docker run -d --name db training/postgres

	docker run -d --name web --link db -P training/webapp python app.py 
#
#
	docker inspect
# Cat for cheicking contains
	docker exec web cat /etc/hosts
	docker exec db  cat /etc/hosts
# show link between web and db address's contains
docker exec web env
	
```

# INSPECT STATS ( INSPECTER LES STATISTIQUES )

```bash
#Contaire list
	docker ps
# Inspect container
	docker inspect container |more
# Filtre Address Mac
	docker inspect --format='{{range .networkSettings.Networks}} {{.MacAddress}} {{end}}' container 
# Filtre IP
	docker inspect --format='{{range .networkSettings.Networks}} {{.IPAddress}} {{end}}' container 
# Filtre Name
	docker inspect --format='{{.Name}}' container
	docker inspect --format='Le container TEST {{.Name}}' container
```

# exec et attach

```bash
# Container List
 docker ps
# Ouvrir une shell dans un container
	docker attach attach_exce
# TO out without stop Container
	Ctrl+p +q
# Executed cmd out container
	docker exec container cmd
	docker exec ubuntu    ifconfig eth0
```

# Commit Container

```bash
# Start a container with new mane on port 8080:80
	docker run -d --name web_test -p 8080:80 ngnix
# Execute cmd " edite file "
	docker exec web_test bash -c 'echo " <h2>bienvenue sur TEST</h2>" > /urs/share/ngix/html/index.html'
# Check on browser
	My_local_IP_address:Port
# Commit container and tag tag image ( New image personalized)
	docker commit id_container localhost:5000/nginx_test
# LIST images and RUN new container after commit
	docker images
	docker run -d -p 8009:80 localhost:5000/nginx_test
# Check CONTAINERS on, run 
	docker ps	
```

# BUILD IMAGES - Dockerfile

![build-image.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/build-image.png)

## Dockerfile

```bash
# create image
FROM centos
# author
MAINTAINER boubacar <bsdiakite123@gmail.com>
# INSTALL web-tool and EDIT FILE index.html
RUN yum install -y https net=tools && \
echo "<h2>Welcome</h2>" > /usr/share/httpd/noindex.index.html
# open Prot 80
EXPOSE 80
# Surcharge cmd web-server with option
CMD ["-D","FOREGROUND"]
# Start web-server
ENTRYPOINT ["/usr/sbin/httpd"]
```

## BUILD with tag image

```bash
docker build --tag:**localhost:8000/httpd_test** .
```

## RUN a Container with my image

```bash
docker run -d -p 8009:80 **localhost:8010/htppd_test**
```

## CHECK with CURL and BROWSER

```bash
# CURL
	curl -l http://localhost:8010
# BROWSER
	http://localhost:8010
```

# BRIDGE, HOST, NULL

![NETWORK.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/NETWORK.png)

```bash

# run NEW CONTAINER
	docker run -it --name network busybox
# Show me my_ip_address
	if config
# Check container on run
	docker ps
# show me brigde
# SHOW me	
ip ad 
	docker network ls
# To use brigde
	docker run -it --name host --network host busybox
# To connect on NETWORK
	docker network [option]
# Create OTHER network
	docker network create RED
# SHOW networks
	docker network ls
	ip ad

# CREATE NEW NETWORK with bridge BLUE
	docker create --driver bridge BLUE
# SHOW ME brigdes
	docker network ls
# NEw Container
	docker run -it --name red_net --network RED busybox
	# Show me Ethernets on container red_net
		ifconfig
# Inpect NETWORK
	docker network inspact RED
	docker inspect red_net

# CREATE NEW NETWORK with bridge BLUE
	docker create --driver bridge BLUE
# SHOW ME brigdes
	docker network ls
# NEw Container
	docker run -it --name blue_net --network blue busybox
	# Show me Ethernets on container red_net
		ifconfig

```

# DOCKER MACHINE  ON  DEBIAN

[https://github.com/docker/machine/releases](https://github.com/docker/machine/releases)

```bash
# On Linux 
	curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
  chmod +x /tmp/docker-machine &&
  sudo cp /tmp/docker-machine /usr/local/bin/docker-machine 
# Show me infos
	docker-machine version
```

# DOCKER-COMPOSE

Allows to define and run multi-container docker apps under services

TO INSTALL : [https://github.com/docker/compose/releases](https://github.com/docker/compose/releases)

```bash
# Install docker-compose
	sudo apt install docker-compose
	
# Execute services
	docker-compose up
```

## Compose Service

```bash
# Make directory && create a file into it
	mkdir app && cd app && nano docker-compose.yml
```

```yaml
version: '2.0'

services:
	web:
	  container_name: server-web
	  image: nginx
	  port:
	  - 8000:80
	db:
		container_name: server-db
		image: mysql
		ports:
			- 3306:3306
		environment:
			- MYSQL_ROOT_PASSWORD=test	

	 
```

```bash
# check file
	docker-compose config

# load / create
	docker-compose up -d
# load only service db
	docker-compose up -d --no-deps db

# Show me
	docker-compose ps

# STOP and DELETE 
	docker-compose stop
	docker-compose rm

# Exec Shell on server-web
	docker-composer exec web /bin/bash
	# On Container install iputils-ping
		apt update && apt install -y iputils-ping
	# ping on db
		ping db

```

### NETWORK

```bash
# Check Network
	docker network ls
# Inspect Network
	docker network inspect app_default
# Ping my container for test
	ping container_ip

# Check log
docker-compose logs

**# STOK and DELETE CONTAINERS & NetworkS**
	docker-compose down

	
```

## DOCKER-COMPOSE : BUILD ( Dockerfile |  docker-compose.yml)

**Dockerfile** 

```bash
FROM ubuntu
RUN apt update && \
		apt install -y iputils-ping
CMD ["localhost"]
ENTRYPOINT ["ping"]
```

**docker-compose.yml**

```yaml
version: '2'

services:
	ping:
		build: .
```

**CMD on terminal**

```bash
# Load
	docker-compose up
# Show compose images
	docker-compose images

# Delete images
	docker-compose rm

```

## DOCKER-COMPOSE : VOLUMES (Dockerfile | docker-compose.yml)

**Dockerfile** 

```bash
FROM ubuntu
RUN apt update && \
		apt install -y iputils-ping
CMD ["localhost"]
ENTRYPOINT ["ping"]
```

**docker-compose.yml**

```yaml
version: '2'

services:
	web:
		image: nginx
		ports:
			-8000:80
		volumes: 
			- web-file:/usr/share/nginx/html
volumes:
	web-file:
		driver: local
		driver_opts:
			type: nfs
			o: addr=192.168.99.102
			device:":files/web-files"
```

**CMD on terminal**

```bash
# Load
	docker-compose up -d
# Show compose images
	docker-compose ps

# Add something in web-page
	docker-compose exec web bash
	# In container "web"
		echo "Welcome" >/usr/share/nginx/html/index.html &&	exit

# Show me Volumes
	docker volume ls
# Inspect Volume
	docker volume inspect app_web-file
# Manager ALLOW for users
	Add user on docker:group
		sudo setfacl -m g:docker:rw /files/web-files/*  // en prod 
	/var/lib/docker/volumes
# More information 
	github.com/ContainX/docker-volume-netshare

# Delete link
	docker-compose down -v 
# Down for test
	docker-composer down

# Delete images
	docker-compose rm

```

# DOCKER-COMPOSE : RESEAUX (Dockerfile | docker-compose.yml)

**Dockerfile**

```bash
FROM ubuntu
RUN apt update && apt install -y iptils-ping
COPY ./entrypoint.sh /Usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
CMD /usr/bin/entrypoint.sh
```

**docker-compose.yml**

```yaml
version: '2'

services:
	web:
		image: nginx
		ports:
			-8000:80
		volumes: 
			- web-file:/usr/share/nginx/html
		networks:
			- front-web

	db: 
		image: mysql
		ports:
			-3306:3306
		volumes:
			- db-file:/var/lib/mysql
		environment:
			- MYSQL_ROOT_PASSWORD=root
		networks:
			- back-web

		ping:
			build: .
			network:
				- front-web	
				- back-web		

volumes:
	web-file: {}
	db-file: {}

networks:
	front-web:
		driver: brigde
		ipam:
			config:
				- subnet: 172.20.0.0/24
	back-web:
		driver: brigde
		ipam:
			config:
				- subnet: 172.21.0.0/24
```

**CMD on terminal**

```bash
# check and Load
	docker-compose config
	docker-compose up
# Show compose images
	docker-compose ps

# Show me network
	docker-compose network ls
# Inspect Network
	docker network inspect
		docker network inspect app_default

# Load
	docker-compose up -d
# NET-TOOLS on container "ping"
	apt update &&	apt install net-tools
# Show compose images
	docker-compose ps
# Show me log
	docker network log -f

# Delete link
	docker-compose down -v 
# Down for test
	docker-composer down

# Delete images
	docker-compose rm

```

# DOCKER-COMPOSE : LOGS (Dockerfile | docker-compose.yml)

**Dockerfile**

/web/**docker-compose.yml**

```yaml
version: '2.1'

services:
	web:
		container_name: web-srv
		image: nginx
		volumes: 
			- web-file:/usr/share/nginx/html
		ports:
			-8000:80
		logging:
			driver: gelf
			options:
				gelf-address; "udp://localhost:12201"		
	networks:
		web-conf: {}
		web-conf: {}

```

**CMD on terminal**

```bash
# Show compose images
	docker-compose ps
# LOGS
	docker-compose logs --follow	
# check and Load
	docker-compose config
	docker-compose up -d
# Show compose images
	docker-compose ps

# LOGS
	docker-compose logs
# EDIT LOG (/etc/rsyslog.conf)
	# provides UDP syslog reception
	module(load="imudp")
	input(type="imudp" port="514")
	systemctl restart rsyslog.service
# Show me log
	tail -f /var/log/syslog

# EDIT /etc/rsyslog.d/50-default.conf
# SEARCH on NET interface Elasticsearch or ELK (Elasticsearch Logtail Kibana) 

# Delete link
	docker-compose down -v 
# Down for test
	docker-composer down

# Delete images
	docker-compose rm

```

**cd /web/elk/  && cat docker-compose.yml (ELK)**

```yaml
elastucsearch:
	image: elasticsearch:2.1.1
	volumes:
					- /srv/elasticsearch/data:/usr/share/elasticsearch/data
	ports:
					- "9200:9200"
logstash:
		image: logstash:2.1.1
		environment:
					TZ: Europe/Paris
		expose:
					- "12201:12201"
		ports:
					- "12201:12201"
					- "12201:12201/udp"
		volumes:
					- .conf:/conf
		links:
					- elasticsearch:elasticsearch
		command: logstash -f /conf/gelf.com

kibana:
		image: kibana:4.3
		links:
						- elasticsearch:elasticsearch
		ports:
						- "5601:5601"
# ela | kibana | logs
```

**CMD on terminal**

```bash
# LOGS
	docker-compose logs

# EDIT LOG (/etc/rsyslog.conf)
	# provides UDP syslog reception
	module(load="imudp")
	input(type="imudp" port="514")
	systemctl restart rsyslog.service
# Show me log
	tail -f /var/log/syslog

# EDIT /etc/rsyslog.d/50-default.conf
# SEARCH on NET interface Elasticsearch or ELK (Elasticsearch Logtail Kibana) # ela | kibana | logs
# connect nginx to logstash via port
# Edit /web/elk/confi/gelf.conf
	input {
		gelf {
			type => docker
			port => 12201
		}
	}
	
	output {
		elasticsearch {
			hosts => elasticsearch
		}
	}

# check and Load
	docker-compose config
	docker-compose up -d
# Show compose images
	docker-compose ps

# Delete link
	docker-compose down -v 
# Down for test
	docker-composer down

# Delete images
	docker-compose rm

```

# MODE SWARM ( SERVICE, LEADER, WORKER, STACK )

## STACK ( COMPOSE  - SWARM - CLUSTERS MANAGERS )

![Stack.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/Stack.png)

## SERVICE

![Service.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/Service.png)

### Initialize SWARM ( CLUSTERS )

![Swarm-cycle.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/Swarm-cycle.png)

```bash
# Show me Swarmms status
	docker info [ grep -i swarm

# initialize a swarm with local IP_address
	docker swarm init --advertise-addr 192.168.99.102
# Show me Swarmms status
	docker info [ grep -i swarm

# Show me swarms List
	docker node ls

# Show me more option for swarm
	docker swarm
```

### SWARM JOIN  ( LEADER, WORKER, NODE )

![swarm-join.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/swarm-join.png)

```bash
# login to **worker-00**
	ssh worker-00
# copy and execute token from previous swarm 
	docker swarm joint ...

# login to **worker-01**
	ssh worker-01
# copy and execute token from previous swarm 
	docker swarm joint ...

# Show me node (join)
	docker node ls
```

### NODE MANAGEMENT ( LEADER, WORKER, NODE )

```bash
# show me nodes 
	docker node ls
# Inspect
	docker node inspect worker00 |more
# Inspect specific (exemples)
	docker node inspect worker00 --format "{{ .Status.addr }}"
	docker node inspect worker00 --format "{{ .Status.state }}"
	docker node inspect worker00 --format "{{ .Spec.Role }}"
	docker node inspect worker00 --format "{{ .Spec.Labels }}"

# show me nodes containers
	docker node ps
	
# Promote node like manager (Reachable)
	docker node promote worker01
# demote node like No Manager status
	docker node demote worker01

# show me nodes
	docker node ls

# show me more option for node
	docker node

```

## SERVICE

![Service.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/Service.png)

```bash
# Service option
	docker service
# docker service
	docker service create --publish 8080:80 --name web nginx
# Show me services
	docker service ls
# Where is it ?
	docker service ps web

# Create 3 Replicas
	docker service create --publish 8088:80 --name web-00 --replicas 3 nginx
# Show me services and Replicas
	docker service ls 
# Where are My 3 Replicas ?
	docker service ps web-00
# Show me containers
	docker container ps 

	# In container worker00 : web-00.3....
		docker container ps

# show web-00
	docker container ps web-00
# show me replais web
	docker service ls

# For more replicas
	docker service scale web web-00=6
# Show replicas
	docker service ps web-00
			
# For less replicas
	docker service scale web web-00=3
# Show replicas
	docker service ps web-00
```

### service life cycle

```bash
# show me services (docker service "option")
	docker service ls
	docker service ps web-00
	docker service inspect web-00 | more
# Delete service
	docker service rm web
```

## COMPOSE STACK

### Service

### stack

### docker-compose

![Stack.png](DOCKER%20ESSENTIAL%200d949f37bf1b4a818b900e10b68edd2a/Stack.png)

### Edit docker-compose.yml

```yaml
version:'3'

services:
	web:
		images: nginx
		volumes:
			- "web-conf:/etc/nginx/conf.d"
			- "web-file:/usr/share/nginx/html/"
		ports:
			- 8080:80
		logging:
			driver: gelf
			options:
				gelf-address: "udp://localhist:12201"
			deploy:
				mode: replicated
				replicas: 3
				placement:
					constraints: [node.role == worker]
	db:
		image: mysql
		ports:
			- 3306:3306
		environment:
			- MYSQL_ROOT_PASSWORD=root
		deploy:
			mode: replicated
			replicas: 3
			placement:
				constraintes: [node.role == worker]
volumes:
	web-conf: {}
	web-files: {}
```

### **on terminal**

```bash
# Manager docker stacks on folder "/web"
	docker stack
	docker stack COMMAND
# Deploye stack
	docker stack deploy --help

#														CMD          FILE           Stack-name  
	docker stack deplay --compose-file docker-compose.yml web

# Show services
	docker stack ls
# Show me more on "web" service
	docker stack ps web

```

### Stack life cycle

```bash
	docker stack

	docker stack CMD
								
	docker stack rm name
```
