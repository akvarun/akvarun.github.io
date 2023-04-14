---
title: "Distributed System"
summary: "A distributed system is a network of independent computers that work together by dividing tasks and coordinating actions through communication. It is used in various applications, such as e-commerce, social networks, and online gaming, but it can be challenging to design and implement due to the need for efficient communication, synchronization, and fault-tolerance.."
draft: false
math: true
comments: true

---

## Unit 1 
A Taxonomy of Distributed Systems - Models of computation: shared memory and message
passing systems, synchronous and asynchronous systems. Communication in Distributed Systems:
Remote Procedure Calls, Message Oriented Communications and implementations over a simple
distributed system.

## Unit 2
Global state and snapshot algorithms. Logical time and event ordering, clock synchronization,
Distributed mutual exclusion, Group based Mutual Exclusion, leader election, concurrency control,
deadlock detection, termination detection, implementations over a simple distributed system.

## Unit 3 
Consistency and Replication: Data Centric Consistency, Client Centric Consistency, Replica
Management, Consistency Protocols. Fault tolerance and recovery: basic concepts, fault models,
agreement problems and its applications, commit protocols, voting protocols, check pointing and
recovery. Distributed file systems: scalable performance, load balancing, and availability. Case Studies:
Dropbox, Google FS (GFS)/ Hadoop Distributed FS (HDFS), Bigtable/HBase MapReduce, RDDs,
Apache Spark	

### Characteristics of a distrubuted system 
1. No common physical clock :
* inherent asynchrony amongsnt the processors.
2. No shared memory
* so when there is no shared memory we use the message passing for communication. 
3. Geographical separation 
* now (network of workstations) which is being locally connected i.e LAN . NOW is becoming popular because of its low cost and high speed. 
4. autonomy and hetrogenity 
* loosely coupled , each processor will have different speeds and can run on a different operating system. They co-operate and do the work by offering services and solving a problem.

* The distrubted software is also called as the **middleware**
* A distrubuted execution is the execution of the processes across the distributed system to acheive a common goal 
* RPC - remote procedure call is the method where one machine calls a function that is residing on another machine. 

### motivation to use distrubuted systems 
1. Inherently distributing compitation 
* for example bank money transfer , where they are located distrubtedly
2. Resource sharing 
3. Access to remote data and resources 
4. Enhanced reliabilty 
5. Increased performance / cost ratio 
6. Scalabity 
7. Modularity and incremental exandability

* A multiprocessor system is a parallel system in which the multiple processors have direct access to shared memory 
which forms a common address space.
* Multiprocessor system has two architectures 
1. UMA (uniform memory access)
2. NUMA (non-uniform memory access)

### Flyns taxonomy (Four modes)
* Single instruction stream , single data stream (SISD)
single CPU and single memory unit connected by a bus.
* Single instruction stream , multiple data stream (SIMD)
* Multiple instruction stream , single data stream (MISD)
* Multiple instruction stream , multiple data stream (MIMD)

#### Coupling , parallesim , concurrency and granularity 
* Coupling - Tightly coupled and loosely coupled 
Tightly coupled are homogenous i.e mostly of the same type and shared memory 
<br>
Loosely coupled are hetrogenous and the systems are remotely located and does not have a shared memory and does not share a common clock for example the distrubuted system ( connected by a LAN or WAN)

* Parallelism and concurrency is similar 
we use concurrency in distributed systems 

* Granularity of a program 
ratio of the amount of computation to the amount of communication within the parallel or distrubuted system 
The granularity is high for tightly coupled system 


## Primitives for distributed communication
1. Synchronous 
2. Asynchronous
3. Blocking 
4. Non - Blocking 

* Synchronous - The send () primitive completes only when it knows when the receiver() primitive is invoked  
* Asynchronous - It is said to be asynchronous when the control returns back to invoking process. 
* Blocking - A primitive is blocking if control returns to the
invoking process after the processing for the primitive (whether in syn-
chronous or asynchronous mode) completes.
* Non - blocking - A primitive is non-blocking if control returns
back to the invoking process immediately after invocation, even though
the operation has not completed.

#### four version for send primitive :
1. synchronous blocking 
2. synchronous non - blocking
3. asynchronous blocking 
4. asynchronous non blocking 

#### two for recv primitive :
1. synchronous blocking 
2. synchronous non blocking 

* blocking sync send ,blocking recv 
* non-blocking sync send , non blocking recv 
* blocking asyn send 
* non blocking async send 

## Three events in the distrubuted system 
* internal event
* send event
* receive event 
The execution of process pi produces a sequence of events e1i , e2i (2nd event of the ith process) , ... exi. 
H = (hi -> i) defines that the event is occuring an a linear order , the -> i denotes the linear order.

### Distrubuted program 
* processors do not share a global memory and communicate solely by message passing. 
* it does not share a global clock hence it is asynchronous .

#### lamports happen before ->
* ei -> ej here ej is directly or transitively dependent on ei . 
* FIFO , NON - FIFO and causal ordering. 
* **causal ordering** - is based on the *lamports happen before relation*.
* In causal ordering eg . send (m1) -> send (m2) , in this example the recv of m1 happens before the recv of m2 even though the m2 message is sent earlier , it is stored in the buffer.
### distrubuted mutual exclution 
* only one process is allowed to access the critical section 
* concurrent process can access a shared resources , but kinda serialized as one can access at one time
* message passing
##### Three approches 
1. Token based - Unique token , if they have the token with them they can access the crtical section.  
2. Non - token based - messages 
3. Quorum based - Subset 

### system model 
* N - sites 
* asynchronous 
* let us assume there are 5 sites , s1 s2 s3 s4 s5 , if s1 is requesting for the critical section , it waits for the reply from the other sites , during the wait period it cannot place another request to enter the critical section. 
#### process 
1. Request to enter the cs 
2. executing the cs
3. release the cs

* if two sites request for the critical section , it depends on the time stamp (lamports time stamp)
* the smallest the time stamp the higher the priority (who places the request first)

### requirements of the mutual exclusion 
1. safety property - only one process can execute
2. liveness property - absense of deadlock (wait endlessly for msg) & starvation (indefinite wait) 
3. fairness property - if a site s1 is the sole owner of the token and is not giving the token to other process , and greedily keeps it with itself .

### Performance metrics
1. message complexity - no of message require to execute a cs .
2. synchronization delay - time between the (last site exits the critical section - next site enters the critical section.)
3. response time - the time between the request message sent out and then exiting the critical section .
4. system throughput - 1/ sd + E ; sd = sync delay , E - crtical section execution time. 

## Non token based  for mutual exclusion - lamport algorithm 
1. request the critical section , broadcasts the REQUEST (tsi,i) tsi - time stamp of i (the logical clock) , i is the process id , request_queue (tsi, i)


#### complexity of lamport 
1. the number of REQUEST - N-1 
2. the number of REPLY - N-1 
3. the number of RELEASE - N-1 
* Total = 3(N-1)



## Non token based for mutual exlusion - ricart agarwala





#### complexity of ricart agarwala 
1. the number of REQUEST - N-1 
2. the number of REPLY - N-1 
* Total = 2(N-1)
optimized than lamport algorithm as we use the deffered queue


