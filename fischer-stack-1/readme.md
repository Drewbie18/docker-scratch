# Creating first Stack

This is using Docker Toolbox on windows 10 home. 

## Create Two Node Swarm (local machine)

### Create Docker Machines

- Command: "docker-machine create --driver virtualbox vm1"
- Command: "docker-machine create --driver virtualbox vm2"

### Initialize Swarm
- Wait until both vm's are initialized.
- Get the information on the running vm's, command: "docker-machine ls"
- Result should be something like: 

|Name|Active|Driver|State|URL|SWARM|Docker|Errors|
|----|------|------|-----|---|-----|------|------|
|vm1 |-|virtualbox|running|tcp://192.168.99.100:2376|-| v17.12.0-ce|-|
|vm2 |-|virtualbox|running|tcp://192.168.99.101:2376|-| v17.12.0-ce|-| 

- Initialize swarm on vm1: docker swarm init --advertise-addr 192.168.99.100
- Shold get a token notification reading something like: "docker swarm join --token SWMTKN-1-3c9nib6dhaeldin63rv2r4mihe6zf4dl8u9avj9i5189koucoz-db4vm53ou6ujn9420mwoh78zt 192.168.99.100:2377"
- Run that command in vm2. Should get a response as "This node joined swarm as worker"



# Adding Files to Vm's

In order to deploy docker stack files to a Virtualbox vm on your local machine you may want to copy files from your machine (host) to the vm's. Here are some options. 

## Docker Machine SCP

### Git Bash SCP
- Install git, should come with git bash
- Command: scp ./foo.txt vm1:/home/docker

### PowerShell
- Add scp.ext to you PATH in windows
- command scp foo.txt docker@192.168.99.100:/home/docker
- the default password for the docker-machine vm is username: docker, password: tcuser


