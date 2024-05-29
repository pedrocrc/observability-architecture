# Observability Architecture

## Context

I realized that architecting a comprehensive observability solution is a task many people struggles with. Because of that, I propose a well-architected framework for some topological scenarios. Each scenario intend to represent a relevant category of computing environment similar to the ones that I've found in the market after some research.

## Scenario 1 - on premises environment for a medium-size organization

### Environment assumptions

on premises environment comprised of:
* vmWare vSphere Cluster over an array of physical servers
* some Dell EMC Unity XT storages appliances
* a SAN network based on Brocade switches, connecting the storages and the servers
* Fortinet Switches and Firewalls with central management into Fortigate
* the vSphere cluster contains multiple VMs, some of them are Windows Server, and others are based on Linux
* there are some kubernetes clusters based on linux nodes. Some nodes are VMs on the vSphere cluster and some others are bare-metal physical servers
* all the physical servers are Dell with Dell idRac management interface
* there are severam databases and other softwares that are deployed among the environment
* there is a kafka cluster based on [Strimzi](https://operatorhub.io/operator/strimzi-kafka-operator) operator, which is installed into one of the kubernetes clusters
* there is a S3 cluster based on [Min.io](https://min.io)
* there is no availability to use public cloud services


### Proposed architecture
The proposed architecture consists on the following guidelines:
* the log delivery system must be reliable and logs should not be lost
* we intend to centralize logs into an unified bus where any solution may ingest or consume log entries, as long as it has the proper permissions to do so
* the logs may contain sensitive data and need protection against unauthorized access
* logs must be retained for long term and there must be ways to make queries against them, even though queries against historical logs may consume long time to process
* recent logs must be easily queried and all the queries must comprise with some latency and availability SLOs
* metrics must be stored for long term, bus it is acceptable to compress metrics to longer time periods for historic metrics
* metrics must be stored in unified format for many sources
* all the long-term stored data must be based on S3 standard on the [Min.io](https://min.io) cluster

### Architecture blueprint
![observability architecture blueprint](https://github.com/pedrocrc/observability-architecture/blob/main/architecture-1.png?raw=true "observability architecture blueprint")
