<!--markdown-->
## Data Driven Debugging

![log](https://farm9.staticflickr.com/8640/16221210937_9b318f5a77_c.jpg)

In this article, I share my recent experience in data-driven debugging.
Data-driven debugging is a practice of using data to trouble-shoot software
defect efficiently. It is not a new idea at all. In fact, most of software
engineers do it already. But I think we could practice more consciously because
it may work to cut the debugging of complex systems from days to a few hours.

### Case 1: Web socket communications

Consider a pair of devices at a store like Walgreens. One is facing the cashier
and the other is facing the customer. As the cashier processes the purchase,
relevant information is displayed on both devices in different presentations to
aid the communication between the customer and the cashier.

Such system can be built with two tablets and a web socket server as shown in
Fig. 1. Because cashier-side tablet (Tablet B) needs to process some
kind of transaction, it sends Application Programming Interface (API) calls 
to the API server that reads and writes data to the database. The web socket 
server also use the same API and database to retrieve information of the mapping
of device pairs so it can handle many pairs in different stores.

![web socket system](https://farm8.staticflickr.com/7318/16219713070_e3a3544d54_c.jpg)
Figure 1 Paired tablets communicating via web socket server with potential
cause of failure. The red "bangs" indicate potential points of failure.

#### Many points of failure. So big hypothesis space.

This is fairly complex system. Each component from the tablet to the database
is subject to the logical failure, and each communication line is also a point
of failure.

If we get a report from the customer support team that many tablets stopped
talking to each other. What would we suspect?

- Was Tablet A not recovering web socket connection after boot?
- Was Tablet B not recovering web socket connection?
- Both tablets are connected to web socket but there is an application logic
  failure preventing the correct information to be displayed?
- Did one of the pair miss the handshake signals from the other?
- Was Store Wi-fi signal not reliable? (the tablets are connected to the
  internet via Wi-fi.)

In a complex system, the number of failure scenario (or hypotheses)
dramatically increases (i.e. the number of nodes plus the number of edges
connecting them). Investigating just one of the hypotheses could be a lot of
work. It could easily take days to get to the bottom of the issue if we
exhaustively investigate one hypothesis to another.

#### Reduce the hypothesis space based on the evidence

So, it is clever to reduce the hypothesis space before jumping into the
code-level investigation. Analysing log would help to achieve it.

Luckily, in our hypothetical case, we have a log from web socket server. After
extracting the portion of the logs relevant to the affected store and date,
we may notice a pattern as shown in Fig. 2.

![log analysis](https://farm8.staticflickr.com/7404/16381140766_43d0d4cdd4_c.jpg)
Figure 2: Log analysis

At the beginning, there was an existing connection from both Tablets A and B
to the web socket server at 18:05:45. A mapping between the tablets was
created just after the connection opened at 18:06:40.

Then strange thing happened. There was another open connection event by Tablet
A. This is odd because there wasn't a "close connection" event log before this.
It seems there are two open sockets for Tablet A at 18:22:56.

What happened next was Tablet A closes the connection just after receiving a
handshake message from Tablet B at 18:22:57. But strangely, Tablet A still
sent a message at 18:22:58. So, Tablet A wasn't disconnected after all?

Based on the log analysis, and the knowledge of the software and store
environment, one could think of the following failure scenario:

Store Wi-fi was weak as it often happens in the local retail store environment.
Suddenly weak signal caused a disconnection of the socket of Tablet A not in
unclean way. So, the disconnection wasn't recognized by the web socket server
immediately. Tablet A, regaining Wi-fi signal, sends an socket open request to
the web socket and it is accepted. When Tablet B sends a handshake signal, the
disconnection of the previous (and lost) socket (I call this a zombie socket)
was finally recognized by the web socket server. In this software, the mapping
information between Tablets A and B is removed upon the closing of the socket.
So, when the zombie socket finally died, the web socket was removing the
mapping information although another alive socket still existed for Tablet A.

As a result, the socket for both tablets existed, but mapping between the two 
was broken.

#### Lessons learned

This turned out to be exactly what was happening, and we could quickly fix it.
Even if this had not been the case, we could rule out many scenarios that does
not conform to the evidence from the log.

Lessons learned:

1. Try reducing hypothesis space first based on evidence when debugging a
   complex system.
2. Make sure that the application logs at each critical step.
3. Send the client log to the server.

### Case 2: Discrepancy between two data sources

The second example is about data discrepancy between systems. Suppose we need
to send the account information on the proprietary database (SQL) to Salesforce
so that the sales department can use it for the sales and customer retention
efforts.

One day, we receive an email saying that the data in the SQL and Salesforce
have a discrepancy. We first need to investigate by comparing the data between
the two sources.

#### Analysing data from multiple sources

This case is not so much about reducing the hypothesis space. Instead we needed
to carefully compare the data between two systems: SQL and Salesforce.  

What tool do we use to do it to start analysing? It may sound very strange to
application programmers, but my choice was R. R has packages to connect to
PostgreSQL as well as Salesforce. So without hopping one console to another,
I can send queries to both SQL and Salesforce. I can load the data on the data
frames, join them to analyze in a integrated environment. Figure 3 shows a
very simple example code that fetch data from both sources and merge them.

![r markdown](https://farm8.staticflickr.com/7282/16407123515_9ef3bcc20a_z.jpg)
Figure 3 R has packages to fetch data from various sources. R-markdown let us
document as we analyse for the reproducibility.

#### Documentation is the key. The key is to do it painlessly.

In the course of data analysis, we can easily make mistakes in writing a
complex queries. Wrong query leads to wrong data. Wrong data leads to a wrong
conclusion. It is very important that we can revisit the steps of the analysis
later, and get it peer-reviewed.

It is therefore very important that the queries we write do not get lost, and
we have a clear documentation for others to review and reproduce the analysis.
However, I found documentation to be very hard if I do it as an independent
task after the main task is done. I easily forget the details of the thought
process. I lose momentum for executing the task once I think I have seen the
result.

R markdown let us write R code for data analysis, and use the result to compose
the narrative of the analysis in the same text file. As long as I leave
a note for each steps of data retrievals and transformations, I can produce a
report with a push of a button. For example, R markdown file in Fig. 3 would
produce a report that look like this:

> ### Investigation of discrepancy between SQL and Salesforce
> **Daigo Tanaka**
>
> **January 21, 2015**
> 
> #### Active records
> Number of active accounts in Salesforce is 1251.
> Number of active accounts in SQL is 1328.
> Number of active accounts in both databases 1237.

All the data (i.e. the number of accounts) is "soft-coded" and compiled upon
the conversion from R markdown to a human readable document format (e.g. HTML,
PDF, and etc) The whole text in R markdown is the source code to reproduce the
analysis, and generate the report. So, R markdown let us document everything we
do, as we do it.

#### Lessons learned

In the enterprise solutions, we often have to deal with the data from multiple
sources such as SQL, Salesforce, Logs, NoSQL, and so on. Having to hop one
console to another just to retrieve the data is disruptive to the analytical
thought process. As the analysis get complexity, we will make mistake in
writing queries and code to transform data. Leaving the code to reproduce and
document the steps of analysis is critical.

1. Have an integarted data analytics environment that can load and merge data
   from multiple sources.
2. Do not use graphical use interface (i.e. mouse clicks and spreadsheet
   program) as it does not leave the clear steps of the data retrieval and
transformation at the end of the analysis.
3. Find a tool that document the analysis at least semi-automatically for
   the reproducibility, and have the analysis peer-reviewed.

### Conclusion

In this article, I advocated the thought framework of Data Driven Debugging.

When troubleshooting a complex systems or do a data intensive analysis, we
should try reducing the hypothesis space with evidence (e.g. log) before jumping 
into code when we troubleshoot a complex system.

In the enterprise solutions, we should be ready to deal with data from multiple
sources. Having an integrated environment reduces the disruption to the
analytical thought process. As the analysis gets more complexity, it is also
crucial to get our analysis peer reviewed. Tools such as R-markdown to
semi-automatically document as we run the ad-hoc analysis facilitates the
process.
