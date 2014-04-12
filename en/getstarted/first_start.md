## 2.1  Starting Mnesia for the first time

Following is a simplified demonstration of a Mnesia system startup. This is the dialogue from the Erlang shell:


````bash

        unix>  erl -mnesia dir '"/tmp/funky"'
        Erlang (BEAM) emulator version 4.9

        Eshell V4.9  (abort with ^G)
        1>
        1> mnesia:create_schema([node()]).
        ok
        2> mnesia:start().
        ok
        3> mnesia:create_table(funky, []).
        {atomic,ok}
        4> mnesia:info().
        ---> Processes holding locks <---
        ---> Processes waiting for locks <---
        ---> Pending (remote) transactions <---
        ---> Active (local) transactions <---
        ---> Uncertain transactions <---
        ---> Active tables <---
        funky          : with 0 records occupying 269 words of mem
        schema         : with 2 records occupying 353 words of mem
        ===> System info in version "1.0", debug level = none <===
        opt_disc. Directory "/tmp/funky" is used.
        use fall-back at restart = false
        running db nodes = [nonode@nohost]
        stopped db nodes = []
        remote           = []
        ram_copies       = [funky]
        disc_copies      = [schema]
        disc_only_copies = []
        [{nonode@nohost,disc_copies}] = [schema]
        [{nonode@nohost,ram_copies}] = [funky]
        1 transactions committed, 0 aborted, 0 restarted, 1 logged to disc
        0 held locks, 0 in queue; 0 local transactions, 0 remote
        0 transactions waits for other nodes: []
        ok
````

In the example above the following actions were performed:

* The Erlang system was started from the UNIX prompt with a flag `-mnesia dir '"/tmp/funky"'`. This flag indicates to Mnesia which directory will store the data.
* A new empty schema was initialized on the local node by evaluating `mnesia:create_schema([node()])`. The schema contains information about the database in general. This will be thoroughly explained later on.
* The DBMS was started by evaluating `mnesia:start()`.
* A first table was created, called funky by evaluating the expression `mnesia:create_table(funky, [])`. The table was given default properties.
* `mnesia:info()` was evaluated and subsequently displayed information regarding the status of the database on the terminal.