# DistributedSearchEngine

## Current Progress

- Implemented a leader election algorithm for zookeeper to allow fault tolerant and horizontally scalable. For electing a leader node from a zookeeper cluster, we will always choose the one with smallest index. And we have every node  add a watcher to watch on the previous node's znode. And when a node was deleted, the next node will be notified and will reelect the leader. In this way, as long as there is a node alive in the cluster, it will be able to reelect the leader. And this algorithm can also help that instead of notifying all the other nodes, we only need to notify the next node to make the corresponding change, which can solved the performance bottlenecks caused by Herd Effect.

- Create a service registry for the zookeeper to keep track of all the worker nodes' IP addresses. 

- Implemented a sequential version of TF-IDF search system so that clients can send a sentence as a query and return the top related documents that are stored in our package as well as their corelation scores. The scores are calculated by 2 parts. First part is, we will have a map to store the frequency of each words in the query sentence for each document, we will get a percentage by dividing the frequency with the total number of words in the document. And then since for each word, the weight should be the same, because like  the word "the" appears in almost every document, while the word "car" should be more weighted. Thus, the second part is, calculate the log(total number of documents / how many documents contains the word). The more documents contains the word, the less weight the word will get. 

- Integrate the zookeeper into the sequential version of TF-IDF search system to make it parallel. Using Java serialization objects to send http requests and response between the coordinators and workers. 

- Integrateed the web application server as well as front-end UI, the web application server will get search query Json from web browsers by Ajax, and then, the web application will send the search request to the distributed system through Google Protocol Buffers. Once the distributed search system finished the computing, they will send the response back to the web application server for doing data transforming and filtering. And finally will be returned back to the front end.

## Next Steps

1. Onboard the HAProxy to our existing system to use round robin algorithm for load balancing. Onboard the java jar package and the HAProxy into docker containers for convenient deployment(HAProxy can only be used on Linux distributed system) to release the pressure of the web application server. 
2. Onboard Kafka to our system for better system extension.
3. Consider using Elastic Search to improve the full text search, thus further improve the search performance.

## Related Link

- Sample client server code: https://github.com/CassieW999/DistributedSearchEngine_ClientServer.git
