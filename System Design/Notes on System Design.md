**Software design** --> how lines of code are assembled. Primitives like variables, functions, classes, etc.
System design --> how services are assembled. Primitives like app servers, databases, caches, queues, event buses, proxies, etc.

Good system design should be underwhelming. It looks like nothing going wrong for a long time. 

Complexity needs to be earned, evolved from a simple system that works. Beginning from scratch with a complex system is a bad idea, usually.

The hard part about software design is state. If you're storing any kind of information for any amount of time, you have a lot of tricky decisions to make about how you save, store and serve it. 

If you aren't storing info, your app is "stateless". GitHub has an internal API that takes a PDF file and returns an HTML rendering of it. That is a REAL stateless service. Anything that writes to a DB is stateful.

Try to minimize the amount of components (and in particular, stateful components) in any system. The reason you should do this is that stateful components can get into a bad state. Our stateless PDF-rendering service will safely run forever, as long as you're doing broadly sensible things -- running it in a restartable container so that if anything goes wrong it can auto be killed and restored. 

IN PRACTICE --> this means having one service that knows about the state -- i.e. it talks to a database -- and other services that do stateless things. Avoid having five different services all write to the same table.

Since managing state is the most important part of system design -- most important component is where the state lives: the database.

#### Schemas and Indexes
If you need to store something in a database, the first thing to do is define a talbe with the schema you need. 

Schema design should be flexible!!

#### Bottlenecks

Database access is often the bottleneck in high-traffic applications. This is true even when the compute side of things is relatively inefficient -- complex apps need to make A LOT of DB calls. 

When you query the database, *query the database*. Almost always more efficient to get the database to do the work than to do it yourself.

If you need data from multiple tables, `JOIN` them instead of making seperate queries and stitching them together in-memory. 

Send as many read queries as you can to database replicas. A typical DB setup wil have one write node and a bunch of read-replicas. The more you can avoid reading from the write-node, the better. That write node is already busy enough doing all the writes. Only exception -- when you can't tolerate ANY replication lag ( since read-replicas are always running at least a handful of ms behind the write node).

Beware spikes of queries (particularly write queries, and particularly transactions). Once a Database gets overloaded, it gets slow, which makes it more overloaded. 

#### Slow Operations, Fast Operations
A service has to do some things fast -- user API interactions, etc.
A service also has to do other things that are slow.

general pattern for this? -- splitting out the minimum amount of work needed to do something useful for the user and doing the rest of the work in the background.
- In the PDF-to-HTML example, you might render the first page of HTML quickly, and then background job the rest.

What is a "background job"? It's worth answering this in detail as they are a core design primitive. Every company has some srot of background job system!
- a collection of queues e.g. in Redis
- job runner service that will pick up items from the queues and execute them

Redis persistence is not guaranteed over long term. 


#### Caching
Sometimes an operation is slow because it needs to do an expensive (slow) task thats the same between users. -- Calculating how much to charge a user in a billing service, you might need to do an API call to look up the current prices. 

Classic solution here is caching -- only looking up prices every five minutes, and storing the value in the meantime. 

It's easiest to cache in-memory, but usuing some fast external KV Store like Redis or Memcached is also popular

Typical pattern --> Junior engineers want to cache everything, while senior engineers want to cache as little as possible. 
- Why? It comes down to the dangers of statefulness discussed earlier. -- A cache is a sort of state. It can get weird data in it, get out-of-sync with the actual truth, or cause mysterious bugs by serving stale data

Useful caching trick --> using a scheduled job and a document storage like S3 or Azure Blob Storage as a large-scale persistent cache

#### Events
Along with a caching infrastructure and a background job system, tech companies will have an event hub.

Most common implementation of this is Kafka. 

An event hub is just a queue -- like the one for background jobs -- but instead of putting "run this job with these params" on the queue, you put "this thing happened" on the queue.

Good example is a "new account created" event and then having multiple services consume that event and take some action: welcome email, scan for abuse, set up per-account infrastructure, etc. 
