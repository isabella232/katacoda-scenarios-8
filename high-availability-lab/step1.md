A quick reminder, Scylla offers high availability by replicating data across multiple nodes. The Replication Factor (RF) is equivalent to the number of nodes where data (rows and partitions) are replicated. Data is replicated to multiple (RF=N) nodes.
An RF of one means there is only one copy of a row in a cluster, and there is no way to recover the data if the node is compromised or goes down. RF=2 means that there are two copies of a row in a cluster. An RF of at least three is used in most systems.
Data is always replicated automatically. Read or write operations can occur to data stored on any of the replicated nodes.
The Consistency Level (CL) determines how many replicas in a cluster must acknowledge a read or write operation before it is considered successful.

In this part, you’ll bring up a three-node cluster with a Replication Factor (RF) set to 3. You'll also create a keyspace, a table, write some data and then read it. 



## Task

First, you'll bring up a three-node Scylla cluster using Docker. Sstart with one node, called Node_X:

`docker run --name Node_X -d scylladb/scylla:4.3.0`{{execute}}
 
Create two more nodes, Node_Y and Node_Z, and add them to the cluster of Node_X. The command “$(docker inspect –format='{{ .NetworkSettings.IPAddress }}’ Node_X)” translates to the IP address of Node-X: 
 
` docker run --name Node_Y -d scylladb/scylla:4.1.0 --seeds="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' Node_X)"`{{execute}} 
 
 
`docker run --name Node_Z -d scylladb/scylla:4.1.0 --seeds="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' Node_X)"`{{execute}} 

Wait a minute or so and check the node status: 

`docker exec -it Node_Z nodetool status`{{execute}}  

You’ll see that eventually, all the nodes have UN for status. U means up, and N means normal. Read more about Nodetool Status [here](https://docs.scylladb.com/operating-scylla/nodetool-commands/status/).
Once the nodes are up, and the cluster is set, we can use the CQL shell to create a table.

Run a CQL shell: 

`docker exec -it Node_Z cqlsh`{{execute}} 

Create a keyspace called “mykeyspace”, with a Replication Factor of three: 

`CREATE KEYSPACE mykeyspace WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'replication_factor' : 3};`{{execute}}

Next, create a table with three columns: user id, first name, and last name, and insert some data: 

`use mykeyspace;`{{execute}} 

`CREATE TABLE users ( user_id int, fname text, lname text, PRIMARY KEY((user_id)));`{{execute}} 

Insert into the newly created table two rows: 

```insert into users(user_id, fname, lname) values (1, 'rick', 'sanchez'); 
insert into users(user_id, fname, lname) values (4, 'rust', 'cohle');```{{execute}} 

Read the table contents:

`select * from users;`{{execute}}

You just saw how to create a three-node cluster with RF=3, how to open a CQL Shell, create a table, insert data into it, and read the data. Next, you will see what happens when nodes are down and how the Consistency Level impacts the read/write operations.