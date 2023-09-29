# elastic-search set up(3 node cluster)

This is to configure elastic search on k8s for reproduction

Run the following command to install or upgrade the Elastic Exporter in the default namespace:

```bash
helm upgrade --install elasticsearch-exporter prometheus-community/prometheus-elasticsearch-exporter -f es-exporter.yaml-n default
```


For Elastic Master (Upstream):

Run the following command to install or upgrade the Elastic Master in the default namespace:



```bash
helm upgrade --install elasticsearch elastic/elasticsearch -f es-master.yaml -n default
```

Apply the StatefulSet configuration:
```bash
kubectl apply -f statefulset.yaml -n default
````

Add the following label to the headless master service:
```
kubectl label service elasticsearch-master-headless -n default consul.hashicorp.com/service-ignore=true
```

Manually restart the Elasticsearch masters to apply all settings. This action should bring up the 3-node cluster.


Once set up, you can now manually curl from es exporter and see this operates through consul's service mesh. You can use [kubectl debug](https://github.com/aylei/kubectl-debug) to exec into the exporter container
and curl.

exec using kubectl debug:

```bash
kubectl debug elasticsearch-exporter-prometheus-elasticsearch-exporter-6mzj7b -it  --image=nicolaka/netshoot -n default
```

curl
```bash
curl -vvv elasticsearch-master.<namespace>.svc.cluster.local:9200
```
