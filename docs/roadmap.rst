.. _roadmap:

Roadmap
=======

Below is a (non-exhaustive) list of the work that still needs to be done.

Global System
*************

 - Windows support
 - Logging
 - Ease distributed architecture deployments
 - Database support: Fix MySQL 5.5 issue, add PostgreSQL

Rest/S3 APIs
************

 - Unified proxy to access all gateways on one endpoint, including WebSocket & HTTP2 endpoints
 - Swagger documentation (swagger is used internally to generate the REST points)
 - Generate clients for various languages (from swagger)
 - S3 Gateway currently needs a specific header for authentication, move to Key/Secret instead

Security
********

 - Secure API's access
 - Implement SSL support for all services facing outside world
 - Split access logs and application logs
 - Display logs in UX

Synchronization
***************

 - New desktop sync clients based on the indexes built by the datasources
 - Configurable server-2-server sync for distributed architectures and backup

Authentication
**************

 - Add and test more connectors to external directories
 - Ability to use the OpenId Connect login page to third party applications

Scheduler
*********

 - Improve notifications in the UX
 - Ability to manage jobs in the UX
 - Document available actions