# Hazelcast Node.js Client PCF Sample App

This sample app is based on the Pivotal Cloud Node.js Sample App at https://github.com/cloudfoundry-samples/cf-sample-app-nodejs

```$ cf push```

(See https://docs.cloudfoundry.org/buildpacks/node/node-tips.html)

After that, bind an existing Hazelcast service (e.g.,`hcmc1`) to the app and restart it.
```
$ cf bs cf-nodejs hzmc1
$ cf restage cf-nodejs
Restaging app cf-nodejs in org system / space system as admin...

Staging app and tracing logs...
...

Waiting for app to start...

name:              cf-nodejs
requested state:   started
instances:         1/1
usage:             512M x 1 instances
routes:            cf-nodejs-toadish-decolonisation.cfapps.example.com
last uploaded:     Fri 22 Dec 12:48:02 CET 2017
stack:             cflinuxfs2
buildpack:         node.js 1.6.4 (no decorators apply)
start command:     npm start

     state     since                  cpu    memory          disk          details
#0   running   2017-12-28T10:49:40Z   0.1%   37.6M of 512M   80.2M of 1G 
We confirmed that it was started correctly and noticed the route assigned to it, which is `cf-nodejs-toadish-decolonisation.cfapps.example.com`.
```
Take note of the route assigned to it, for example `cf-nodejs-toadish-decolonisation.cfapps.example.com`.
You can test put and get operations with `curl`.
```
First a put operation for a new key, returning null as expected.
$ curl -v 'http://cf-nodejs-toadish-decolonisation.cfapps.example.com/put?key=testKey1&value=testValue1'
*   Trying 35.186.234.125...
* TCP_NODELAY set
* Connected to cf-nodejs-toadish-decolonisation.cfapps.example.com (35.186.234.125) port 80 (#0)
> GET /put?key=testKey1&value=testValue1 HTTP/1.1
> Host: cf-nodejs-toadish-decolonisation.cfapps.example.com
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Content-Length: 4
< Content-Type: application/json; charset=utf-8
< Date: Thu, 28 Dec 2017 10:58:02 GMT
< Etag: W/"4-K+iMpCQsduglOsYkdIUQZQMtaDM"
< X-Powered-By: Express
< X-Vcap-Request-Id: 474f9742-2c1c-47d9-5ee4-1399b30cb527
< Via: 1.1 google
< 
* Connection #0 to host cf-nodejs-toadish-decolonisation.cfapps.example.com left intact
null
Then a get for that same key, returning the test value that we expected.
$ curl -v  'http://cf-nodejs-toadish-decolonisation.cfapps.example.com/get?key=testKey1'
*   Trying 35.186.234.125...
* TCP_NODELAY set
* Connected to cf-nodejs-toadish-decolonisation.cfapps.example.com (35.186.234.125) port 80 (#0)
> GET /get?key=testKey1 HTTP/1.1
> Host: cf-nodejs-toadish-decolonisation.cfapps.example.com
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Content-Length: 11
< Content-Type: application/json; charset=utf-8
< Date: Thu, 28 Dec 2017 11:01:23 GMT
< Etag: W/"b-tLvHxCJXky/5rwaD7k1YLFxaa60"
< X-Powered-By: Express
< X-Vcap-Request-Id: 362211eb-c5e5-49b0-53ce-332bb41fcb62
< Via: 1.1 google
< 
* Connection #0 to host cf-nodejs-toadish-decolonisation.cfapps.example.com left intact
"testValue1"
