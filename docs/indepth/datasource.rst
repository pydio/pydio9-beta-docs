Datasources and indexation
==========================
A Datasource provides access to data. It continuously listens and stores changes to maintain a consistent data index.

What is a datasource?
*********************

.. image:: ../img/datasource.svg
    :target: ../images/datasource.svg
    :alt: datasource

Internally datasource is composed of (see the image below):

- **A storage service** : Provides access to the data and can be accessed by any tool that talks Amazon S3 protocol.

- **An index service** : Stores the data state and stands as the only source of truth for request on data state.

- **A synchronizer** : Maintains the storage and the index database synchronized.

Every time a datasource has it state updated, the index service publishes events to notify the other services.

.. image:: ../img/pydio-data.svg
    :target: ../images/pydio-data.svg
    :alt: datasource in Pydio environment


The tree server
***************
The tree service aggregates all the datasources and presents the whole as a single data tree in which each datasource is a child of the root node.

.. image:: ../img/tree-service.svg
    :target: ../images/tree-service.svg
    :alt: datasource tree
