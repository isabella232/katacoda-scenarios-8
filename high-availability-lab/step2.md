After you saw how to create a three node cluster, open a CQL Shell to a node in the cluster, created a table with RF=3, write and read data, you will now see what happens when we try to read and write data to our table, with varying Consistency Levels and when some of the nodes are down.

## Task


Set the Consistency Level to QUORUM and perform a write:

use mykeyspace; 

CONSISTENCY QUORUM 

insert into users (user_id, fname, lname) values (7, 'eric', 'cartman');  

Read the data to see the insert was successful:

select * from users; 

The read and write operations were successful. What do you think would happen if we did the same thing with Consistency Level ALL? 

CONSISTENCY ALL 

insert into users (user_id, fname, lname) values (8, 'lorne', 'malvo'); 
select * from users;

The operations were successful again. CL=ALL means that we have to read/write to the number of nodes according to the Replication Factor, 3 in our case. Since all nodes are up, this works. 

Next we’ll take one node down and check read and write operations with a Consistency Level of Quorum and ALL.

Exit the CQL Shell and take down Node_Y and check the status (it might take some time until the node is actually down): 

exit

docker stop Node_Y 

docker exec -it Node_Z nodetool status 

Now, set the Consistency Level to QUORUM and perform a write: 

docker exec -it Node_Z cqlsh 

CONSISTENCY QUORUM 

use mykeyspace;

insert into users (user_id, fname, lname) values (9, 'avon', 'barksdale');  

Read the data to see the insert was successful: 

select * from users; 

With CL = QUORUM, the read and write were successful. What will happen with CL = ALL? 

CONSISTENCY ALL 

insert into users (user_id, fname, lname) values (10, 'vm', 'varga');  

select * from users; 

Both read and write fail. CL = ALL requires that we read/write to three nodes (based on RF = 3), but only two nodes are up.

What happens if another node is down?

Take down Node_Z and check the status:

exit

docker stop Node_Z 

docker exec -it Node_X nodetool status 

Now, set the Consistency Level to QUORUM and perform a read and a write: 

docker exec -it Node_X cqlsh 

CONSISTENCY QUORUM 

use mykeyspace; 

insert into users (user_id, fname, lname) values (11, 'morty', 'smith');  

select * from users; 

With CL = QUORUM, the read and write fail. Since RF=3, QUORUM means at least two nodes need to be up, in our case just one is. 

What will happen with CL = ONE? 

CONSISTENCY ONE 

insert into users (user_id, fname, lname) values (12, 'marlo', 'stanfield');   

select * from users; 

This time the read and write are successful. CL = ONE requires that we read/write to just one node. Since one node is up this works. 

