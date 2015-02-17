title: Realize DHT Network through RESTful
tags:
  - DHT
  - RESTful
id: 27
categories:
  - Cloud Computing
date: 2014-11-07 23:53:34
---

# 1.DHT Network

> DHT([Distributed Hash Tables](http://en.wikipedia.org/wiki/Distributed_hash_table "DHT"))is a class of a decentralized [distributed system](http://en.wikipedia.org/wiki/Distributed_computing "Distributed computing") that provides a lookup service similar to a [hash table](http://en.wikipedia.org/wiki/Hash_table "Hash table"); (_key_, _value_) pairs are stored in a DHT, and any participating [node](http://en.wikipedia.org/wiki/Node_(networking) "Node (networking)") can efficiently retrieve the value associated with a given key. Responsibility for maintaining the mapping from keys to values is distributed among the nodes, in such a way that a change in the set of participants causes a minimal amount of disruption. This allows a DHT to scale to extremely large numbers of nodes and to handle continual node arrivals, departures, and failures.
The key idea for the DHT network is utilize the pair(key,value) to  store and retrieve the data in distributed system.

1:Every resource has a unique** pair(key,value)** to distinguish.

2:System will use hash function to process every key,and use the result to determine where this resource should be stored in the DHT network.

3:When a user want to get the resource,use the same algorithm to hash the key and get the destination of the resource.

Maybe you didn't know what I am saying now,but I will use the real model to realize this algorithm and then you will know the key idea about the DHT.After used this algorithm,the DHT network has a new key feature different from other distributed system like the GFS([The Google File System](http://blog-linliu.rhcloud.com/wp-content/uploads/2014/10/The-Google-File-System.pdf)).Every node(a computer in the DHT network) is both client and server,to responsible for a small route and storage in its area.So DHT don't need a "super computer" to know everything in the system.This is why DHT is called **decentralized distributed system.**
> Tips:In the GFS,system need to use a **master** to "control" everything.It needs to know all the information in the distributed system.Obviously,The **master **is the bottleneck in this system,but google has done a lot of optimize in the system,so it is very useful,more information about GFS,you can see the paper in the reference below.
&nbsp;

In that case,DHT can be very scalability because it isn't limited from the central control like the master in the GFS.So it is beneficial to store the huge amount of data in a distributed network.The famous application is the [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database such as **[Cassandra](http://en.wikipedia.org/wiki/Apache_Cassandra) **by facebook and [Dynamo](http://en.wikipedia.org/wiki/Dynamo_(storage_system)) by Amazon.

There are a lot of different way to realize the DHT algorithm like Ring, Tree, Hypercube, Skip List, BuVerfly Network, ...I have used the [chord](http://en.wikipedia.org/wiki/Chord_(peer-to-peer)) to do that because it is easy to explain and use.

![45b48d324a1089fc21efa&amp;690]({{BASE_PATH}}/images/c875063ffe4b73b15aa8d6d3e19c213c9b56437e.png)

**figure 1**

Step to realize the algorithm:

1:Every computer in the DHT network is a **node** which means it is responsible for store the data and route.In the figure1,each computer is represented as N1,N8,N14,N21......

2:Every resource is a **pair(key,value).**The resource can be everything such as the movie,music or the data in the application.The resource name will be hashed into a key and stored in a node.The resource is represented as K10,K24,K30.....The number is the key number of the resource.

3:The **rule** to store the resource in the computer(node) is that the resource is stored in the node which node number is **bigger** than resource key and the **closest** to that resource.For instance,the K10 need to store in N14 because N14 is bigger than 10 and N14 is the closest node to the K10.So K30 and K24 is stored in N32.In that case,once a new resource want to be stored in the DHT network,the resource name will be hashed into a key value and then use this value to determine where the resource should be stored.(This point is very **important** to understand the DHT network,if you can't even understand what I am saying now,back to the step1 again.)

[![45b48d324a1089fd1ebbd&amp;690]({{BASE_PATH}}/images/42aba3ce704bd922cb35b2ffa6cdf00647d7fdbe.png)](http://blog-linliu.rhcloud.com/wp-content/uploads/2014/11/45b48d324a1089fd1ebbdamp690.png)

&nbsp;

**figure 2**

Now,you should know how to store a resource in a node.But how to realize it?

**rule**:every node has its own finger table to route.FINGERN(i) = min {IDN2 | IDn2 ≥ IDn + 2^i (mod M)}

In the figure 2:

N8+1=N9,N9 belong to N14

N8+2=N10,N10 belong to N14

N8+8=N16,N16 belong to N21

For example,if one resource enter in the system and the key value is 22,so it is called K22.Once K22 enter in the system from the N8 and then retrieve the finger table,N8+8 is 16,N8+16 is 24.The K22 is between 16 and 24.So the K22 go to the 16 which is N21.And then use the N21 finger table to retrieve.

&nbsp;

[![45b48d324a1089fffe1be&amp;690]({{BASE_PATH}}/images/59ebda1755c8399caa8871fe3b0f31c04fc245a8.png)](http://blog-linliu.rhcloud.com/wp-content/uploads/2014/11/45b48d324a1089fffe1beamp690.png)

&nbsp;

**figure 3 **

If the key value is bigger than any number of the finger table,use the biggest one.Figure 3 show this condition.[![Screen Shot 2014-11-08 at 01.52.25]({{BASE_PATH}}/images/3bef9e86b26304a922e1223c6d98e17b7b8ece1d.png)](http://blog-linliu.rhcloud.com/wp-content/uploads/2014/11/Screen-Shot-2014-11-08-at-01.52.25.png)

**figure 4**

Figure 4 show the detail about the route process.

> So,what is the **algorithm complexity**?It is same to the finger table size!With high probability, Chord contacts O(log N) nodes to find a successor in an N-node network.
<!--more-->

# 2.RESTful

The RESTful([**Representational state transfer**](http://en.wikipedia.org/wiki/Representational_state_transfer)) is used to connect each node.It is easy to use because it only use the **HTTP command** which is familiar to us.Every node has RESTful client and server,so they can send the request to another and get the response.

In the client side,the client send the **http request** to the server side and get a Response from the server.

[php highlight="3-6,19"]
private Response getRequest(URI uri) {
		try {
			Response cr = client.target(uri)
					.request(MediaType.APPLICATION_XML_TYPE)
					.header(Time.TIME_STAMP, Time.advanceTime())
					.get();
			processResponseTimestamp(cr);
			return cr;
		} catch (Exception e) {
			error(&quot;Exception during GET request: &quot; + e);
			return null;
		}
	}

public String[] get(NodeInfo n, String k) throws Failed{
		UriBuilder ub = UriBuilder.fromUri(n.addr);
		URI getPath = ub.queryParam(&quot;key&quot;, k).build();
		info(&quot;client get(&quot;+getPath+&quot;)&quot;);
		Response response=getRequest(getPath);
		if (response == null || response.getStatus() &gt;= 300) {
			throw new DHTBase.Failed(&quot;GET ?key=&quot;+k+&quot;addr=&quot;+n.addr);
		} else {
			return response.readEntity(tableRowType).getValue().vals;
		}
	}
[/php]

In the server side,you can use the annotation to identify the path and response the request.

[php]
	@GET
	@Path(&quot;info&quot;)
	@Produces(&quot;application/xml&quot;)
	public Response getNodeInfoXML() {
		return new NodeService(headers, uriInfo).getNodeInfo();
	}

[/php]

The more detail about the RESTful you can see the wiki and the paper in the UCI.

#  3.Final

This is my first time to write a English technology passage,so if you have found there are some errors or someplace you didn't understand yet,please do not hesitate to contact me through the email **liulin.jacob@gmail.com **or leave a message below the passage.I am glad to receive your email.If you have some comment about my blog,I am appreciated to learn from you.

Thank you,have fun

lin

liulin.jacob@gmail.com

&nbsp;

**References:**

*   [Distributed Hash Tables](http://en.wikipedia.org/wiki/Distributed_hash_table "DHT")
*   [chord](http://en.wikipedia.org/wiki/Chord_(peer-to-peer))
*   [NoSQL](http://en.wikipedia.org/wiki/NoSQL)
*   [Cassandra](http://en.wikipedia.org/wiki/Apache_Cassandra)
*   [Dynamo](http://en.wikipedia.org/wiki/Dynamo_(storage_system))
*   [The Google File System](http://blog-linliu.rhcloud.com/wp-content/uploads/2014/10/The-Google-File-System.pdf)
*   [谷歌技术"三宝"之谷歌文件系统](http://blog.csdn.net/opennaive/article/details/7483523)
*   [结构化P2P网络——DHT网络原理](http://blog.sina.com.cn/s/blog_45b48d320100q6u7.html)
*   [Representational state transfer](http://en.wikipedia.org/wiki/Representational_state_transfer)
*   [Representational State Transfer (REST)](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
*   [RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)

### Related articles across the web

*   [![]({{BASE_PATH}}/images/c6b6bb93117a03074522e3292ed420c48fa495b8.jpg)](http://blog.libtorrent.org/2014/11/dht-routing-table-maintenance/)[DHT routing table maintenance](http://blog.libtorrent.org/2014/11/dht-routing-table-maintenance/)
*   [![]({{BASE_PATH}}/images/ac855e72b11d34da1bb518f43d6e000380c53f47.jpg)](http://themindstorms.blogspot.com/2014/11/nosql-nosql-databases-hadoop-big-data.html)[NoSQL: NoSQL databases, Hadoop, Big Data: Pinned tabs Nov.3rd](http://themindstorms.blogspot.com/2014/11/nosql-nosql-databases-hadoop-big-data.html)
*   [![]({{BASE_PATH}}/images/5a14a111c81e9492df286ed237beb7c84978d2ef.jpg)](http://www.databasetube.com/nosql/dynamic-dynamos-comparing-riak-and-cassandra/)[Dynamic Dynamos: Comparing Riak and Cassandra](http://www.databasetube.com/nosql/dynamic-dynamos-comparing-riak-and-cassandra/)
*   [![]({{BASE_PATH}}/images/1772745eefe96420fd3a003d9d75fd46980dbba9.jpg)](http://www.hakkalabs.co/articles/eventbrite-recommendation-engine-apache-cassandra-2/)[Eventbrite Recommendation Engine on Apache Cassandra](http://www.hakkalabs.co/articles/eventbrite-recommendation-engine-apache-cassandra-2/)
*   [![]({{BASE_PATH}}/images/082126dfc9c351850e17320faab33fc5b1bd1cfb.jpg)](http://cloudcelebrity.wordpress.com/2014/10/20/the-present-and-future-of-hadoop-from-its-creator-doug-cutting/)[The present and future of Hadoop from its creator Doug Cutting](http://cloudcelebrity.wordpress.com/2014/10/20/the-present-and-future-of-hadoop-from-its-creator-doug-cutting/)
*   [![]({{BASE_PATH}}/images/be783f37a738995347c6672aa3ee7cf3558d1834.jpg)](http://www.programmableweb.com/news/opendirect-api-standardizes-digital-ad-trading/elsewhere-web/2014/11/06)[OpenDirect API Standardizes Digital Ad Trading](http://www.programmableweb.com/news/opendirect-api-standardizes-digital-ad-trading/elsewhere-web/2014/11/06)