### 3.3  Starting Mnesia

Before we can start Mnesia, we must initialize an empty schema on all the participating nodes.

* The Erlang system must be started.
* Nodes with disc database schema must be defined and implemented with the function create_schema(NodeList).

When running a distributed system, with two or more participating nodes, then the mnesia:start( ). function must be executed on each participating node. Typically this would be part of the boot script in an embedded environment. In a test environment or an interactive environment, mnesia:start() can also be used either from the Erlang shell, or another program.

#### Initializing a Schema and Starting Mnesia

To use a known example, we illustrate how to run the Company database described in Chapter 2 on two separate nodes, which we call a@gin and b@skeppet. Each of these nodes must have have a Mnesia directory as well as an initialized schema before Mnesia can be started. There are two ways to specify the Mnesia directory to be used:

* Specify the Mnesia directory by providing an application parameter either when starting the Erlang shell or in the application script. Previously the following example was used to create the directory for our Company database:

```bash
%erl -mnesia dir '"/ldisc/scratch/Mnesia.Company"'
```

* If no command line flag is entered, then the Mnesia directory will be the current working directory on the node where the Erlang shell is started.

To start our Company database and get it running on the two specified nodes, we enter the following commands:

* On the node called gin:

```bash
 gin %erl -sname a  -mnesia dir '"/ldisc/scratch/Mnesia.company"'
```

* On the node called skeppet:

```bash
skeppet %erl -sname b -mnesia dir '"/ldisc/scratch/Mnesia.company"'
```

* On one of the two nodes:

```bash
(a@gin1)>mnesia:create_schema([a@gin, b@skeppet]).
```

* The function mnesia:start() is called on both nodes.
* To initialize the database, execute the following code on one of the two nodes.

As illustrated above, the two directories reside on different nodes, because the /ldisc/scratch (the "local" disc) exists on the two different nodes.

By executing these commands we have configured two Erlang nodes to run the Company database, and therefore, initialize the database. This is required only once when setting up, the next time the system is started mnesia:start() is called on both nodes, to initialize the system from disc.

In a system of Mnesia nodes, every node is aware of the current location of all tables. In this example, data is replicated on both nodes and functions which manipulate the data in our tables can be executed on either of the two nodes. Code which manipulate Mnesia data behaves identically regardless of where the data resides.

The function mnesia:stop() stops Mnesia on the node where the function is executed. Both the start/0 and the stop/0 functions work on the "local" Mnesia system, and there are no functions which start or stop a set of nodes.

#### The Start-Up Procedure

Mnesia is started by calling the following function:

          mnesia:start().

This function initiates the DBMS locally.

* The choice of configuration will alter the location and load order of the tables. The alternatives are listed below:
* Tables that are stored locally only, are initialized from the local Mnesia directory.
* Replicated tables that reside locally as well as somewhere else are either initiated from disc or by copying the entire table from the other node depending on which of the different replicas is the most recent. Mnesia determines which of the tables is the most recent.
* Tables that reside on remote nodes are available to other nodes as soon as they are loaded.

Table initialization is asynchronous, the function call mnesia:start() returns the atom ok and then starts to initialize the different tables. Depending on the size of the database, this may take some time, and the application programmer must wait for the tables that the application needs before they can be used. This achieved by using the function:

```erlang
mnesia:wait_for_tables(TabList, Timeout)
```

This function suspends the caller until all tables specified in TabList are properly initiated.

A problem can arise if a replicated table on one node is initiated, but Mnesia deduces that another (remote) replica is more recent than the replica existing on the local node, the initialization procedure will not proceed. In this situation, a call to to mnesia:wait_for_tables/2 suspends the caller until the remote node has initiated the table from its local disc and the node has copied the table over the network to the local node.

This procedure can be time consuming however, the shortcut function shown below will load all the tables from disc at a faster rate:

mnesia:force_load_table(Tab). This function forces tables to be loaded from disc regardless of the network situation.
Thus, we can assume that if an application wishes to use tables a and b, then the application must perform some action similar to the below code before it can utilize the tables.

          case mnesia:wait_for_tables([a, b], 20000) of
            {timeout,   RemainingTabs} ->
              panic(RemainingTabs);
            ok ->
              synced
          end.

Warning
When tables are forcefully loaded from the local disc, all operations that were performed on the replicated table while the local node was down, and the remote replica was alive, are lost. This can cause the database to become inconsistent.

If the start-up procedure fails, the mnesia:start() function returns the cryptic tuple {error,{shutdown, {mnesia_sup,start,[normal,[]]}}}. Use command line arguments -boot start_sasl as argument to the erl script in order to get more information about the start failure.