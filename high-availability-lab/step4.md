What happens if we set the Consistency Level to ALL?

## CL=ALL, All Nodes Are Up


Set the Consistency Level to ALL and perform a write:

CONSISTENCY ALL 

insert into users (user_id, fname, lname) values (8, 'lorne', 'malvo'); 
select * from users;

The operations were successful again. CL=ALL means that we have to read/write to the number of nodes according to the Replication Factor, 3 in our case. Since all nodes are up, this works. 

