Scale-out on multiple nodes
===========================

Disclaimer: this part is currently untested. Welcome to the jungle!

Nats Discovery
**************

As any micro-service architecture, Pydio relies on a shared discovery mechanism so that services can talk together. By default it ships with nats and consul and Pydio is able to actually start the discovery service when running on a single node.

To run services on multiple nodes, it is first required to set up the cluster, by e.g. deploying NATS.io on the various nodes and let them know about each other. See http://nats.io/documentation/server/gnatsd-cluster/ for more info.

Starting without nats
*********************

To start one or more pydio services without another instance of nats server (as you already run your own, right?), just use the --exclude (-x) flag when using the pydio command line :

.. code:: bash

    pydio start --exclude=pydio.grpc.discovery


Sharing storages
****************

The micro-service architecture implies at its core that data persisted by one service will never be accessed by anything else by this service API. The default Pydio configuration will use one default database for all services, but you can easily modify the configurations to "shard" all data accross multiple databases. For example, DataSources can use their own database, IDM services can store Users, Roles, ACLs, etc. in separate databases, etc.

That said, when scaling one service horizontally by starting as many node as required, the service instances will still require an access to a common storage. Some services are using a specific file-based database (BoltDB) and cannot currently be scaled out. In some case, if these services are under heavy fire from the frontend, scaling out the gateway (a small layer translating REST requests to GRPC) can improve performances.

TODO: list of services using Bolt.