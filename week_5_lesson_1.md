# DBMS-based and non-DBMS-based Approaches to Big Data

In the previous modules, we talked about data variety and streaming data.
In this module, we'll focus on a central issue in large scale data processing and management and that is :
when should we use Hadoop or Yarn style system ?
when should we use a database system that can perform parallel operations?

then we'll explore how the state of the art big data management systems address these issues of volume and variety. We start with the problem of **high volume data** and two contrasting approaches for handling them. So after this lesson, you'll be able to And briefly describe:
1.   explain the various advantages of using a DBMS over a file system.
2.   Specify the differences between parallel and distributed DBMS.
3.   briefly describe a MapReduce-style DBMS and its relationship with the current DBMSs.


In the early days, when database systems weren't around or just came in, databases were designed as a set of application programs. 
They were written to handle data that resided in files in** a file system**. However, 

soon this approach led to problems:-

**First**, there are multiple file formats. often, there was a duplication of information in different files. Or the files simply had inconsistent information that was very hard to determine, especially when the data was large and the file content was complex.

**Secondly**, there wasn't a uniform way to access data. Each data access task, like finding employees in a department sorted by their salary versus finding employees in all departments sorted by their start date needed to be written as a separate program.So people ended up writing different programs for data access, as well as data update. 

**A third** problem was rooted to the enforcement of constraints, often called integrity constraints. For example, to say something like every
employee has exactly one job title. One had arrived that condition, as part of an application program called. So if you want to change the constraint, you need to look for the programs where such a rule is hard coded.

**The fourth** problem has to do with system failures. Supposed Joe, an employee becomes the leader of a group and moves in to the office of the old leader, Ben who has now become the director of the division. So we update Joe's details and move on to update, Ben's new office for the system crashes. So the files are incompletely updated and there is no way to go back, and start all over.

</br>The term **atomicity** means that all of the changes that we need to do for these promotions must happen altogether,
as a single unit. They should either fully go through or not go through at all. This atomicity is very difficult to handle
when the data reside in one or more files. So, a prime reason for the transition to a DBMS is to alleviate these and other difficulties. If we look at the current DBMS, especially relational DBMS, we will notice a number of advantages. DBMSs offer query languages, which are declarative. 
Declarative means that we state what we want to retrieve without telling the DBMS how exactly to retrieve it. In a relational DBMS, we can say, find the average set of salary of employees in the R&D division for every job title and sort from high to low. We don't have to tell the system how to group these records by job title or how to extract just the salary field. A typical user of a DBMS who issues queries does not worry about how the relations are structured or whether they are located in the same machine, or spread across five machines. The goal of data independence is to
isolate the users from the record layout so long as the logical
definition of the data, which means the tables and
their attributes are clearly specified. Now most importantly, relational DBMSs
have developed a very mature and continually improving methodology of
how to answer a query efficiently, even when there are a large
number of cables and the number of records
exceeds hundreds of millions. 
From a 2009 account,
EB uses the tera data system with 72 machines to manage approximately
2.4 terabytes of relational data. These systems have built powerful
data structures, algorithms and sound principles to determine how
a specific array should be onset efficiently despite the size of the data
and the complexity of the tables. Now with any system,
bad things can happen. Systems fail in the middle
of an operation. Malicious processes try to get
unauthorized access to data. One large can often underappreciated
aspect of a DBMS is the implementation of transaction
safety and failure recovery. Now, recall our discussion of atomicity. In databases, a single logical operation
on the data is called a transaction. For example, a transfer of funds
from one bank account to another, even involving multiple changes
like debiting one account and crediting another is a single transaction. Now, atomicity is one of the four
properties that a transaction should provide. The four properties,
collectively called ACID are atomicity, consistency, isolation and durability. Consistency means any data
written to the database must be valid according to all defined
rules including constrains. The durability property ensures that
once a transaction has been committed, it will remain so, even in the event
of power loss, crashes or errors. The isolation property comes
in the context of concurrency, which refers to multiple people
updating a database simultaneously. To understand concurrency, think of
an airline or a railway reservation system where hundreds and thousands of
people are buying, cancelling and changing their reservations and
tickets all at the same time. The DBMS must be sure that
a ticket should no be sold twice. Or if one person is in the middle
of buying the last ticket, another person does not see
that ticket as available. These are guaranteed by
the isolation property that says, not withstanding the number of people
accessing the system at the same time. The transactions must happen as if they're
done serially, that is one after another. Providing these capabilities is
an important part of the M in DBMS. So next, we consider how traditional
databases handle large data volumes. The classical way in which DBMSs
have handled the issue of large volumes is by created parallel and
distributed databases. In a parallel database, for
example, parallel Oracle, parallel DB2 or post SQL XE. The tables are spread across multiple
machines and operations like selection, and join use parallel algorithms
to be more efficient. These systems also allow a user
to create a replication. That is multiple copies of tables. Thus, introducing data redundancy, so that failure on replica can be
compensated for by using another. Further, it replicates in
sync with each other and a query can result into
any of the replicates. This increases the number of simultaneous
that is conquer into queries that can be handled by the system. In contrast, a distributed DBMS, which
we'll not discuss in detail in this course is a network of independently running
DBMSs that communicate with each other. In this case, one component knows some
part of the schema of it is neighboring DBMS and can pass a query or part of
a query to the neighbor when needed. So the important takeaway issue here is, are all of these facilities
offered by a DBMS important for the big data application that
you are planning to build? And the answer in many
cases can be negative. However, if these issues are important,
then the database management systems may offer a viable option for
a big data application. Now, let's take a little more time to
address an issue that's often discussed in the big data word. The question is if DBMSs are so powerful, why do we see the emergence
of MapReduce-style Systems? Unfortunately, the answer to this
question is not straightforward. For a long while now, DBMSs have
effectively used parallelism, specifically parallel databases in addition to
replication would also create partitions. So that different parts of a logical
table can physically reside on different machines,, then different parts of a query
can access the partitions in parallel and speed up creative performance. Now these algorithms not only improve
the operating efficiency, but simultaneously optimize algorithms to
take into account the communication cost. That is the time needed to
exchange data between machines. However, classical parallel DBMSs did
not take into account machine failure. And in contrast, MapReduce was
originally developed not for storage and retrieval, but for distributive
processing of large amounts of data. Specifically, its goal was to support
complex custom computations that could be performed efficiently on many machines. So in a MapReduce or MR setting, the number of machines
could go up to thousands. Now since MR implementations were
done over Hadoop file systems, issues like node failure were
automatically accounted for. So MR effectively used in complex
applications like data mining or data clustering, and these algorithms
are often very complex, and typically require problem
specific techniques. Very often,
these algorithms have multiple stages. That is the output from one processing
stage is the input to the next. It is difficult to develop these
multistage algorithms in a standard relational system. But since these were genetic operations,
many of them were designed to work with unstructured data like text and
nonstandard custom data formats. Now, it's now amply clear that this
mixture of data management requirements and data processing analysis requirements
have created an interesting tension in the data management world. Just look at a few of
these tension points. Now, DBMSs perform storage and
retrieval operations very efficiently. But first,
the data must be loaded into the DBMS. So, how much time does loading take? In one study,
scientists use two CVS files. One had 92 attributes with
about 165 million tuples for a total size of 85 gigabytes. And the other had 227 attributes
with 5 million tuples for a total size of 5 gigabytes. The time to load and index this data in
MySQL and PostgreSQL, took 15 hours each. In a commercial database running on
three machines, it took two hours. Now there are applications like
the quantities in the case we discussed earlier where this kind of loading
time is simply not acceptable, because the analysis on
the data must be performed within a given time limit
after it's arrival. A second problem faced by
some application is that for them, the DBMSs offer
too much functionality. For example, think of an application
that only looks at the price of an item if you provide it with a product name or
product code. The number of products it serves
is let's say, 250 million. This lookup operation happens
only on a single table and does not mean anything
more complex like a join. Further, consider that while there
are several hundred thousand customers who access this data,
none of them really update the tables. So, do we need a full function DBMS for
this read-only application? Or can we get a simpler solution which
can use a cluster of machines, but does not provide all the wonderful
guarantees that a DBMS provides? At the other end of the spectrum,
there is an emerging class of optimization that meets all the nice transactional
guarantees that a DBMS provides. And at the same time, meets the support
for efficient analytical operations. These are often required for
systems like Real-Time Decision Support. That will accept real-time data like
customer purchases on a newly released product will perform some
statistical analysis, so that it can determine buying trends. And then decide whether in real-time, a discount can be offered
to this customer now. It turns out that the combination
of traditional requirements and new requirements is leading
to new capabilities, and products in the big data
management technology. On the one hand, DBMS technologies
are creating new techniques that make use of MapReduce-style data processing. Many of them are being
developed to run on HDFS and take advantage of his data
replication capabilities. More strikingly, DBMSs are beginning
to have a side door for a user to perform and
MR-style operation on HDFS files and exchange data between the Hadoop
subsystem and the DBMS. Thus, giving the user the flexibility
to use both forms of data processing. It has now been recognized
that a simple map and reduce operations are not sufficient for
many data operations leading to a significant expansion in the number
of operations in the MR ecosystems. For example,
Spark has several kinds of join and data grouping operations in
addition to map and reduce. Sound DBMSs are making use of large distributed memory management
operations to accept streaming data. These systems are designed with the idea
that the analysis they need to perform on the data are known before. And as new data records arrive,
they keep a record of the data in the memory long enough to finish
the computation needed on that record. And finally, computer scientists and data scientists are working
towards new solutions where large scale distributed algorithms
are beginning to emerge to solve different kinds of analytics problems like
finding dense regions of a graph. These algorithms use
a MR-style computing and are becoming a part of a new
generation of DBMS products that invoke these algorithms
from inside the database system. In the next video, we'll take a look at
some of the modern day data management systems that have some
of these capabilities.
