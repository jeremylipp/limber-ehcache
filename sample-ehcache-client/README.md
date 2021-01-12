#What is the sample ehcache client ?

Terracotta Server supports a distributed in-memory data-storage topology, which enables the sharing of data among multiple caches and in-memory

The client java app connects to the Terracotta Server and starts putting and getting key/value mappings.

You can have a look at its code at : src/ClientDoingInsertionsAndRetrievals.java ; it's configured to hold few elements (50) on heap, and of course use the Terracotta server at terracotta:9410 as its clustered tier.

The client will either insert or retrieve values every 0.1 seconds


#How to use this image: QuickStart

You can start it up simply with :

    docker run --name client -d terracotta/sample-ehcache-client:5.6.4

But you would get such an error message :
    
    The environment variable TERRACOTTA_SERVER_URL was not set; using terracotta:9410 as the cluster url.

followed by :

    WARN - We couldn't load configuration data from the server at 'terracotta:9410'; retrying. (Error: terracotta.)


That would be because you need a Terracotta server running.

You can run a terracotta server using (provided you built the terracotta image) :

    docker run -d -p 9410:9410 --name terracotta terracotta/terracotta-server-oss:5.6.4

and then re try running the client, with :

    docker run -d --link terracotta:terracotta --name client terracotta/sample-ehcache-client:5.6.4
    
and checkout what's happening with :

    docker logs -f client


#### How to build this image

To build this [Dockerfile](https://github.com/Terracotta-OSS/docker/blob/master/5.6.4/server/Dockerfile), clone this [git repository](https://github.com/Terracotta-OSS/docker) and run :

    $ cd 5.6.4/sample-ehcache-client
    $ docker build -t sample-ehcache-client:5.6.4 .