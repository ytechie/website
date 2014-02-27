---
title: Nitrogen Service
---

# Service

A service is the principal and messaging hub in the Nitrogen system.  It provides services for authenticating and authorizing principals, creating and finding messages and principals, and real time message subscription and routing for messages.  A typical device application using the JavaScript library looks like this:

```javascript

var config = {
  host: 'api.nitrogen.io',
  http_port: 443,
  protocol: 'https'  
};

var service = new nitrogen.Service(config);

service.connect(light, function(err, session, light) {
    if (err) return console.log('Failed to authenticate: ' + err);

    // your application here
});
```

The service class manages authentication and session management for you.  You provide it a callback function that describes the work you'd like it perform after a session is established.  Its possible that your session will fail.  The service class handles reconnection / reauthorization transparently for you and will call you back again when everything's a-ok again.

## Sessions

When a principal authenticates with the service, it is provided an access token for subsequent service requests. The client library automatically manages these details for you and encapsulates this in a session object for you. All of the client functions that interact with the service in the context of a principal take a session to use in that operation:

```javascript
nitrogen.Message.find(session, { to: camera.id }, function(err, messages) {
    // ...
});
```

The service can automatically renew a session as it nears the end of the AccessToken's lifetime.  This detail is again automatically handled for you by the Service and Session class.