# docker-swarm

# Run a Docker Swarm in the AWS Cloud Console 

## Important Documentations

1. https://get.docker.com/
2. https://www.cloudping.info/
3. https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:v=3;$case=tags:true%5C,client:false;$regex=tags:false%5C,client:false

## Steps to create a docker swarm on AWS

### Create the Leader node EC2 instance in our AWS cloud console
1. Selecte the required Amazon Machine Image -- I'm selecting the ubuntu image
2. Select the instance type.
3. Selete the Security Group to support the HTTP, TCP protocols.
4. Download the key-pair and launch the instance.
   Download the ppk file to be used on Putty.
5. Name this EC2 as the Docker-swarm [as it's going to be our master node]

### Create the 2 worker nodes in AWS EC2.
1. Create the EC2 instance following the above steps.
2. Set the tags as node1 and node2.

## Steps to Create the Node using Putty
1. Open the .ppk using Putty
2. Set the Public IP of the instance in the Session as Host name 
3. Name the session, save and load it.
4. Click Open
5. Login as "ubuntu" and install the docker in this node.

#### This step remains same for all the other nodes as well.

## Steps to Create the Leader
1. Write the following command in the Leader container. Here named as "Docker-swarm"
   sudo docker swarm init --advertise-addr <public ip address>
   After this command we get the swarm token.

## Steps for nodes to join the Manger
1. Copy paste the above command on each node and run
2. The nodes will join the swarm and eventually become the worker nodes.

## Commands to run the swarm

1. curl -fsSL https://get.docker.com -o install-docker.sh

2. sh install-docker.sh --dry-run

3. sudo sh install-docker.sh

4. sudo docker info | less

5. sudo docker swarm init --advertise-addr <public ip address>


6. docker swarm join --token <token> <ip address>
7. To leave the swarm -  sudo docker swarm leave

//Type the following commands in the Leader cmd
8. To check the nodes within the swarm ---
 sudo docker node ls

9. To make the node1 the manager -- 
sudo docker node update --role manager <Hostname ip-address of node1>

To make the node2 the manager -- 
sudo docker node update --role manager <Hostname ip-address of node2>

Now to run services in the Leader
sudo docker service create --replicas 3 alpine ping 8.8.8.8
sudo docker service ls
sudo docker service ps <name of the service>
sudo docker container ls



