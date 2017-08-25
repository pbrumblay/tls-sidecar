# tls-sidecar
TLS/SSL sidecar for terminating HTTPS w/i k8s clusters

You can use this repository to demo a sidecar which can terminate TLS connections. This demo makes use of the kubernetes service type of type LoadBalancer, so it assumes that your cluster supports the automatic creation of load balancers. 

To use:
```
# create a self-signed certificate
openssl req -x509 -newkey rsa:2048 -keyout tls.key -out tls.crt -nodes -subj '/CN=echo-server'
```

```
# create a TLS secret
kubectl create secret tls echo-tls --cert=tls.crt --key=tls.key
```

```
# clone this repository and load up the pbrumblay/tls-sidecar and pbrumblay/echo-server pods.
git clone https://github.com/pbrumblay/tls-sidecar
kubectl apply -f tls-sidecar/kubernetes/deployment.yaml
```

```
# Test it out. 
kubectl get svc echo-server
NAME          CLUSTER-IP     EXTERNAL-IP      PORT(S)         AGE
echo-server   10.3.250.228   <REDACTED>   443:31619/TCP   10m

# Now with the new IP you can verify the echo-server works.
curl -k https://<REDACTED>/lorem
```

Note that we're using curl's "-k" switch to disable certification validation. You'll want to use a real certificate for real-world use cases.
