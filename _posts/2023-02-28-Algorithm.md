---
layout: post
title:  "Algorithm"
date:   2023-02-28 20:54:00 +0800
categories: algorithm
---

### 1. [Algorithms in system design interviews](https://twitter.com/alexxubyte/status/1544346786365460480)

> Here are some of the algorithms we should know before taking system design interviews.

![Algorithms in system design interviews](https://pbs.twimg.com/media/FW6fNGBVsAIrua0?format=jpg&name=4096x4096)

<br/>

---

<br/>

### 2. [How quadtree works](https://blog.bytebytego.com/p/how-quadtree-works?s=r)

> A **quadtree** is an in-memory data structure and it is not a database solution. 
> It runs on each LBS (Location-Based Service, see last week’s post) server, and the data structure is built at server start-up time.

![How quadtree works](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F4fafbfe9-be63-42d2-95d2-2241fb185b49_1959x2502.jpeg)

- **The first diagram** shows a quadtree is a data structure that is commonly used to partition a two-dimensional space by recursively subdividing it into four quadrants (grids) until the contents of the grids meet certain criteria. 

- **The second diagram** explains the quadtree building process in more detail. The root node represents the whole world map. The root node is recursively broken down into 4 quadrants until no nodes are left with more than 100 businesses. 

**How to get nearby businesses with quadtree?**
- Build the quadtree in memory. 
- After the quadtree is built, start searching from the root and traverse the tree, until we find the leaf node where the search origin is. 
- If that leaf node has 100 businesses, return the node. Otherwise, add businesses from its neighbors until enough businesses are returned.

**Update LBS server and rebuild quadtree**
- It may take a few minutes to build a quadtree in memory with 200 million businesses at the server start-up time. 
- While the quadtree is being built, the server cannot serve traffic. 
- Therefore, we should roll out a new release of the server incrementally to a small subset of servers at a time. This avoids taking a large swathe of the server cluster offline and causes service brownout. 

<br/>

---

<br/>

### 3. [How Consistent Hashing works](https://twitter.com/alexxubyte/status/1554851895826558976)

> What do Amazon DynamoDB, Apache Cassandra, Discord, and Akamai CDN have in common?
> They all use consistent hashing. Let’s dive right in.

![How Consistent Hashing works](https://pbs.twimg.com/media/FZPxjppUsAIoVod?format=jpg&name=large)

𝐖𝐡𝐚𝐭’𝐬 𝐭𝐡𝐞 𝐢𝐬𝐬𝐮𝐞 𝐰𝐢𝐭𝐡 𝐬𝐢𝐦𝐩𝐥𝐞 𝐡𝐚𝐬𝐡𝐢𝐧𝐠?
- In a large-scale distributed system, data does not fit on a single server. They are “distributed” across many machines. This is called horizontal scaling.
- To build such a system with predictable performance, it is important to distribute the data evenly across those servers.
- Simple hashing: serverIndex = hash(key) % N, where N is the size of the server pool

𝐂𝐨𝐧𝐬𝐢𝐬𝐭𝐞𝐧𝐭 𝐡𝐚𝐬𝐡𝐢𝐧𝐠
- Consistent hashing is an effective technique to mitigate this issue.
- The goal of consistent hashing is simple. We want almost all objects to stay assigned to the same server even as the number of servers changes.
- As shown in the diagram, using a hash function, we hash each server by its name or IP address, and place the server onto the ring. Next, we hash each object by its key with the same hashing function.
- To locate the server for a particular object, we go clockwise from the location of the object key on the ring until a server is found. Continue with our example, key 0 is on server 0, key 1 is on server 1.

𝐀𝐝𝐝 𝐚 𝐬𝐞𝐫𝐯𝐞𝐫
- Now let’s take a look at what happens when we add a server.
- Here we insert a new server s4 to the left of s0 on the ring. Note that only k0 needs to be moved from s0 to s4. This is because s4 is the first server k0 encounters by going clockwise from k0’s position on the ring. Keys k1, k2, and k3 are not affected.

𝐇𝐨𝐰 𝐜𝐨𝐧𝐬𝐢𝐬𝐭𝐞𝐧𝐭 𝐡𝐚𝐬𝐡𝐢𝐧𝐠 𝐢𝐬 𝐮𝐬𝐞𝐝 𝐢𝐧 𝐭𝐡𝐞 𝐫𝐞𝐚𝐥 𝐰𝐨𝐫𝐥𝐝
- Amazon DynamoDB and Apache Cassandra: minimize data movement during rebalancing
- Content delivery networks like Akamai: distribute web contents evenly among the edge servers
- Load balancers like Google Network Load Balancer: distribute persistent connections evenly across backend servers

<br/>

---

<br/>

### 4. [B-Tree vs LSM-Tree](https://twitter.com/alexxubyte/status/1583119489318518786)

> What are the differences between B-Tree and LSM-Tree?

![B-Tree vs LSM-Tree](https://pbs.twimg.com/media/FfheyA_VEAEEKS-?format=jpg&name=4096x4096)

𝐁-𝐓𝐫𝐞𝐞
- B-Tree is the most widely used indexing data structure in almost all relational databases.
- The basic unit of information storage in B-Tree is usually called a “page”. To look up a key, it traces down the range of keys until the actual value is found.

𝐋𝐒𝐌-𝐓𝐫𝐞𝐞
- LSM-Tree (Log-Structured Merge Tree) is widely used by many NoSQL databases, such as Cassandra, LevelDB, and RocksDB.
- LSM-trees maintain key-value pairs and are persisted to disk using a Sorted Strings Table (SSTable), in which the keys are sorted.

Level 0 segments are periodically merged into Level 1 segments. This process is called 𝐜𝐨𝐦𝐩𝐚𝐜𝐭𝐢𝐨𝐧. The biggest difference is probably this:
- B-Tree enables faster reads
- LSM-Tree enables fast writes

<br/>

---

<br/>

### 5. [What are the common load-balancing algorithms?](https://twitter.com/alexxubyte/status/1625536625030885377)

> The diagram below shows 6 common algorithms.

![common load-balancing algorithms](https://pbs.twimg.com/media/Fo8Q77UaMAALL2t?format=jpg&name=4096x4096)

***Static Algorithms***

1. **Round robin**
The client requests are sent to different service instances in sequential order. The services are usually required to be stateless.

2. **Sticky round-robin**
This is an improvement of the round-robin algorithm. If Alice’s first request goes to service A, the following requests go to service A as well.

3. **Weighted round-robin**
The admin can specify the weight for each service. The ones with a higher weight handle more requests than others.

4. **Hash**
This algorithm applies a hash function on the incoming requests’ IP or URL.

***Dynamic Algorithms***

5. **Least connections**
A new request is sent to the service instance with the least concurrent connections.

6. **Least response time**
A new request is sent to the service instance with the fastest response time.

<br/>

---

<br/>

### 6. [Graph Algorithm](https://twitter.com/Franc0Fernand0/status/1630960825258131461)

> Graph algorithms are used in many real-world scenarios.
>
> Here are 5 standard graph algorithms and their significant use cases:

![Graph Algorithm](https://pbs.twimg.com/media/FqJWOA6WcAQz3OP?format=jpg&name=large)

1. **Breadth-first search**
- It's a traversal algorithm implemented using a queue data structure.
- It starts from a node and first explores all its adjacent nodes.
- Then it explores all nodes two steps away from the starting one, and so on.
- ***Applications of BFS are***:
    - build web pages indexes in web crawlers
    - discove neighbour nodes in p2p networks
    - find neighboring places in GPS navigation systems

2. **Deepth First Search**
    - It's a traversal algorithm implemented using a stack data structure.
    - It starts from a node and explores as far as possible along each path.
    - When no more nodes exist to explore in the current path, it retraces.
    - ***Applications of DFS are***:
        - find a path between 2 nodes
        - detect cycles in a graph
        - topological sorting
        - solve maze or sudoku puzzles

3. **Shortest path**
    - It's a traversal algorithm that calculates the shortest path between two nodes.
    - The shortest path minimizes the sum of the weights on the traveled edges.
    - There are two main algorithms: Dijkstra and Bellman–Ford.
    - ***Applications of the Shortest path are***:
        - find travelling paths in navigation systems
        - solve the min-delay path problem in networking
        - finding the best route from one point to another in video games

4. **Topological Sorting**
    - It's a graph theory algorithm finding an order in which visiting the nodes.
    - It linearly orders the nodes of a directed graph so that for each edge x -> y, vertex x precedes y.
    - ***Applications of the Topological sorting are***:
        - scheduling interdependent job 
        - data serialisation and sentence ordering
        - compilation tasks ordering and symbols dependencies resolution

5. **Minimum spanning tree**
    - It's a graph theory algorithm finding a subset of the edges connecting all the nodes.
    - Such edges have no cycles and minimum sum of weights.
    - Every connected and undirected graph has at least one spanning tree.
    - ***Applications of the Minimum spanning tree are***:
        - network design
        - image registration and object tracking
        - analysis of graph-based data clusters

<br/>

---

<br/>

### 7. [Binary trees traversal](https://twitter.com/Franc0Fernand0/status/1641089870151180289)

> A binary tree is a computer science data structure consisting of nodes.
> Each node is an abstract data type storing both data and pointers to other nodes.
> In a binary tree, a node points to at most two nodes, the left and right children.
>
> There are two special kinds of nodes in a binary tree:
- the one at the top, called the root
- the ones without children, called leaves
>
> All the nodes in the tree are accessible starting from the root. 
> The depth of a node is how far it is from the root node.

![Binary trees traversal](https://pbs.twimg.com/media/FsZSicUWYAIQtG-?format=jpg&name=large)

**1. One way to traverse a binary tree is using `Depth-first search (DFS)`.**
> DFS starts at the root and traverses as far down the tree as possible in one direction.
>
> Whan it reaches a leaf node, the algorithm goes backward, considering the other direction.
>
> Recursion makes this operation of moving back up the tree simple and elegant.

There are 3 ways to perform DFS according to the order to visit a node and its children.

- ***Preorder*** traversal visit first a node and then its children.

```javascript
print(data)
func(node.left)
func(node.right)
```

- ***Inorder*** traversal visits first the left child, the node, and finally the right child.

```javascript
func(node.left)
print(data)
func(node.right)
```

- ***Postorder*** traversal visits first the children and then the node.

```javascript
func(node.left)
func(node.right)
print(data)
```

> Switching between the above traversals requires only changing the position of the recursive calls.

**2. An alternative way to traverse a binary tree is using `Breadth-first search (BFS)`.**
> BFS starts at the root and traverses all nodes at a depth level before moving on to the next one.
>
> The standard way to implement BFS is using a queue, thanks to its FIFO way of working.