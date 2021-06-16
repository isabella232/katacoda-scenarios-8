After seeing how to connect to Scylla Cloud and create a Cluster, you’ll use the CQL shell (Cqlsh) with Docker to connect to the cluster.

[CQL](https://university.scylladb.com/courses/data-modeling/lessons/basic-data-modeling-2/topic/cql-cqlsh-and-basic-cql-syntax/) is a query language that is used to interface with Scylla. It allows us to perform basic functions such as insert, update, select, delete, create, and so on.
The CQL Shell is an interactive Command Line Interface to interact with the database. The connection is established to any one of the nodes, which is then designated as the coordinator node for that specific connection. The coordinator node manages the request path and the response back to the client.

Keep in mind that most real-world applications use drivers to interact with the cluster, you can learn more about using Scylla Drivers in [this course](https://university.scylladb.com/courses/using-scylla-drivers/). 


## Connect to the Cluster

Copy the password and IP address from the instructions tab and run the following command with the password and IP address you copied::

`docker run -it --rm --entrypoint cqlsh scylladb/scylla -u scylla -p *************** 44.193.147.54`{{copy}}

Next, create a Keyspace called mykeyspace:

`CREATE KEYSPACE mykeyspace WITH replication = {'class': 'NetworkTopologyStrategy', 'AWS_US_EAST_1' : 3} AND durable_writes = true;`{{execute}}

A [Keyspace](https://university.scylladb.com/courses/data-modeling/lessons/basic-data-modeling-2/topic/keyspace/) is a top-level container that stores tables with attributes that define how data is replicated on nodes. It defines a number of options that apply to all the tables it contains, most prominently of which is the replication strategy used by the Keyspace.

Use the newly defined keyspace:

`USE mykeyspace;`{{execute}}

## Read and Write Data

Next, create a Table:

`CREATE TABLE monkeySpecies (
    species text PRIMARY KEY,
    common_name text,
    population varint,
    average_size int
);`{{execute}}

A [Table](https://university.scylladb.com/courses/data-modeling/lessons/basic-data-modeling-2/topic/table-and-basic-concepts/) is how Scylla stores data and can be thought of as a set of rows and columns.

![](https://university.scylladb.com/wp-content/uploads/2019/04/primary_key-2.png#main)

Insert some data into the newly created table:

`INSERT INTO monkeySpecies (species, common_name, population, average_size) VALUES ('Saguinus niger', 'Black tamarin', 10000, 500);`{{execute}}

And read the data you just wrote:

`SELECT * FROM monkeySpecies;`{{execute}}



