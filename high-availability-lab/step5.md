Next we’ll take one node down and check read and write operations with a Consistency Level of Quorum.

## Read and Write Data 


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

With CL = QUORUM, the read and write were successful. 

