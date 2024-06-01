0x09. Web infrastructure design
Concepts
DNS

Monitoring

Web Server

Network basics

Load balancer

Server

What is a database

What’s the difference between a web server and an app server?

DNS record types

Single point of failure

How to avoid downtime when deploying new code

High availability cluster (active-active/active-passive)

What is HTTPS

What is a firewall

Application server vs web server

Tasks
0. Simple web stack
Description
This is a simple web infrastructure that hosts a website that is reachable via www.foobar.com. There are no firewalls or SSL certificates for protecting the server's network. Each component (database, application server) has to share the resources (CPU, RAM, and SSD) provided by the server.

Specifics About This Infrastructure
What a server is. A server is a computer hardware or software that provides services to other computers, which are usually referred to as clients.

The role of the domain name. To provide a human-friendly alias for an IP Address. For example, the domain name www.wikipedia.org is easier to recognize and remember than 91.198.174.192. The IP address and domain name alias is mapped in the Domain Name System (DNS)

The type of DNS record www is in www.foobar.com. www.foobar.com uses an A record. This can be checked by running dig www.foobar.com. Note: the results might be different but for the infrastructure in this design, an A record is used. Address Mapping record (A Record)—also known as a DNS host record, stores a hostname and its corresponding IPv4 address.

The role of the web server. The web server is a software/hardware that accepts requests via HTTP or secure HTTP (HTTPS) and responds with the content of the requested resource or an error messsage.

The role of the application server. To install, operate and host applications and associated services for end users, IT services and organizations and facilitates the hosting and delivery of high-end consumer or business applications

The role of the database. To maintain a collection of organized information that can easily be accessed, managed and updated

What the server uses to communicate with the client (computer of the user requesting the website). Communication between the client and the server occurs over the internet network through the TCP/IP protocol suite.

Issues With This Infrastructure
There are multiple SPOF (Single Point Of Failure) in this infrastructure. For example, if the MySQL database server is down, the entire site would be down.

Downtime when maintenance needed. When we need to run some maintenance checks on any component, they have to be put down or the server has to be turned off. Since there's only one server, the website would be experiencing a downtime.

Cannot scale if there's too much incoming traffic. It would be hard to scale this infrastructure becauses one server contains the required components. The server can quickly run out of resources or slow down when it starts receiving a lot of requests.

1: Distributed Web Infrastructure
Description
This is a distributed web infrastructure that atttempts to reduce the traffic to the primary server by distributing some of the load to a replica server with the aid of a server responsible for balancing the load between the two servers (primary and replica).

Specifics About This Infrastructure
The distribution algorithm the load balancer is configured with and how it works. The HAProxy load balancer is configured with the Round Robin distribution algorithm. This algorithm works by using each server behind the load balancer in turns, according to their weights. It’s also probably the smoothest and most fair algorithm as the servers’ processing time stays equally distributed. As a dynamic algorithm, Round Robin allows server weights to be adjusted on the go.

The setup enabled by the load-balancer. The HAProxy load-balancer is enabling an Active-Passive setup rather than an Active-Active setup. In an Active-Active setup, the load balancer distributes workloads across all nodes in order to prevent any single node from getting overloaded. Because there are more nodes available to serve, there will also be a marked improvement in throughput and response times. On the other hand, in an Active-Passive setup, not all nodes are going to be active (capable of receiving workloads at all times). In the case of two nodes, for example, if the first node is already active, the second node must be passive or on standby. The second or the next passive node can become an active node if the preceding node is inactive.

