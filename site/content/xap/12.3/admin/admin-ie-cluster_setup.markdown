---
type: post123
title:  Cluster Setup for InsightEdge
categories: XAP123ADM,PRM
weight: 100
parent: admin-insightedge.html
---

This topic explains how to install and run InsightEdge on a cluster.

# Starting the Whole Cluster

Your cluster should consist of one master node and several slave nodes for the following configuration:

* Master nodes usually have the Spark master and the data grid management running
* Slave nodes have Spark workers and data grid cluster members running on them


The master nodes start XAP Manager along with Apache Zookeeper for high availability. Therefore you need to configure the `XAP_MANAGER_SERVERS` on each machine. Please refer to the [XAP Manager](../admin/xap-manager.html) page for more information.

In addition, it is recommended to set the `XAP_LOOKUP_GROUPS` property that is used to discover XAP components across the network. Refer to the [Environment Variables](../started/common-environment-variables.html) page for more advanced configurations.

These configuration parameters can be set in the `<XAP_HOME>/bin/setenv-overrides.sh/bat` file.

## Starting locally

The run-agent command automatically resolves which service to run on the current host.
The resolution is based on the `XAP_MANAGER_SERVERS` environment variable, but when undefined it will use localhost as the server IP.

```bash
<XAP-HOME>/bin/insightedge host run-agent --auto
```

This command will run a XAP Manager, Web Management Console, Spark master, Spark worker and the Zeppelin interpreter.

REST URL - http://localhost:8090
Web Management Console - http://localhost:8099
Spark master - http://localhost:8080/
Spark worker - http://localhost:8081/


## Starting Master Nodes

Master nodes consist of a XAP Manager and a Spark master. On each master node, run the following:

```bash
<XAP-HOME>/bin/insightedge host run-agent --manager --spark-master
```

## Starting Slave Nodes

Slave nodes consist of XAP containers and a Spark worker. On each slave node, run the following:

Use `--containers=n` to put XAP containers on a specific machine. If not specified, no XAP containers will be started.

```bash
<XAP-HOME>/bin/insightedge host run-agent --spark-worker [--containers=n]
```

After installation, you can verify that the Spark workers are up and running using the Spark master web UI at `http://your-master-ip-here:8080`.

# Deploying an Empty Space

```bash
#   topology 2,1 starts 2 primary partitions with 1 backup partition for each primary
<XAP-HOME>/bin/insightedge space deploy --partitions=2 --ha insightedge-space
```
