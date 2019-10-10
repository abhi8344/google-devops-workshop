# Logging Best Practices

## Stackdriver

Stackdriver has capability of parsing json logs, allowing you to search on properties contained within, lets take a look at two different implementations of nginx.

`kubectl apply -f nginx.yaml` 

`kubectl apply -f nginxjson.yaml`

Now that we have our services deployed, we will use a temporary container to send requests in.

`kubectl run --generator=run-pod/v1 tmp-shell --rm -i --tty --image nicolaka/netshoot -- /bin/bash`

It may take a moment to download the image, and start the container, follow any directions in the prompt.

Because we are in the same namespace, we can curl the nginx service directly.

`curl nginx`

Output:
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```


`curl nginxjson`

Output:
```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```


Lets take a look at the Stackdriver UI, to see the difference in capability.



## Forwarding Logs

Stdout Logs are forwarded by an agent on the cluster to some destination, based on what is setup.

Sometimes when multiple log files are necessary, there are soem strategies to get logs in.

1. Stream all logs to stdout in json format, so that you can differentiate log source when querying.
2. Add sidecar containers which can read from the log file and push to its stdout, which will get collected.
3. Write logs to a host volume and then write custom fluentd/agent logic to parse and forward those files.

