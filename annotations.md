##### ACID PROPERTIES

	Atomicity A sequence of operations where either all occur or nothing occurs.

	Consistency the state of the database, both before or after execution of an operation
	remains consistent whether or not the transactions commits or is rolled back

	Isolation changes mades by a transaction are isolated from the rest
	of the system until after the transaction has commited

	Durability commited data is saved by the system such that, even in the
	event of failure and system restart, the data is available in its correct state.

##### Scaling

#####	Replication

	Pros: Offloads master code
	Read replica can scale horizontally


	Const:Does not increase the speed of writes
	Replication lag


#####	Sharding
	
	Pros: Can store larger data sets
	Can handle more load than a single master node

	Cons:Queries become more complex (range queries might hit all nodes)
	Joins become difficult,your tables are no longer all on the same node
