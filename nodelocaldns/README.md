## Kubernetes集群中使用NodeLocal DNSCache

### Install
```
curl -s -O https://raw.githubusercontent.com/kubernetes/kubernetes/master/cluster/addons/dns/nodelocaldns/nodelocaldns.yaml

sed -i -e "s/__PILLAR__DNS__DOMAIN__/${KUBE_DNS_NAME:-cluster.local.}/g" nodelocaldns.yaml
sed -i -e "s/__PILLAR__DNS__SERVER__/${KUBE_DNS_SERVER_IP:-10.68.0.2}/g" nodelocaldns.yaml
sed -i -e "s/__PILLAR__LOCAL__DNS__/${KUBE_LOCAL_DNS_IP:-169.254.20.10}/g" nodelocaldns.yaml

kubectl create -f nodelocaldns.yaml


//修改kubelet --cluster-dns=169.254.20.10

sed -i -e "s/--cluster-dns=${KUBE_DNS_SERVER_IP:-10.68.0.2}/--cluster-dns=${KUBE_LOCAL_DNS_IP:-169.254.20.10}/g" /etc/systemd/system/kubelet.service
systemctl daemon-reload
systemctl restart kubelet.service

```
