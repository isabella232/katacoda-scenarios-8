After you saw how to create a three node cluster, open a CQL Shell to a node in the cluster, creat a table with RF=3, write and read data, you will now see what happens when we try to read and write data to our table, with varying Consistency Levels and when some of the nodes are down.

## CL=QUORUM, All Nodes Are Up


Set the Consistency Level to QUORUM and perform a write:

use mykeyspace; 

CONSISTENCY QUORUM 

insert into users (user_id, fname, lname) values (7, 'eric', 'cartman');  

Read the data to see the insert was successful:

select * from users; 

The read and write operations were successful. What do you think would happen if we did the same thing with Consistency Level ALL? 

