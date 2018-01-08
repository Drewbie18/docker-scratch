# Create Docker Swarm

## Launch VM's

- docker-machine create --driver virtualbox vm1
- docker-machine create --driver virtualbox vm2

## Intialize Swarm 
- Get IP address: docker-machine ls
- docker-machine ssh vm1
- docker swarm init --advertise-addr <IP>

This will give the command to input into vm2 to join the sarm. In new 
window. 

- docker-machine ssh vm2 
- copy command to join the smarm that vm1 is the manager of. 
- Verify they have joined "docker node ls" from leader node (vm1)

## Create Networks the APP will use 

- docker network create --driver overlay frontend
- docker network create --driver overla backend

## Create Vote Service

- image: dockersamples/examplevotingapp_vote:before
- web front end for users to vote dog/cat
- ideally published on TCP 80. Container listens on 80
- on frontend network
- 2+ replicas of this container
### Command
- docker service create --name vote -p 80:80 --replicas 2 --network frontend dockersamples/examplevotingapp_vote:before

## Create redis Service
- redis:3.2
- key/value storage for incoming votes
- no public ports
- on frontend network
- 1 replica NOTE VIDEO SAYS TWO BUT ONLY ONE NEEDED
### Command 
- docker service create --name redis --network frontend redis:3.2

## Create 'db' service
-image:  postgres:9.4
- one named volume needed, pointing to /var/lib/postgresql/data
- on backend network
- 1 replica

### Command
- docker service create --name db --network backend --mount type=volume,src=db-data,target=/var/lib/postgresql/data postgres:9.4


## Create 'worker' service

- image: dockersamples/examplevotingapp_worker
- backend processor of redis and storing results in postgres
- no public ports
- on frontend and backend networks
- 1 replica

### Command:
- docker service create --name worker --network frontend --network backend dockersamples/examplevotingapp_worker

## Create 'result' Service
- image:  dockersamples/examplevotingapp_result:before
- web app that shows results
- runs on high port since just for admins (lets imagine)
- so run on a high port of your choosing (I choose 5001), container listens on 80
- on backend network
- 1 replica

### Command
- docker service create --name result --network backend -p 5001:5001 dockersamples/examplevotingapp_result:before