Congratulations! You have completed the second Materialized Views and Indexes Lab. 

## Inspecting the sstables for a Better Understanding

NOTE: The directories and files below are hypothetical. You should get the correct names from the ls -l */* (the last command ran in each terminal).

We will use sstabledump to converts the SSTable into a readable (JSON) file format, and inspect the results.

### BASE TABLE – On Terminal #1

sstabledump menus-76bce980e9e711e9b4a6000000000001/mc-2-big-Data.db

### GLOBAL INDEX – On Terminal #2

sstabledump menus_dish_type_idx_index-11248001e9e811e9b4a6000000000001/mc-2-big-Data.db

### LOCAL INDEX – On Terminal #3

sstabledump menus_dish_type_idx_1_index-bac41d51e9e811e9b4a6000000000001/mc-2-big-Data.db

If you’d like to further investigate what happens when using the above queries with secondary indexes, try turning [TRACING](https://docs.scylladb.com/using-scylla/tracing/) on and executing the queries again.

To summarize, Indexing is a useful tool that provides more types of queries on your tables. In principle, columns we wish to be queryable should be declared when the table is created, as part of a table’s primary key. Secondary Indexing is a neat way of making other columns queryable, but it comes with a cost of additional storage space and processing power to maintain the secondary index data coherent with the primary index information. 

Check out other courses on [Scylla University](https://university.scylladb.com/) to improve your skills. 

You can learn more about these topics in Scylla Documentation: [Materialized Views](https://docs.scylladb.com/using-scylla/materialized-views/#), [Local Secondary Indexes](https://docs.scylladb.com/using-scylla/local-secondary-indexes/), and [Global Secondary Indexes](https://docs.scylladb.com/using-scylla/secondary-indexes/). Two additional and useful references are [this](https://www.scylladb.com/2017/11/03/secondary/) blog post and [this](https://www.scylladb.com/2019/07/23/global-or-localsecondary-indexes-in-scylla-the-choice-is-now-yours/) one.




