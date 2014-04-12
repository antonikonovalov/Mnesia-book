### 3.1  Defining a Schema

The configuration of a Mnesia system is described in the schema. The schema is a special table which contains information such as the table names and each table's storage type, (i.e. whether a table should be stored in RAM, on disc or possibly on both, as well as its location).

Unlike data tables, information contained in schema tables can only be accessed and modified by using the schema related functions described in this section.

Mnesia has various functions for defining the database schema. It is possible to move tables, delete tables, or reconfigure the layout of tables.

An important aspect of these functions is that the system can access a table while it is being reconfigured. For example, it is possible to move a table and simultaneously perform write operations to the same table. This feature is essential for applications that require continuous service.

The following section describes the functions available for schema management, all of which return a tuple:

* {atomic, ok}; or,
* {aborted, Reason} if unsuccessful.

#### Schema Functions

* `mnesia:create_schema(NodeList)`. This function is used to initialize a new, empty schema. This is a mandatory requirement before Mnesia can be started. Mnesia is a truly distributed DBMS and the schema is a system table that is replicated on all nodes in a Mnesia system. The function will fail if a schema is already present on any of the nodes in NodeList. This function requires Mnesia to be stopped on the all db_nodes contained in the parameter NodeList. Applications call this function only once, since it is usually a one-time activity to initialize a new database.
* `mnesia:delete_schema(DiscNodeList)`. This function erases any old schemas on the nodes in DiscNodeList. It also removes all old tables together with all data. This function requires Mnesia to be stopped on all db_nodes.
* `mnesia:delete_table(Tab)`. This function permanently deletes all replicas of table Tab.
* `mnesia:clear_table(Tab)`. This function permanently deletes all entries in table Tab.
* `mnesia:move_table_copy(Tab, From, To)`. This function moves the copy of table Tab from node From to node To. The table storage type, {type} is preserved, so if a RAM table is moved from one node to another node, it remains a RAM table on the new node. It is still possible for other transactions to perform read and write operation to the table while it is being moved.
* `mnesia:add_table_copy(Tab, Node, Type)`. This function creates a replica of the table Tab at node Node. The Type argument must be either of the atoms ram_copies, disc_copies, or disc_only_copies. If we add a copy of the system table schema to a node, this means that we want the Mnesia schema to reside there as well. This action then extends the set of nodes that comprise this particular Mnesia system.
* `mnesia:del_table_copy(Tab, Node)`. This function deletes the replica of table Tab at node Node. When the last replica of a table is removed, the table is deleted.
* `mnesia:transform_table(Tab, Fun, NewAttributeList, NewRecordName)`. This function changes the format on all records in table Tab. It applies the argument Fun to all records in the table. Fun shall be a function which takes a record of the old type, and returns the record of the new type. The table key may not be changed.

```erlang
-record(old, {key, val}).
-record(new, {key, val, extra}).

Transformer =
   fun(X) when record(X, old) ->
      #new{key = X#old.key,
           val = X#old.val,
           extra = 42}
   end,
{atomic, ok} = mnesia:transform_table(foo, Transformer,
                                      record_info(fields, new),
                                      new),
```

The Fun argument can also be the atom ignore, it indicates that only the meta data about the table will be updated. Usage of ignore is not recommended (since it creates inconsistencies between the meta data and the actual data) but included as a possibility for the user do to his own (off-line) transform.

* `change_table_copy_type(Tab, Node, ToType)`. This function changes the storage type of a table. For example, a RAM table is changed to a disc_table at the node specified as Node.