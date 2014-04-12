### 3.2  The Data Model

The data model employed by Mnesia is an extended relational data model. Data is organized as a set of tables and relations between different data records can be modeled as additional tables describing the actual relationships. Each table contains instances of Erlang records and records are represented as Erlang tuples.

Object identifiers, also known as oid, are made up of a table name and a key. For example, if we have an employee record represented by the tuple {employee, 104732, klacke, 7, male, 98108, {221, 015}}. This record has an object id, (Oid) which is the tuple {employee, 104732}.

Thus, each table is made up of records, where the first element is a record name and the second element of the table is a key which identifies the particular record in that table. The combination of the table name and a key, is an arity two tuple {Tab, Key} called the Oid. See Chapter 4:Record Names Versus Table Names, for more information regarding the relationship between the record name and the table name.

What makes the Mnesia data model an extended relational model is the ability to store arbitrary Erlang terms in the attribute fields. One attribute value could for example be a whole tree of oids leading to other terms in other tables. This type of record is hard to model in traditional relational DBMSs.