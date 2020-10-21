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


##### CAP Theorem

	CAP theorem states that is impossible for a distributed data store to
	simultaneously provide more then TWO out of the following three guarantees;

	CA: Data is consistent between all nodes-- as long as all nodes are online- and you can
	read/write from any node and be sure the data is the same, but if you ever develop a partition
	between nodes, the data will be out of sync.

	CP: Data is consistent between all nodes and maintains partition tolerance( preventing data de-sync) by becoming 
	unavailable when a node goes down.

	AP: Nodes remain online even if they can't communicate with each other and will  be re-sync data once
	the partition is resolved but you aren't guaranteed that all nodes will have the same data (either during or after the partition)

	
	In distributed systems,we always needs to select partition tolerance; we need to tolerate packet loss on the network level.
	So in reality, we only have a choice between consistency and availability (CA)

	SQL databases select both C and A.
	NoSQL database select either C or A

	Consistency: All clients see the same data at the same time
	Availability: The system continues to operate even in the presence of node failures.
	Partition Tolerance: The system continues to operate in spite of network failures


##### ACID vs BASE

	ACID
	Strong consistency
	Isolation
	Focus on 'commit'
	Nested transactions
	Availability
	Conservative/pessimistic
	Difficult to evolve a schema

	BASE
	Weak consistency 
	Availibility first
	Best effort
	Approximate answers are acceptable
	Aggresive/optimistic
	Faster and easier schema evolution

#### Paritions and Data Distribution

	A partition is an allocation of storage for a table, backed by solid states drives(SSDs) and automatically replicated across multiple AZ inside a aws region.
	Partition Key: A simple primary key, composed of one attribute known as partition key.This is also called the hash attribute.
	Partition and Sort key: Also known as a composite primary key,this type of key comprises two attributes. The first attribute is the partition key,and the
	second attribute is the sort key. This is also called range attribute.
	
	Read/Write capacity
	On-demand/ Provisioned capacity
	On-demand
	Database scales according to demand
	Good option for:
	Applications with unpredictable traffic
	Prefer to pay as you go.
		


	Provisioned cheaper than On-Demand mode
	Applications with predictable traffic
	Applications whose traffic is consistent or ramps gradually
	Capacity requirements can be forecasted, helping to contol costs.
	
	Both hava a limit of 40.000 RCU/WCU
	You can switch between modes only once per 24 hours.

#### Items
	
	Are limited to 400kb.
	Datatypes:
	Scalar:Exaclty one value-number, string, binary, boolean and null.
	Document: Complex structure with nested attributes(eg. JSON) list and map
	Document Types:
	List:Ordered List
	Map:Unordered attributes

####  DynamoLocal

	Java
	java -Djava.library.path=./DynamoDBLocal_lib \
	-jar DynamoDBLocal.jar
	
	Docker
	docker run -d -p 8000:8000 amazon/dynamodb-local

	For using aws cli
	aws dynamo list-tables \ --endpoint-url http://localhost:8000

#### Metrics for dynamodb

	Track metrics
	Create dashboards
	Create alarms
	Create rules for events
	View logs

#### Metrics 
	
	ConsumeddReadCapacityUnits
	ConsumedWriteCpacityUnits
	ProvisionedReadCapacitUnits
	ProvisionedWriteCapacityUnits
	ReadThrottleEvents
	SuccessfulRequetLatency
	SystemsErrors
	ThrottledRequests
	UerErrors	
	WriteThrottleEvents

#### Alarms

	INSUFFICIENT
	ALARM
	OK

    Alarms have a number of key components:
	Metric
	Thresold
	Period
	Action: sns, auto scaling, ec2

#### Exponential backoff

	It will automatically retry if a max number of error occurs


#### Partition key or partition and sort key

	Identify unique item
	Partition key cannot be delete,modified.
	All columns can change with differente data type,string later number,later object.

#### RCU, WCU

	Remember 8kb = (divide by 4) 1 block=4kb
	Strong		= RCU for Strong consistents (2 RCU)
	Eventual	= Strong consistence /2 = Eventual Consistent (1 RCU)
	Transactional= Strong consistence * 2 (4 RCU)
  
       20 reads at 15KB per item = (16/4) = 4 * 20 = 80 RCU for strong , 40 for eventual, 160 for transactional
