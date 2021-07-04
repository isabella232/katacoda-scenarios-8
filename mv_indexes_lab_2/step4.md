In this step you will use the [sstabledump](https://docs.scylladb.com/operating-scylla/admin-tools/sstabledump/) tool to see what happens under the hood when using indexes and Materialized Views.  

## Inspecting the sstables for a Better Understanding

NOTE: The directories and files below are hypothetical. You should get the correct names from the ls -l */* (the last command ran in each terminal).

We will use sstabledump to converts the SSTable into a readable (JSON) file format, and inspect the results.

### BASE TABLE – On Terminal #1

`sstabledump menus-76bce980e9e711e9b4a6000000000001/mc-2-big-Data.db`{{copy}}

### GLOBAL INDEX – On Terminal #2

`sstabledump menus_dish_type_idx_index-11248001e9e811e9b4a6000000000001/mc-2-big-Data.db`{{copy}}

### LOCAL INDEX – On Terminal #3

`sstabledump menus_dish_type_idx_1_index-bac41d51e9e811e9b4a6000000000001/mc-2-big-Data.db`{{copy}}

If you’d like to further investigate what happens when using the above queries with secondary indexes, try turning [TRACING](https://docs.scylladb.com/using-scylla/tracing/) on and executing the queries again.

