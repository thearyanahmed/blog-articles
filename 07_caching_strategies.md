Caching can improve the performance of our application tremendously. And there are ways to implement the caching mechanism. In this article, I’ll briefly discuss 2 caching strategies, them being Lazy Loading & Write Through.

### Lazy Loading
Lazy loading means loading the data (into cache) when necessary. Lazily, if you like. The process goes like so,

1. Application asks for data to the cache
2. Cache has that data ?
	1. Yes: Return the data (cache hit)
	2. No: Returns null/void/empty value (cache miss)
		1. Application will fetch the data from the data store and trigger a cache write.

#### Advantages
1. *Only requested data is cache*. Therefore, if you have a massive amount of data in your data store, you’ll probably less-er data in your cache. Utilising your cache-space in a proper way.
2. *Cluster/Node failures doesn’t break your application*. In this way the caching mechanism will sort of act in a **Proxy Pattern**. But your database will take the hit if your cache-mechanism is not working.

#### Disadvantages 
1. Apart form every cache miss takes additional I/O operation. 
2. *Stale Data*, If the implementation (design) says that the cache data will only be updated only when there is a cache miss, then there is a chance of stale data. Maybe between 2 cache miss the database has updated, but as the cache is being written into when there is a cache miss from the application, therefore the cache holds old/sometimes invalid data. 

Adding a TTL sometimes may help, depending on the situation.

### Write Through
Write through strategy says that the cache would be written in when data is written to the database. It is somewhat the opposite of Lazy Loading’s cache mutation, conceptually.

#### Advantages 
1. In this mechanism, cache data is never stale as it is being updated along with the datastore.
2. Read Performance is always good (considering other stats are ok, like network latency, disk read-write speed etc).

#### Disadvantages
1. Most of the data might never be read, yet your cache will act as a secondary data store. This will cost you more for your caching server.
2. In terms of clustering or sharding, when you pick up a new node AKA add a shard to the cluster, either due to node failure or scaling out, your data might be unavailable for a certain period of time, causing total cache miss until it is fixed. 
3. Also, if the shard for some reason gets out of sync, the next time the valid data will be added/updated once it is re-written again in the database.

There are hybrid strategies where you use both of the together and definitely other strategies too. And the advantages and disadvantages covers some of the common ones. 
I hope it helped.

 
