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

Connecting external directory
*****************************

Unlike in the previous version of Pydio, where external directories where synced on-the-fly when a user logged in or when the admin was listing the users, Pydio 9 now provides an asynchronous synchronisation of external directory with the internal services (users and roles).

Authentication Connectors
.........................

In Pydio 9, authentication is done by passing a set of connectors in charge of authenticating a user with her credential. This is an ordered list and you can add many connectors as you want. The configuration is located at "pydio.grpc.auth" >> "connectors" section in pydio.json file. In the top level of "connectors" section, we defined one 'parent' connector named "Pydio Aggregation Connector" to contain others connectors such as ldap, pydio-api. This connector is fixed, do not modify it. Going to deeper level, you will see "pydioconnectors". This is place that you can add/remove connectors by yourself.

.. code:: json

    "connectors": [
      {
        "type": "pydio",
        "id": "pydio",
        "name": "Pydio Aggregation Connector",
        "config": {
          "pydioconnectors": [
            {
              "type": "pydio-ldap",
              "name": "connector_01",
              "id": 3,
              "config": {}
            },
            {
              "type": "pydio-ldap",
              "name": "connector_02",
              "id": 2,
              "config": {}
            },
            {
              "type": "pydio-api",
              "name": "pydioapi",
              "id": 1
            }
          ]
        }
      }
    ]

The configuration of a connector has 4 items:

- **type**: Currently there are three types available [pydio-api, pydio-ldap, pydio-mysql].
- **name**: The name is required to distinguish the users between connectors.
- **id**: when user authenticates with her credential, the connector with higher ID will be used. If the authentication fails, user's credential will be passed to next connector in the list until it reach success or failure at last connector - pydio-api. The id of pydio-api should be 1 - the last one.
- **config**: the structure of this value will change depending on the connector type. Type pydio-api does not require any config, but pydio-ldap needs a complex config (see below).

Configuring LDAP connection
...........................

The config of pydio-ldap connector has three sections:

- General information for ldap server and schema
- A set of rules for mapping user's attributes in ldap to pydio user's attribute. The 'LeftAttribute' defines the name of attribute of external source such as ldap or other sql-base authentication. The 'RightAttribute' is the name of attribute in Pydio such as 'Roles', 'displayName', 'email', 'GroupPath'
- Mapping options: some option supports mapping process.

Here is a sample config below:

.. code:: json

    "connectors": [
    {
      "type": "pydio",
      "id": "pydio",
      "name": "Pydio Aggregation Connector",
      "config": {
        "pydioconnectors": [
          {
            "type": "pydio-ldap",
            "name": "pydioldap",
            "id": 2,
            "config": {
              "Host": "127.0.0.1:389",
              "Connection": "normal",
              "domainname": "example.org",
              "SkipVerifyCertificate": true,
              "RootCA": "",
              "RootCAData": "",
              "BindDN": "cn=admin,dc=example,dc=org",
              "BindPW": "P@ssw0rd",
              "PageSize": 500,
              "SupportNestedGroup": false,
              "ActivePydioMemberOf": false,
              "UserAttributeMeaningMemberOf": "memberOf",
              "GroupValueFormatInMemberOf": "dn",
              "GroupAttributeMeaningMember": "member",
              "GroupAttributeMemberValueFormat": "dn",
              "User": {
                "IDAttribute": "uid",
                "DNs": [
                  "ou=staff,ou=people,dc=example,dc=org"
                ],
                "Filter": "(objectClass=inetOrgPerson)",
                "Scope": "sub"
              },
              "Group": {
                "IDAttribute": "cn",
                "DNs": [
                  "ou=groups,dc=example,dc=org"
                ],
                "Filter": "(objectClass=groupOfNames)",
                "Scope": "sub",
                "DisplayAttribute": "cn"
              }
            },
            "mappingrules": [
              {
                "LeftAttribute": "displayName",
                "RightAttribute": "displayName"
              },
              {
                "LeftAttribute": "memberOf",
                "RightAttribute": "Roles"
              },
              {
                "LeftAttribute": "mail",
                "RightAttribute": "email"
              }
            ],
            "mappingoptions": {
              "AuthSource": "pydioldap",
              "RolePrefix": "ldap_"
            }
          },
          {
            "type": "pydio-api",
            "name": "pydioapi",
            "id": 1
          }
        ]
      }
    }
    ]

Triggering a directory synchronization
......................................

After adding an external connector to Pydio, the external user still cannot login. You should execute a command in Pydio to import users form external source to Pydio. Depending on the number of users you have in ldap, the command make take several minutes to finish.

To trigger this command in the Pydio Scheduler, use the client binary delivered with the installation. It will add a job in the scheduler, that will start right away. The job owner is hardcoded as "admin", so if you have a local user named "admin" who is logged in, you should see the progress appear in the frontend.

.. code:: bash

    ./client jobs sync-users
