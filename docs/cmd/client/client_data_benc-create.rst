.. _client_data_benc-create:

client data benc-create
-----------------------

Client method for benchmarking node creates.

Synopsis
~~~~~~~~


Client method for benchmarking node creates.

::

  client data benc-create [flags]

Options
~~~~~~~

::

      --batch int        Floods by group of n
      --flood int        Floods of n create nodes
      --flood-soft int   Floods one after one
  -h, --help             help for benc-create
      --output string    result output file (default "1512555083")
      --path string      Path to the data
      --uuid string      UUID of the data

Options inherited from parent commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

      --registry string   Registry used to manage services (default "nats")
      --source string     Source where the data resides

SEE ALSO
~~~~~~~~

* :ref:`client data <client_data>` 	 - Commands for managing indexed data

*Auto generated by spf13/cobra on 6-Dec-2017*
