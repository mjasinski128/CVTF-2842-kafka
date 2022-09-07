

https://www.youtube.com/watch?v=HtU9pb18g5Q

Kafka

Linkedin - web-click stream analysis




use cases:
- CDC
- microsevices

    - event-sourcing
        - time ordered sequence of events, instead of only state
        - 
    - CQRS
        - separate read (query)/ writes (command)
        - writes event sourced

    - Choreography
        - event trigger actions = choreography
        - versus centralised workflow = orchestration
        
    - Data store / event store 9:41
        - events in Kafka never expire
        - 
Features
- default 7 days expiry, but can be stored forever
- brokers 
    - instances/replicated machines
    - all topics split between brokers
    - producers should use key - i.e. session id to store togeter/otherwise load-balanced

    - state is stored in zookeeper

    -  difficult to setup and configure
    -  hard HA
    - no visible metrics
    - tricky to scale
    - operational excellence


deployment
    - K8S + Operator
        - work:
            - monitoring
                - integrate with cloudwatch / prometheus
            - logging
                - ELK ?
            - support
                - 
    - MSK
        - Serverless !!!
        - Provisioned


Ideas for service
    - provide KAfka monitoring metrics to client team - how to make promethues data available?
    - export logs to client team
    - 


Pain points

vs kinesis

MSK
- 

events vs messages



MSK best practices
-  

=========
https://www.adservio.fr/post/kafka-patterns-and-anti-patterns


=========

intro

    - CCOE:
        - not expert on Kafka
        - expert on cloud

        - knowledge from other projects
        - multiple cloud providers

        - relationship with cloud providers
            - need consultation on MSK?
        

=========


Requirements:







=========        

- questions

    - objectives on this projects

        - experingemt for different approaches
        - build PoC
        - validate architecture decisions
        - support affords
        - create account?
        - create VPC

====

    - explain use case
        - reasons to keep kafka, rather than use kinesis

    - AWS service canididates
        - best candidates
        - rulled out 
            - EC2?
            - EKS?
    - preference for managed service - Which is the right environment? (strong preference for PaaS)


    - ability to customize config per client project
    - requirements: throuput how defined: global/ per project

    Security
    - SNC in messsages
    - encryption requirements
    - HSM?
    - encryption per client or central?
        - any specific requirements?
    - KMS - in multi use scenario - will client need to have access to key?

    - specific challenges / pain points? from past solution


    Sizing:
    - max supported throughput
        - msg per/sec
        / number of consumers/producers

    - number of clients/clusters
    - isolate test/dev/ prod clients ?

    - authenitcation/auth mechanisms - switch to IAM ?
        - who manages ACLS


    Architecture

    - kafka replication/hub and spoke ? 
        - protocol
        - scenarios / DC-Cloud/ saas -cloud ?

    supported connectivity patterns in AWS
    
        - who can connect
        - protocol

        - separate VPC
        - another Region
        - account with multiple VPC, regions (peering?)

        - how connect other AWS projects = VPC peering/VPC transit gateway?

        DR plans
        - replication to secondary region?
        - access from public internet, possible but is it required?

        Who is going to connect to that service? From where [DC, Azure, AWS, SaaS systems]?

                supported connectivity patterns in Azure/OVH?

                - privte ?
                - public ?
                - Equinix ?

                Can this work on its own, or it requires also a managed networking service that will facilitate those interconnections?

                Is it the right protocol? (From a networking perspectie, everything that's not https is more of a pain))


    Cost
    - cost expectations - shared clusters or dedicated?

    - serverless donest support KMS CMK/ only AWS provided
    - defend against abuse/bad config

    - MSK limitations: https://docs.aws.amazon.com/msk/latest/developerguide/limits.html



    Functionality exposed to clients
        - cluster configuration?
        - monitoring metrics?

        - connectivity/security groups?

        Is it a good pattern? (as some shared services sure, but should it be a single Kafka ESB with everything depending on it, or multiple instances for different domains?). When it is a good pattern? (I think right now Kafka is abused both as an event and a messaging platform, and we might need to decide)


        If they opt for one central kafka, central services are in the "hub" of a hub-and-spoke, so that it's easy to access them from the Organization's "spoke" environments. It might be worth discussing with C4 (Panagiotis Kritikos...) what is the best topology. 


    - does data need to be moigrated from existing brokers? 
        https://aws.amazon.com/msk/faqs/ - Q: Can I migrate data within my existing Apache 
        
            Kafka cluster to Amazon MSK?
            Yes, you can use third-party tools or open-source tools like MirrorMaker, supported by Apache Kafka, to replicate data from clusters into an Amazon MSK cluster. Here is an Amazon MSK migration lab to help you complete a migration.

Other
    - IaC


===========

    https://aws.amazon.com/blogs/big-data/how-goldman-sachs-builds-cross-account-connectivity-to-their-amazon-msk-clusters-with-aws-privatelink/
        pros:
        - PrivateLink vs peering vs TransitGateway
        cons
        - 2 stage deployment (2nd in client account)
        tbc:
        - abillity to rebuild master account without dropping resources in client acct?
        - does it support connecting multiple clusters?


    https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-limits-endpoints.html

    https://aws.amazon.com/blogs/big-data/secure-connectivity-patterns-to-access-amazon-msk-across-aws-regions/


    DR plan
        - failover to secondary region?
        migrate data, restore from backups?
        - RTO, RPO
        - 



    - https://www.adservio.fr/post/kafka-patterns-and-anti-patterns
    - 1. plan durability
        - Kafka not optimized for durability by default
        = ack=all to wait for ack from all replicas
        = by default only ack for for 1 replica

    - 2. retires
        - impact on duplication
        - default retries is 0
        - indempotence - enable!
    - 3. indempotent customers
        - kafka is at least once > exactyl once > at most once
        - manula commit  - adds llad but doesnt make it exactly once!

    - 4. eexceptions 
        - plan

    - 5. additional Anti-Patterns

        The above represents the most common anti-patterns, but the following should also be on your list to avoid:

        Failing to implement data governance
        Not leveraging security, access control list (ACL), or quota
        Not configuring your operating system
        Ignoring Apache ZooKeeper
        Failing to understand ordering
        Failing to use monitoring
        Using too many or not enough partitions
        Calling external services in Kafka streams
        How to Successfully Implement Kafka
        Kafka is built on top of simple principles that when combined together allow building a wide range of applications.

        Kafka is also an essential component to build reactive systems because it is;

        Message-driven,
        Resilient (reliable message storage + replication),
        Elastic (partitioning), and
        Responsive (consumer groups).





    ======

    - book a meeting with AWS solution architects??

    - POC
        - private link for MSK
        - access from clinet VPC
        - perring in clinet VPC - also accessing service on private link
        - 


=====

https://medium.com/@stephane.maarek/an-honest-review-of-aws-managed-apache-kafka-amazon-msk-94b1ff9459d8


https://pages.awscloud.com/rs/112-TZM-766/images/EV_build-apache-kafka-applications-not-kafka-scaffolding_Mar-2021.pdf

responsibility - page 13

Amazon MSK uses Amazon EBS server-side encryption
and AWS KMS keys to encrypt storage volumes

https://docs.aws.amazon.com/msk/latest/developerguide/bestpractices.html


https://aws.amazon.com/blogs/big-data/increase-apache-kafkas-resiliency-with-a-multi-region-deployment-and-mirrormaker-2/



=====

MSK limitations
- can not resize running cluster (confirm!)
- cost


    - https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html
        only 50 endpoints per region


==========








