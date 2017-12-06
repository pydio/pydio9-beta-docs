Troubleshooting
===============

You may encounter some issues at start up, that can be hard to troubleshoot due the massive amount of logs outputted by the services. If it seems to start but you see thing disfunction in the frontend, some tips&tricks below. Please make sure to read through these before turning to the team for help.

Authentication Issues
.....................

Symptoms
~~~~~~~~
 - I cannot login
 - Cannot connect to Dex API errors
 - JWT errors.

Checks
~~~~~~
Auth service may not be properly started.

 - Check that the port (5556 by default) is not occupied by another process
 - Check that your MySQL database is version 5.6 or upper. The service will fail creating tables otherwise.
 - Check that your ClientID/ClientSecret are properly configured, look in the backend pydio.json file (look for staticClients section), and in the frontend bootstrap.json file.
 - Hum, check you have the correct username and password...


DataSource not correctly launched
.................................

Symptoms
~~~~~~~~
 - When trying to create a workspace that point to the root of your datasource (e.g. pydiods1), you see an API error flash, and if you save and reopen the workspace editor, path is empty.
 - I can list my files, but cannot download anything, and see no thumbnails for images
 - I cannot upload anything, upload blocks.
 - I cannot add an avatar to my user account (error when saving).

Checks
~~~~~~
The underlying s3 server is not properly started/configured.

 - Make sure that the datasource folder is NOT empty, it must contain at least one non-empty file to be correctly detected.
 - Try to stop and restart the backend, a couple of times.
 - If still no chance, try to search for an error in the logs near "pydio.grpc.data.objects.yourdatasourcename".
 - Make sure that the datasource name is not too short, it must be at least 8-characters long.
 - Make sure that the port configured (9001) is not occupied by another process.
 - Make sure that the gateway port (9020) is not occupied by another process.
 - Make sure the disk is not full or near to be full (you should see clear errors in the logs).

Websocket
.........

Symptoms
~~~~~~~~
 - Whenever I do a file/folder change, it is not reflected in the interface unless I reload the list manually

Checks
~~~~~~
Websocket server is not connected

 - Reload the page if it was open during a backend reload
 - Make sure the default port (5050) is not occupied by another process
 - Make sure the default port is accessible to the outside world (by the browser)
 - Look in the Developer Tools / Network tab in your browser, check if you see anything in the WS section.

Indexation Issues
.................

Symptoms
~~~~~~~~
 - If i move / rename a deep folder, I end up in a unexpected listing state
 - If i move/rename a folder, nothing happens, when I reload the list

Checks
~~~~~~
 - There might be an indexation issue. We are still working on strengthening that (vital) part. You can trigger a datasource re-indexation in the Settings, via Workspaces > Manage Datasources
 - Folder moves are managed in background by a scheduler task. Scheduler service may not have been started properly. Perform a 'pydio list' command and check that the scheduler related services are started.

