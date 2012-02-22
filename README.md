## Real-Time Monitoring Spike

### Past Production Issues

These are a few of the issues that have occurred in production. Many caller related issue are surrounding issues getting backend services from DAS or ESP.

* 9-29-2011: Low virtual memory on Windows machines.
    * This issue can occur on Linux machines as well as windows machines due to low memory, or issues with excessive load on the machine resources.
* 9-8-2011: Appointment and Outage Webservice failure inconsistently.
    * This is an issue where failing webservices are affecting caller experience but nobody is actively notified.
* [date]: Payment authorization failures on backend services.
    * This is an issue where failing webservices are affecting caller experience but nobody is actively notified.
* [date]: Outbound dialer data load failures
    * This is an issue where failing FTP services are affecting outbound date loads but nobody is actively notified. The last time this occurred the issue was not found for almost 5 days after the issue began.
* [date]: Outbound dialer calls not being initiated
    * This is an issue where failing FTP services are affecting outbound date loads but nobody is actively notified. The last time this occurred the issue was not found for almost 5 days after the issue began.
* VIP Servers going offline
    * This is an issue where failing application server and nobody is notified. Callers should be minimally affected but operations might like to have this information.

### Metrics that can be collected

These are some of the data elements that can be collected from a productions system.

* Callers beginning callflow
* Caller ending callflow
* Callers failing identification
* Payments failing
* Callers failing other DAS Services
* Consolidated record of Systems (DAS, ESP, MTI, MPP, Prompts, ASR, TTS) callflow has interacted with.
* FTP and Data load details.
* Outbound call reports.
* MPP call failure
* MPP ASR failure
* MPP TTS service failure
* MPP Application server access failure
* MPP Prompt server access failure
* VIP Server startup / shutdown.

### Alerting and Reporting Requirements

Reporting needs to be done real-time, daily, weekly, monthly and indefinitely. The metrics collected can be alerted or reported on in many different ways: 

#### Alerting

* SNMP
* Email
* Text/SMS

#### Access to reports:

* Email
* HTML
* Rest services for JSON data access
* CXF Web services
* SQL data access (CXF or Rest)

### Design Solution Options

I believe there are two design options. Generic log4j logging (1), and advanced enterprise asynchronous messaging (2).

#### Design Options

* Consolidated Log4J: Using log4j could be a drop-in logging solution that would allow XIVR, DAS and any other System on the Comcast network to send Log4j based logs for consolidation and aggregation. This is less work for creating the messages, and mostly work aggregating data. But this is not as flexible as far as data format and post processing of data. This could also be combined with an asynchronous solution to offer a comprehensive reporting solution. But would still not be as robust as a full messaging solution.
* Asynchronous Messaging: A natural design for this distributed system such as we have at Comcast. This would be the creation of custom messages.  This would allow the creation of metric messages as well as aggregation and transforming messages into various reports
I believe using log4j as the initial metric message creator and using an asynchronous messaging system for parsing and transforming the data would be the best initially and less work. We could then use the asynchronous framework to be a message generator for more complex metrics patterns.

I believe using log4j as the initial metric message creator and using an asynchronous messaging system for parsing and transforming the data would be the best initially and less work. We could then use the asynchronous framework to be a message generator for more complex metrics patterns.

[INSERT DIAGRAM HERE]

### Potential Tools

Depending upon the chosen design, there are several tools that can be used.

#### Frameworks

* Camel: Asynchronous messaging framework
* Hadoop: The Apache Hadoop software library is a framework that allows for the distributed processing of large data sets across clusters of computers using a simple programming model.
* Log4Mongo Log4J appender: library for sending log4j logs to a MongoDB document database.

#### Messaging Servers

* ActiveMQ: Asynchronous Messaging server

#### Databases

* MongoDB: Document oriented database
* HBase: Big Data database alternative to MongoDB
* FileSystem: Hadoop can actually process files on distributed filesystem if needed.

#### Technologies already prototyped

Several of the technologies above have already been prototyped and can be leveraged in further POC Spikes or implementations.

* Camel: Event processor workflow completed.
* ActiveMQ: Event processor workflow completed.
* MongoDB: MongoDB has been installed and is receiving data in Staging server.
* Current issue with Mongo. Data processor must be created to aggregate incoming data and to provide Alertable event processing and Reportable event processing. 
* Log4Mongo: log4mongo has been installed and is sending data to mongoDb on Staging server.

#### References:

* http://research.google.com/archive/bigtable.html
* http://hbase.apache.org/
* http://www.mongodb.org/
* http://hadoop.apache.org/





