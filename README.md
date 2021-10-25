# kubernetes-services-ports-example

# Deploying configuration with a pod nginx-welcome-test and test-service-nginx-welcome service. The service sends the requests to port 80 and pod listening to # # the same port 80. The service is exposed on the port 8030 so other pods can communicate with this server on this port.
kubectl apply -f nginx-welcome-test-config.yaml
# to check that everything is successfully running and parameters
kubectl describe service test-service-nginx-welcome
kubectl describe pod nginx

creating a test pod in the same cluster to check the connection inside the cluster
kubectl run -i --tty alpine-testpod --image=ubuntu --restart=Never -- sh 
python3
pip install requests
r = requests.get('http://test-service-nginx-welcome:8030')
r.text -> Welcome to nginx!
kubectl delete pod alpine-testpod
we called the container nginx-container in nginx-welcome-test in yaml file
lets ssh inside the container
kubectl exec --stdin --tty nginx-welcome-test -c nginx-container -- sh
then
netstat -tulpn | grep LISTEN
one of them will be the open port 80
curl http://0.0.0.0:80 -> our nginx welcome page

kubectl describe pod nginx
Taking the info Node:         gke-cluster-cloud-run--default-pool-2-8049c71d-swxd/10.132.0.40 (taking external IP)
then 
kubectl describe service test-service-nginx-welcome
Taking the info NodePort:                 <unset>  30036/TCP

in our external machine checking: external_ip:node_port -> nginx welcome page

kubectl delete pod nginx-welcome-test
kubectl delete service test-service-nginx-welcome
