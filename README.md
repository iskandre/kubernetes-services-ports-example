\# kubernetes-services-ports-example

### Deploying configuration with a pod nginx-welcome-test and test-service-nginx-welcome service. 
The service sends the requests to port 80 and the pod listens to the same port 80. 

The service is exposed on the port 8030 so other pods can communicate with this server on this port.

```kubectl apply -f nginx-welcome-test-config.yaml```


### Сhecking that everything is successfully running and checking parameters
```kubectl describe service test-service-nginx-welcome```

```kubectl describe pod nginx-welcome-test ```

### creating a test pod in the same cluster to check the connection inside the cluster
```kubectl run -i --tty alpine-testpod --image=ubuntu --restart=Never -- sh ```

```pip install requests```

```python3```

```r = requests.get('http://test-service-nginx-welcome:8030')```

When checking the response ```r.text``` You should see the Welcome to nginx! page.

### Deleting testing pod
```kubectl delete pod alpine-testpod```

### SSHing into container
The container has a name nginx-container in nginx-welcome-test in yaml file
Loging inside the container

```kubectl exec --stdin --tty nginx-welcome-test -c nginx-container -- sh```

### Checking all open ports inside the container

```netstat -tulpn | grep LISTEN```

You will that one of them in the list will have the open port 80 that was specified as a target port in our yaml file

Calling ```curl http://0.0.0.0:80``` should output our nginx welcome page

### Accessing from outside the cluster

Checking the info about the pod

```kubectl describe pod nginx-welcome-test ```

There you should see the line with the 
```Node:         gke-cluster-cloud-run--default-pool-2-XXXXXXX/X.X.X.X```
That's how you know the instance where this pod is deployed. This info shows the name of the instance and the internal IP. You can easily find the external IP for accessing outside.

Similar thing for the service

```kubectl describe service test-service-nginx-welcome```

And you see the line with NodePort like this
```NodePort:                 <unset>  30036/TCP```

So know you can browser this page from outside by the combination **external_ip:node_port** and you will see the same nginx welcome page


### When you're done with testing, you can delete the pod and the service

```kubectl delete pod nginx-welcome-test```

```kubectl delete service test-service-nginx-welcome```
