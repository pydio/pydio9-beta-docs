.. _client_acl_delete:

client acl delete
-----------------

Remove one or more ACLs

Synopsis
~~~~~~~~


Remove one or more ACLs by querying the ACL api.

Flags allow you to query the grpc service for deleting the resulting ACLs

'


::

  client acl delete [flags]

Options
~~~~~~~

::

  -a, --action stringArray         Action value
  -h, --help                       help for delete
  -n, --node_id stringArray        NodeIDs
  -r, --role_id stringArray        RoleIDs
  -w, --workspace_id stringArray   WorkspaceIDs

Options inherited from parent commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

      --registry string   Registry used to manage services (default "nats")

SEE ALSO
~~~~~~~~

* :ref:`client acl <client_acl>` 	 - Manage Access Control List

*Auto generated by spf13/cobra on 6-Dec-2017*
