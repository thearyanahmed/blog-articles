Redis হল একটি key-value pair NoSQL data store বা database. So, redis এ আমরা key এর against data store করতে পারি । Redis সম্ভবত এখন সব থেকে fast database in terms of operation. সাধারণত redis RAM এ data রাখে। HDD বা SSD এর মত persistent datastore এ write operation থেকে RAM এ অনেক write করা অনেক fast.

উধাহরন হিসাবে HDD তে (SATA) 6GB/s এ write করতে পারছেন কিন্তু র‍্যাম এ 40GB/s এ । read in details here.

এবার আসি clustering এ। সহজে বললে cluster কে চিন্তা করলে ধরতে পারেন একটি group of something, doing some stuff where they are aware of each other. সেখানে এক বা একাধিক গ্রুপ leader (master) থাকবে, এবং at least একটা বা একাধিক worker instance থাকে যারা সবাই একটি leader (master) এর সাথে থাকবে আর master-worker বা salve গুল কে instruction দিবে । বাহিরের দুনিয়া থেকে ভিতরে কি হচ্ছে তা জানার প্রয়োজন নেই ।

প্রয়োজন নেই কেন? ধরুন আপনার ৫ টি আলাদা instance আছে। যে কোন একটি প্রোগ্রাম যা কিছু একটা করে। এই instance গুল তে প্রতি master এর জন্য ১ টা না ২০ টি slave আছে তা বাহিরের program এর কোন জানার প্রয়োজন নেই । আবার জেনে ঝামেলা কমবে না, বাড়বে।

কারণ, ধরুন master গুল port 1001,1002,1003,1004,1005 এ run করছে । এর পরে যদি আরো instance add করা হয় (ধরি 1006,1007 ) তাহলে বাহিরের application যে তার manually 1006 আর 1007 এ connect করা লাগবে । আবার যদি instance 1002 gracefully shutdown বা কোন কারণে down হয়ে যায় তাহলে বাহিরের application যতক্ষণ না ওই instance unregister করতেছে ততক্ষণ error পাবে যদি data আপনার instance গুল তে আছে ।

এছাড়া কাজের split হওয়ার বিষয় গুল master node গুল জানে, data কোথায় কে store করেছে তাও জানে । এছাড়া একটা single instance run করা risky, কোন কারণে crash করলে data/functionality serve করতে পারবে না । আবার একা serve করতে বেশি সময় নিবে ।

সুতরাং একটা কাজ কে গ্রুপ করে ভাগ করে দিয়ে complete responsibility group টার উপরে দিয়ে দেওয়া আর তারা নিজেরা একটা synchronized way তে responsibility গুল ভাগ করে নেয়া, কোন disaster হলে নিজেরা সেটা কে সামলানো আর maximum availability কে clustering বলতে পারেন ।

Redis cluster এ একটি master node fall করলে তার সাথে associated একটা slave master হিসাবে act করে data serve করে, ফলে 0 downtimes and no rebooting the system, no error response.
Redis cluster

A Redis cluster is simply a data sharding strategy.
Sharding

    The word “Shard” means “a small part of a whole“. Hence Sharding means dividing a larger part into smaller parts. Sharding is a type of partitioning in which a large data store is divided or partitioned into smaller data, also known as shards. Redis cluster automatically partitions data across multiple Redis nodes. It is an advanced version of Redis that achieves distributed storage and prevents a single point of failure.

![](https://miro.medium.com/max/1400/1*zE-eRbDoBZrLhQyBln6sYQ.png)


So এক কোথায় বড় data কে ছোট ছোট ভাগে ভাগ করে রাখা ।

There are 16384 hash slots in Redis Cluster, and to compute what is the hash slot of a given key, we simply take the CRC16 of the key modulo 16384.

Every node in a Redis Cluster is responsible for a subset of the hash slots, so for example, you may have a cluster with 3 nodes, where

Cluster এঁর node গুল সব সময় নিজেদের মাঝে gossip protocol এ কথা বলত এথাকে । এতে করে auto-discovery achieve করা যায় । যদি একটি নতুন node আসে বা চলে যায় তাহলে node গুল নিজেরা কথা বলেই সবাই কে জানিয়ে দিবে যে একটি node চলে গেছে বা join করেছে।

If A knows B, and B knows C, eventually B will send gossip messages to A about C. Then A will register C as part of the network and will try to connect with C.

When node A talks to the cluster by the gossip protocol, If a/any node fails, the cluster’s reply will be node X, Y responded okay but P, Q, R didn’t response.

এই তোহ । নিজের machine এ cluster implement করতে চাইলে here is a demo. The repository has a demo setup with python, where you can push data concurrently and read from them randomly.