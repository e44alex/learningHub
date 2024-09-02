# MongoDB - Our path

**Introduction**: Hello everyone, For those who don’t know me, my name is Alex Petrachenka. I’ve been working for almost 4 years from now on, and the fact is, all that time I was working on a single FinTech project

The reason you see me today is that I was very lucky to get into the team of high-skilled software engineers, and all together we had a great chance to migrate one legacy application to MongoDB. For me, that was a very exciting experience, and today I’m thrilled to share my story with you.

**Plan:**

1. What we had before. A story of one nasty SQL database
2. How we chose the MongoDB
3. Our first steps in MongoDB and what we’ve found about it
4. How MongoDB let us to create a highly available, yet reliable financial app

# Chapter 1 - It all began with the SQL

But the case, we are interested in, began its existence with one nasty SQL database. The story of the existing SQL database lasted since the very 2007, and by the moment the Vention team (and me ofc) got acquainted with it, its structure was terrifying.

Here are a couple of details about the project to get the big idea:

1. Multitenant application — tenants are added through time, and each tenant database has specific requests, still single app deployment needs to handle it
2. Largest data sets — around a few million, not that impressive, it wasn’t a big data
3. Datasets are under constant recalculations, each update may trigger the recalculation of a significant set of entities. That means we needed to effectively search the data and minimize recalculated data as much as we could.

So, for those of you, who were already working with SQL, you may know 2 things for sure:

1. SQL databases are great. No, really. It is hard to imagine something so widely spread and popular. And I can barely imagine, how many solutions were built on top of SQL. The story of SQL has lasted for more than 50 years now, that’s quite impressive.
    
2. To get the most out of your SQL database, you have to put some serious effort into architecting your data schema. You have to plan and maintain, to make sure you’re following the database normalization forms and best practices. And if you do, most likely you get something as beautiful and simple as that:
    

![Some simple SQL database structure, well organized, easy to navigate, and easy to maintain](https://prod-files-secure.s3.us-west-2.amazonaws.com/b64965ef-3f5f-45ec-b6e9-9be9099de2e9/9e7f2f68-a702-4cb8-be61-8fe95274f86c/Untitled.png)

Some simple SQL database structure, well organized, easy to navigate, and easy to maintain

Now, if you try to compare that database structure with, for example, Northwind, or some well-structured and built database, maybe the first thing you’ll see, there are some tables, called **party-something.** And from the semantical perspective, you may wonder, what the hell does that mean? At that moment, I was like “Okay, I do understand what the accounting period is, what the employees, users, roles, and demographics tables are. But how do they relate to each other? How do I get employee records in terms of one accounting period?” What the hell is going on…

Now you can imagine some common issues with that kind of database

1. Investigation time — I’d put that thing on top of everything because no matter what you’re going to do with that database, first of all, you need to do plenty of investigation and figure out, how you gonna do that. Are you fixing a bug? — most likely you’re gonna spend some serious time querying that database, finding relations, and understanding, why have you lost some employees in the accounting period.
2. How do you get this data? The other pitfall is you can’t simply query the data from the database. through the backend and get to a UI. So as you’re trying to get some data from the database and show it on your application, most likely you’re going to find yourself writing Stored Procedures. And we all know what is like to debug a stored procedure
3. Scalability and performance. Now, as you may know, the SQL database itself is quite fast. You may get millions of records in under a few seconds if your schema is well-architected. But well architecture wasn’t the case. Due to the high customization of tenant applications, one of the requirements of the database structure was to keep it as flexible as possible. That means if you need to add a new entity, you need some abstract way to describe it. That causes many-to-many relations, redundant joins, and a ton of unnecessary logic just to get a few records.

All those things were making us think that we were doing something wrong. If our data looks like a duck and quacks like a duck, then maybe we should keep it as a duck. Ideally, something JSON-like would suit us. But can we put it into SQL? We could. Technically, we could keep some of the data in plain JSON files or even as binary data, and then deserialize it when we need it. Normally, It would mean database denormalization, and that’s a red flag, but we had to do something about it…

But the specific part is, that data wasn’t just static, users were updating those documents quite often. We were also urged to search, filter, and project those data for various calculations. So even if we would try and keep it as JSON in our SQL database, we most likely had to replace the whole document. And that was quite a problem. We realized, we needed something, that could index those JSONs, something that could atomically update it. So, it looked like, we needed a document-oriented database for that reason.

Now, from the document-oriented databases, we were choosing between CosmosDB and MongoDB. From further investigations, we found out, that CosmosDB has a major red flag. As it is using a portable floating point numbers standard, Cosmos is truncating float values. As we were working with financial data and calculating it, we couldn’t afford to lose any digits, that would cause so many problems. So, MongoDB was our choice.

# Chapter 2 - MongoDB overview… TODO…

So for those who don’t know anything about Mongo, these are a few key notes to understand, what’s going on inside of it:

1. MongoDB is a document-oriented database. Apart from SQL, which describes how data should be stored in terms of tables and columns, MongoDB stores your data in BSON - Binary JSON
2. Inside the database, MongoDB stores documents in collections. A Collection is often considered a set of documents with a similar structure. Although, Mongo doesn’t have any restrictions on that.
3. Each document has a restriction of 16MB. Usually, it is quite a lot, but you may consider it when developing your document schemas.
4. To work with the database, Mongo serves you its specific API. Among all the commands, it is something called Aggregation Framework — a very profound tool to effectively work with data inside of MongoDB. And, if you’re a backend engineer — Mongo serves you Mongo.Driver to work with database commands from your backend.
5. Every time you update a document, MongoDB makes so-called In-place updates. It understands, what part of a document you need to update, and then updates only required bytes. That’s much faster than replacing the whole document.

But what attracts us to MongoDB, there are 2 key concepts, we found quite useful;

1. We have a flexible schema. So, Mongo doesn’t require you to have some kind of strict pre-defined schema in your database, as SQL does. With that, we felt real freedom in developing a flexible application and working with dynamic data structures.
2. There is a completely different approach to keeping related data. With MongoDB, you can keep related data right in your host document. Simple as that. No more joins are required, and you can be flexible with designing your data storage from the needs of your application.
3. Hierarchical problems — Because of a flexible document structure it is much easier to store hierarchy trees in the database. And that was a thing in the HRM system when you’re trying to keep the hierarchy of supervisors in your app. Previously in SQL, it took us either writing a ton of SQL code with some limitations in a degree of nesting or recursively calling SQL again and again and again… In Mongo, we can store the tree as it is and then just pick it up. (But you should consider max amount of 100 steps depth)

Another useful feature of MongoDB we found out about is the GridFS. So, as Mongo stores your data in binary format, technically it became possible and quite convenient to keep some kinds of files right inside the database. Simple as that, mongo takes your file, breaks it byte array into chunks, and saves it along with a header, that helps you to retrieve the blob. And of course, that header is customizable, so you can add your custom metadata and simplify future searches. (But, as we figured out later, that was much slower, than keeping the files in blob storages, so probably, you should be careful with using GridFS)

1. Atlas — MongoDB Atlas offers several benefits, including fully managed database services that handle complex operational tasks like backups, scaling, and monitoring, allowing developers to focus on building applications. It provides high availability with automated failover, security features like encryption and access controls, and seamless integration with various cloud providers. Atlas supports multi-region deployments for global applications, automated performance optimization, and real-time analytics. Additionally, it offers a flexible, pay-as-you-go pricing model, ensuring cost efficiency and scalability to meet varying workloads and business needs.
2. Replication — Mongo allows you to deploy a set of DB instances and use this technique to scale your database horizontally. What’s probably the best part of the replica set, it is self-managed, so MongoDB elects your primary node in case of primary instance failure.
3. **Change Streams**: This allows applications to access real-time data changes, enabling reactive programming and real-time analytics.
4. **TTL Indexes**: Time-to-live (TTL) indexes are used to automatically delete documents from a collection after a certain period, useful for managing the expiration of data.

Apart from everything else, here are some key features we found out in MongoDB:

With that, we have developed our first MVP and that was quite impressive…

# Chapter 3 - Is MongoDB good?

|**Aspect**|**SQL (Relational Databases)**|**MongoDB**|
|---|---|---|
|Data Model|Structured (Tables)|Flexible (Json-like documents)|
|Scalability|Vertical scaling (additional hardware resources)|Horizontal scaling (distributed across multiple servers)|
|Query Language|SQL|MongoDB query lang + Aggregation Framework|
|Schema Flexibility|Rigid schema, requires alterations for schema changes|Flexible schema, supports unstructured and evolving data|
|Transactions|ACID-compliant (strong transactional support)|BASE|
|Use Cases|Complex querying, structured data, transactions|Unstructured data, high scalability, real-time analytics|

While we were investigating MongoDB and it’s capabilities, useful for our application, we found quite an interesting thing.

As some of you may know, SQL databases follow the ACID principle. ACID stands for Atomicity, Consistency, Isolation and Durability. All those principles make your SQL database a solid tower, so you don’t need to worry much about your data while building applications on top of SQL. That reliability is quite useful in commerce applications, such as ERP systems (like, SAP), or financial applications, just like our project.

MongoDB, on other hand, follows the BASE principle. BASE stands for Basically Available, Soft state, Eventual consistency. If you dig into that principle, it may seem like a time-ticking bomb to you. It may look like Mongo highly available and scalable, however, it trades-off reliability instead.

But while MongoDB follows the BASE principles to achieve high availability and scalability, it incorporates several features to ensure reliability:

- **Replication**: Data redundancy and automatic failover through replica sets.
- **Durability**: Journaling and transaction support to ensure data persistence.
- **Consistency**: Flexible read concerns and eventual consistency mechanisms to balance performance and reliability.
- **Monitoring and Maintenance**: Tools and services for monitoring, backup, and automated maintenance.

# Conclusion

So that was the story of one team

But apart from that, it was worth it. Switching to a No-SQL database and taking some unknown and non-traditional path was very enlightening. What’s more important, it showed us, that there is a life with a No-SQL database, if you know how to architect it well.

So, what we achieved with this migration so far:

1. We have resolved our issues with dynamic data schemas. The flexible schema of MongoDB allows us to effortlessly develop new features and fulfill clients’ requests. We no longer have any business logic on database level, and that’s much simplifies the development process.
    
2. As the project grows, we have a solid base for database scaling. We can scale out horizontally by adding new replica nodes
    
3. For performance purposes, we still can use sharding
    

Broaden your knowledge, and don’t be afraid to experiment

Thanks for your attention.

**FEEDBACK:**

1. ✅транзакционность: ACID vs BASE
    
2. ✅ если происходит апдейт аттрибута, что происходит —
    

When you update a field in a document, MongoDB does not replace the whole document by default. Instead, it performs in-place updates, modifying only the relevant bytes on the disk. Here’s a detailed look at how updates work:

### In-Place Updates

- **Modifying Existing Data**: If the update modifies fields in such a way that the overall size of the document does not change or the change is within a certain threshold, MongoDB updates the document in place. This involves writing the new values directly to the existing location on disk.
- **Performance**: In-place updates are efficient and minimize the amount of I/O required, leading to better performance.
- That leads us to conclusion, that having documents in MongoDB could be more write-efficient, rather than keeping documents in

3. сравнение запросов.
    
4. рекомендации, когда стоит использовать SQL а когда стоит использовать NoSQL. Развить тему.
    
5. ✅ углубиться в проблему проекта, про объем данных, какой был эффорт.
    
6. ✅ какие ставились задачи, является ли структура динамической,
    
7. ✅ взаимодействие с монгой с точки зрения аналитических запросов.
    
8. ✅ как вы выбирали монгу, раскрыть тему, почему именно монга … зачем монго
    
9. ✅попробовать перевести это в историю
    
10. про выводы: вернуться
    
11. жизнерадостность)
    

TODO:

1. ACID vs BASE. What did Mongo does to achieve performance.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b64965ef-3f5f-45ec-b6e9-9be9099de2e9/1596d028-3cb0-4b5c-bf10-2b6cac956e57/Untitled.png)

- - MongoDB was a perfect match with OOP, it can naturally understand the class structure, whereas SQL required some mappings. In our case, for some entities, ORM mapping was not even possible, as we were not able to predict the structure and fields.
- MongoDB is easy to scale. In comparison with SQL, MongoDB has a built-in replication mechanism. That allows us to add new nodes to increase the total query performance. However, this has a trade-off, called Eventual consistency. Theoretically, it is possible to get in a small period of time, when we’re reading from secondary nodes, that does not contain updates from the primary…
- For big data projects, MongoDB has sharding: a mechanism of splitting data into shard chunks, to optimize data queries

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b64965ef-3f5f-45ec-b6e9-9be9099de2e9/e9b4baa6-d4ab-43e4-a48b-50b7f37f81c9/Untitled.png)

TODO: make some pics of dynamic documents, to show, how can they vary and how dynamic they are…

Absolutely! Here are some tips for crafting your story about choosing MongoDB over SQL and migrating your project:

### 1. **Define Your Audience**

Understand who your audience is. Are they technical experts, business stakeholders, or perhaps a mix of both? Tailoring your story to their interests and knowledge level is crucial.

### 2. **Start with a Hook**

Grab attention right from the beginning. You can start with a surprising fact, a compelling statistic, or an intriguing question. For example: "Did you know that the flexibility of MongoDB can reduce development time by up to 40%?"

### 3. **Set the Context**

Explain the background of your project:

- What was the project about?
- What were the initial requirements and challenges?
- Why was the choice of database technology crucial?

### 4. **Present the Problem**

Clearly state the issues you faced with your SQL database:

- Performance bottlenecks?
- Scaling issues?
- Complexity of handling unstructured data?

Example: "We were struggling with our SQL database's inability to handle the growing volume and variety of our data. Query performance was deteriorating, and schema changes were becoming a nightmare."

### 5. **Introduce MongoDB as a Solution**

Describe why you considered MongoDB:

- Flexibility with schema design?
- Easier horizontal scaling?
- Better performance for specific workloads?

Example: "After researching various alternatives, MongoDB stood out due to its schema flexibility and horizontal scaling capabilities, which seemed perfect for our rapidly evolving data needs."

### 6. **Detail the Decision Process**

Share insights into how you evaluated MongoDB against SQL:

- Proof of concepts?
- Benchmarks and performance tests?
- Feedback from the team?

Example: "We conducted several benchmarks comparing MongoDB with our existing SQL setup. The results showed a 30% improvement in read and write operations, making it a clear winner."

### 7. **Explain the Migration Process**

Describe the migration journey:

- Planning and strategy?
- Tools and techniques used?
- Key challenges and how you overcame them?

Example: "The migration was planned in phases. We used tools like MongoDB's Atlas Data Lake for seamless data transfer and followed best practices to ensure data consistency and minimal downtime."

### 8. **Highlight the Benefits Realized**

Share the positive outcomes post-migration:

- Performance improvements?
- Easier maintenance and scaling?
- Increased developer productivity?

Example: "Post-migration, we saw a 50% reduction in query times and a significant boost in developer productivity, thanks to MongoDB's flexible data model."

### 9. **Include Lessons Learned**

Reflect on the experience and share key takeaways:

- What went well?
- What would you do differently next time?

Example: "One major lesson was the importance of thorough testing before full-scale migration. In hindsight, investing more time in initial data modeling would have streamlined the process even further."

### 10. **Conclude with Future Prospects**

End with a forward-looking statement:

- How will MongoDB continue to benefit your project?
- Any plans for future enhancements?

Example: "With MongoDB, we're well-positioned to scale our application effortlessly as our user base grows. We're excited about leveraging MongoDB's advanced features like sharding and full-text search to further enhance our application."

### 11. **Use Visuals**

Where possible, incorporate diagrams, charts, and screenshots to illustrate points. Visual aids can help make complex technical details more accessible.

### 12. **Keep it Engaging**

Maintain a narrative flow, using a mix of technical details and storytelling techniques to keep your audience engaged.

### Sample Outline

1. **Introduction**
    - Hook
    - Brief project background
2. **Problem Statement**
    - Issues with SQL
3. **Evaluation of Alternatives**
    - Why MongoDB?
4. **Decision-Making Process**
    - Benchmarks and tests
5. **Migration Strategy**
    - Planning and execution
6. **Outcomes**
    - Benefits and improvements
7. **Lessons Learned**
    - Key takeaways
8. **Conclusion**
    - Future prospects

By following these steps, you'll create a compelling and informative story that clearly communicates why and how you transitioned from SQL to MongoDB, along with the benefits realized from this decision.

![[MEETUP PRESENTATION.pptx]]