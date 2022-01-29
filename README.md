# Docker
#### Task 1
> Installing Docker

Followed the steps in the doc: https://docs.docker.com/engine/install/ubuntu/
```sh
apt-get install docker-ce docker-ce-cli containerd.io
```
```sh
root@ip-172-31-39-39:~# apt-cache madison docker-ce
 docker-ce | 5:20.10.12~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.11~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.10~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.9~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.8~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.7~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.6~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.5~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.4~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.3~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.2~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.1~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.0~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.15~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.14~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.13~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.12~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.11~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.10~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.9~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 ```
```sh
apt-get install docker-ce=5:19.03.12~3-0~ubuntu-focal docker-ce-cli=5:19.03.12~3-0~ubuntu-focal containerd.i

root@ip-172-31-39-39:~# docker version
Client: Docker Engine - Community
 Version:           19.03.12
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        48a66213fe
 Built:             Mon Jun 22 15:45:44 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.12
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       48a66213fe
  Built:            Mon Jun 22 15:44:15 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.12
  GitCommit:        7b11cfaabd73bb80907dd23182b9347b4245eb5d
 runc:
  Version:          1.0.2
  GitCommit:        v1.0.2-0-g52b36a2
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```
> Enabling docker swarm
- Master
```sh
root@ip-172-31-39-39:~# docker swarm init
Swarm initialized: current node (xuuppqr3swrfuji0eb3zrgimx) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-4uq134p8grdmathe69n5aml57nmsqeqpjq8qvyz9u1v68kcmmu-0nl2l957irn30i38p7dzljlil 172.31.39.39:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```
- Worker
```sh
root@ip-172-31-35-129:~# docker swarm join --token SWMTKN-1-4uq134p8grdmathe69n5aml57nmsqeqpjq8qvyz9u1v68kcmmu-0nl2l957irn30i38p7dzljlil 172.31.39.39:2377
This node joined a swarm as a worker.
```

#### Task 2
- Running a docker registry container with port 5000 and mounting volume to /opt/registry
> Creating a volume first
```sh
root@ip-172-31-39-39:~# docker volume create myvol
myvol
root@ip-172-31-39-39:~# docker volume ls
DRIVER              VOLUME NAME
local               myvol
```
Followed the steps in the doc: https://docs.docker.com/registry/deploying/
```sh
docker container run -d -p 5000:5000 --restart=always --name registry -v myvol:/opt/registry registry:2
```
```sh
root@ip-172-31-39-39:~# docker container ls -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
027ae2370bd2        registry:2          "/entrypoint.sh /etc…"   23 seconds ago      Up 22 seconds       0.0.0.0:5000->5000/tcp   registry
```
```sh
root@ip-172-31-39-39:~# docker inspect -f "{{ .Mounts }}" 027ae2370bd2
[{volume c639d99b94ba588b89bbfe8428e73df18f0445d6cd62f9d55e6640a1eb5b2054 /var/lib/docker/volumes/c639d99b94ba588b89bbfe8428e73df18f0445d6cd62f9d55e6640a1eb5b
2054/_data /var/lib/registry local  true } {volume myvol /var/lib/docker/volumes/myvol/_data /opt/registry local z true }]
```

#### Task 3
- Building wordpress docker image with Dockerfile,
```sh
# vim Dockerfile

FROM httpd:alpine3.15
  
RUN  mkdir /tmp/website/
 
RUN wget https://wordpress.org/latest.tar.gz -P /tmp/website/

RUN tar -xvzf /tmp/website/latest.tar.gz -C /tmp/website/

RUN cp -R /tmp/website/wordpress/* /usr/local/apache2/htdocs/

RUN rm -rf /tmp/website && rm -rf /usr/local/apache2/htdocs/index.html

RUN apk update && apk add php7 php7-fpm php7-opcache

RUN chown -R www-data:www-data /usr/local/apache2/htdocs/

EXPOSE 8000

CMD [ "httpd-foreground" ]
```
- Pushing it to previously created registry.

Followed the steps in the doc: https://docs.docker.com/registry/deploying/
```sh
root@ip-172-31-45-150:~# docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
word                1                   fee427c986ab        13 minutes ago      275MB
```
```
docker tag word:1 localhost:5000/my-wordpress
```
```
root@ip-172-31-45-150:~# docker push localhost:5000/my-wordpress
The push refers to repository [localhost:5000/my-wordpress]
3f393e4166cd: Pushed 
a94dd5807158: Pushed 
d7b93f4fab60: Pushed 
240aca521514: Pushed 
617a1606ebd2: Pushed 
c0d211d6e438: Pushed 
5bbf31b976dd: Pushed 
5b9bda3e17e8: Pushed 
dede9d4fb2e9: Pushed 
71a62b93fe7b: Pushed 
fc8c77d3c450: Pushed 
83efd5aabbd5: Pushed 
8d3ac3489996: Pushed 
latest: digest: sha256:1090bd2da593ae471987073c0fa831b1a3eeaa8efd9746c4d0425ca0068c48c2 size: 3046
```
```sh
root@ip-172-31-45-150:~# docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
word                1                   fee427c986ab        22 minutes ago      275MB
httpd               alpine3.15          294cecd6c779        5 weeks ago         58.2MB
registry            2                   b8604a3fe854        2 months ago        26.2MB
root@ip-172-31-45-150:~# docker pull localhost:5000/my-wordpress
Using default tag: latest
latest: Pulling from my-wordpress
Digest: sha256:1090bd2da593ae471987073c0fa831b1a3eeaa8efd9746c4d0425ca0068c48c2
Status: Downloaded newer image for localhost:5000/my-wordpress:latest
localhost:5000/my-wordpress:latest

root@ip-172-31-45-150:~# docker image ls
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
word                          1                   fee427c986ab        22 minutes ago      275MB
localhost:5000/my-wordpress   latest              fee427c986ab        22 minutes ago      275MB
httpd                         alpine3.15          294cecd6c779        5 weeks ago         58.2MB
registry                      2                   b8604a3fe854        2 months ago        26.2MB
```

#### Task 4
Followed the steps in the doc: https://docs.docker.com/engine/swarm/stack-deploy/
```sh
docker service create --name registry -p 5000:5000 registry:2

root@ip-172-31-39-39:~# docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
snv88rpxrgfa        registry            replicated          1/1                 registry:2          *:5000->5000/tcp
```
```sh
# vim docker-compose.yml

---
version: "3"

services:
  web:
    image: localhost:5000/stack_wordpress
    build: .
    container_name: wp
    restart: always
    volumes:
       - wpvol:/usr/local/apache2/htdocs/
    networks:
       - wordsite
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wp
      - WORDPRESS_DB_PASSWORD=wp
      - WORDPRESS_DB_NAME=wp
    ports:
      - "8000:8000"

  mysql_db:     
    image: mysql:latest
    container_name: db
    restart: always
    volumes:
       - dbvol:/var/lib/mysql/
    networks:
       - wordsite
    environment:
      - MYSQL_ROOT_PASSWORD=root@123
      - MYSQL_DATABASE=wp
      - MYSQL_USER=wp
      - MYSQL_PASSWORD=wp

volumes:
  wpvol:
  dbvol:
    
networks:
  wordsite:
```
> Testing with compose
```sh
root@ip-172-31-39-39:~/test# docker-compose ps
Name             Command             State               Ports             
---------------------------------------------------------------------------
db     docker-entrypoint.sh mysqld   Up      3306/tcp, 33060/tcp           
wp     httpd-foreground              Up      0.0.0.0:8000->80/tcp, 8000/tcp
```
> To distribute the across the swarm, pushing to the registry 
```sh
root@ip-172-31-39-39:~/test# docker-compose push
Pushing web (localhost:5000/stack_wordpress:latest)...
The push refers to repository [localhost:5000/stack_wordpress]
db6f33a2399a: Pushed
871fb2fc98ca: Pushed
515946b559b7: Pushed
3fcc6a2de9c8: Pushed
fd582f860395: Pushed
ece50c962310: Pushed
dbcf9b713498: Pushed
5b9bda3e17e8: Pushed
dede9d4fb2e9: Pushed
71a62b93fe7b: Pushed
fc8c77d3c450: Pushed
83efd5aabbd5: Pushed
8d3ac3489996: Pushed
latest: digest: sha256:a888aa4ce377d2d08ff03568ace166a7005fed73b4db744f908c5a0b6a3bf534 size: 3046
```

- Deploying Stack
```sh
docker stack deploy --compose-file docker-compose.yml stack_word
```
```sh
root@ip-172-31-39-39:~/test# docker stack services stack_word
ID                  NAME                  MODE                REPLICAS            IMAGE                                   PORTS
ejytrq2dl6ph        stack_word_web        replicated          1/1                 localhost:5000/stack_wordpress:latest   *:8000->80/tcp
mefbitfd5bd8        stack_word_mysql_db   replicated          1/1                 mysql:latest 
```

# Ansible
- Creating an Ansible role and a playbook to automate Docker tasks 3 and 4.
```sh
/root/ans/
└── main.yml
```
```sh
root@ip-172-31-39-39:/etc/ansible/roles# ansible-galaxy init wordpress
- Role wordpress was created successfully
```
```sh
/etc/ansible/roles/wordpress
├── README.md
├── defaults
│   └── main.yml
├── files
│   ├── Dockerfile
│   └── docker-compose.yml
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   └── main.yml
├── templates
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml
```

> vars file
```sh
# Vim /etc/ansible/roles/wordpress/vars/main.yml
--
# vars file for wordpress
image_name: "localhost:5000/stack_wordpress"
```

> main file
```sh
# vim main.yml

#################### Playbook for building Image ###########################
- name: "Docker Image/Build and Image/Push"
  hosts: localhost
  become: true
  roles:
    - wordpress
```

> tasks
```sh
vi /etc/ansible/roles/wordpress/tasks/main.yml

---
# tasks file for wordpress

- name: "Building image and pushing"
  docker_image:
    source: build
    build:
path: "/etc/ansible/roles/wordpress/files/"
    name: "{{ image_name }}"
    tag: "latest"
    push: true
    force_source: yes

- name: "Build-Setp - removing image"
  docker_image:
    state: absent
    name: "{{ image_name }}"
    tag: "latest"

- name: Deploy stack from a compose file
  docker_stack:
    state: present
    name: stack_wordpress
    compose:
      - /etc/ansible/roles/wordpress/files/docker-compose.yml
```

#### Output

> Running Playbook
```sh
root@ip-172-31-39-39:~/ans# ansible-playbook main.yml
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Docker Image/Build and Image/Push] *********************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [localhost]

TASK [wordpress : Building image and pushing] ****************************************************************************************************************
ok: [localhost]

TASK [wordpress : Build-Setp - removing image] ***************************************************************************************************************
changed: [localhost]

TASK [wordpress : Deploy stack from a compose file] **********************************************************************************************************
changed: [localhost]

PLAY RECAP ***************************************************************************************************************************************************
localhost                  : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

```sh
root@ip-172-31-39-39:~/ans# docker service ls
ID                  NAME                       MODE                REPLICAS            IMAGE                                   PORTS
toaes9sr5uap        registry                   replicated          1/1                 registry:2                              *:5000->5000/tcp
w9kgob0wf8ex        stack_wordpress_mysql_db   replicated          1/1                 mysql:latest                            
l28cw3635sut        stack_wordpress_web        replicated          1/1                 localhost:5000/stack_wordpress:latest   *:8000->80/tcp
```

> Stack deploy
```sh
root@ip-172-31-39-39:~/ans# docker stack services stack_wordpress
ID                  NAME                       MODE                REPLICAS            IMAGE                                   PORTS
l28cw3635sut        stack_wordpress_web        replicated          1/1                 localhost:5000/stack_wordpress:latest   *:8000->80/tcp
w9kgob0wf8ex        stack_wordpress_mysql_db   replicated          1/1                 mysql:latest
```


