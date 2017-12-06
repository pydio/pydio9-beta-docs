Identity Management & LDAP Support
==================================

Understanding IDM services
**************************

Identity Manamement (IDM) is split accross 6 micro-services, that separately manage a single aspect of the application objects :

- **acl** (Access Control List) : flags that are attached to nodes to grant a specific permission
- **auth** (Authentication) : OpenID Connect authentication server, based on coreos/dex
- **role** : Pydio roles
- **share** : Metadata Provider service that gather info from ACL's to add shared metadata to nodes dynamically
- **user**: Pydio users and groups, represented as a tree.
- **workspace** : Simple metadata about Pydio workspaces.

These services are generally be accessed by GRPC and eventually provide a REST gateway for external clients management.

ACL provides the glue between nodes, roles and workspaces. For a given user, the workspace list is dynamically computed from all nodes that are seen as readable in the ACL list. As ACLs are attached to a workspace UUID, a workspace can now be composed of many root nodes. The "workspace" service provides just an additional layer of metadata for the workpsaces (label, description, owner, etc.).

Sharing is now much more simple : sharing a folder X with a user U is as simple as granting the read permission (Acl Action "read") on the folder node UUID for the user role. User will then see a new workspace with the folder as its root node.

LDAP Connection
***************

Unlike in the previous version of Pydio, where external directories where synced on-the-fly when a user logged in or when the admin was listing the users, Pydio 9 now provides an asynchronous synchronisation of external directory with the internal services (users and roles).

Configuring LDAP connection
...........................

TODO : Tran

Triggering a directory synchronization
......................................

TODO : Tran