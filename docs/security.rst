Security Considerations
=======================

Please read carefully, we will never repeat it enough: this tech preview is currently **not safe for production use**. Reasons are exposed below.

Frontend SSL Support
********************

WebSocket
.........

Pydio9 PHP frontend is deployed as a normal vhost in your preferred webserver. As such, you could configure a certificate to use SSL connection. However, the WebSocket service provided by the backend is now a central piece of the javascript frontend. And in this preview, the service does not yet support the configuration of an SSL certificate. For this reason, please **stick to http for the moment**, otherwise the https page will not connect to the ws:// (instead of wss://) websocket connection.

S3 Api
......

This is the same for the s3 gateway. Based on standards, the main API to manage files in Pydio9 is following the Amazon S3 protocol. Although most requests are proxied by the PHP to the JS, **downloads and uploads are using the presigned-url scheme** to target directly our s3 backend instead of going through PHP. Until we don't provide an easy way to configure SSL on this S3 api, frontend must stick to http.

This will of course be changed in the next phases of development.

API Securisation
****************

The backend REST API's that are currently queried by the php frontend are authenticated in the send that the frontend does identify itself using a JWT (JSON Web Token). This JWT is delivered by the **auth** service using OpenIDConnect protocol (built on **OAuth2**).

But as of now, some REST api's have to allow anonymous connection as the frontend is querying them at load time, before any user is logged in. This will be changed to allow only some very specific requests to be sent anonymously.